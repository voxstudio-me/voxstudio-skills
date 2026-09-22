---
name: mcp-dubbing
description: Generate and preview voiceover audio in VoxStudio using saved reference voices through MCP. Use for text-to-speech dubbing or auditioning a script in a chosen voice.
---

# Dubbing through MCP

Use the running VoxStudio MCP server from Cursor, Codex, or Claude. No video project is needed. Discover tool schemas and call `voice.list` to resolve the user's selected voice to a real UUID. If no suitable voice exists, create one with `voice.create` from a 3–30 second reference and its exact transcript; obtain missing reference metadata from the user. Do not silently substitute another speaker.

Call `dubbing.create` with `text`, `voice_id`, optional `language` and `title`. It creates a new local dubbing session. The language defaults to the reference language; choose a supported language matching the requested script. This synthesizes the supplied words; translate the script first only when the user requested translation.

Save `session_id` and poll `media.status` every few seconds until completed/failed/cancelled. Do not resubmit because synthesis is slow. Local voice models must be available in VoxStudio. Report actionable model or generation errors and retry only after fixing them. An uncertain response should be checked against existing sessions before creating a duplicate.

On completion use `media.preview` with the session ID, optional `start`, and `duration` (default 15 seconds; maximum 30). Inspect the actual `audio_path` and timed cues. `play: true` auditions the clip in VoxStudio; `open: true` shows the session. `media.status.audio_path` is the full generated output; the preview path is a bounded excerpt. Give the user the actual result path and session title, and say which range was previewed. Do not claim to have listened if only metadata was inspected.

Example:
```json
{"text":"Welcome to our workshop. Today we will practice clear communication.","voice_id":"UUID returned by voice.list","language":"en","title":"Workshop intro"}
```

These tools are available through external MCP. An in-app agent without them should explain how to run this workflow with a connected client rather than inventing results.
