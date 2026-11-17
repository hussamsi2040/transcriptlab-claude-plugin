# TranscriptLab for Gemini CLI

This extension connects Gemini CLI to the TranscriptLab MCP server at
`https://api.transcriptlab.dev/mcp`. It adds transcript tools only; there are no
local scripts, no binaries and nothing to build.

## What it adds

- YouTube transcripts from existing caption tracks, in the language you choose
- Caption-track discovery before you fetch anything
- Speech-to-text for attached audio/video files and direct media links
- Search inside transcripts you already pulled
- Your TranscriptLab library, usage and credit balance
- Export links for TXT, SRT and VTT

## Sign in

The first tool call opens a TranscriptLab consent page in your browser. Sign in
with a TranscriptLab account, or paste an API key. The server supports OAuth with
dynamic client registration, so there is no client ID to configure.

Tokens are scoped `transcriptlab:read` and `transcriptlab:write`, last one hour
and refresh automatically.

## Notes

- A YouTube video without captions is not transcribed automatically. Use the
  speech-to-text tools with the media file or a direct media URL instead.
- The first delivery of a transcript variant may spend credits. Searching a
  transcript you already have, checking a job, listing your library and checking
  usage do not.
- Everything goes to `https://api.transcriptlab.dev`. No other host is contacted.

Docs: https://transcriptlab.dev/docs
