---
name: mcp-voice-reference
description: Create and preview reusable VoxStudio reference voices from short recordings through MCP. Use when the user wants to add a voice to the voice library for later dubbing.
---

# Reference voices through MCP

Use VoxStudio's running MCP server from an external client. No video project is needed. Discover available schemas, then inspect `voice.list` to reuse a voice when the user identifies an existing one.

`voice.create` requires `path`, `name`, `transcript`, `language`, and `gender` (female/male/child, from user-provided metadata). Use an audible 3–30 second clip of a single speaker. The transcript must match the whole reference clip exactly; do not invent a script or pass words from outside the clip. If the source is longer, choose a suitable passage and create a separate clip without changing the original. If its words are unknown, transcribe that passage first, inspect its timed cues, and match the reference clip to them. Ask only for metadata that cannot be obtained from the user's request; do not infer gender from voice alone.

The saved reference has a `voice_id`, duration, language, transcript and managed `audio_path`. Keep that ID for `dubbing.create`. `voice.preview` returns the reference metadata and file path; use `play: true` when the user requests auditioning. Verify the returned duration and words before using it for synthesis.

These tools are external MCP capabilities; do not pretend they are available to an in-app agent that lacks them. Creation writes a new library entry. A failed request should be corrected before retrying; list the library after an uncertain response to avoid duplicates. Do not change the default voice or delete existing voices unless requested.

Example: “Use this 10-second clip and its transcript to add a reference voice named Workshop narrator, then let me hear the saved voice.”
