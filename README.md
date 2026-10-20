# TranscriptLab for Claude

Turn YouTube videos and audio or video files into searchable, exportable transcripts without
leaving Claude. This plugin connects Claude to the [TranscriptLab](https://transcriptlab.dev) MCP
server and adds a skill that routes each request to the right tool.

## What you can ask

- "Get the transcript of https://www.youtube.com/watch?v=…"
- "Which subtitle languages does this video have?"
- "Find where they talk about pricing in that transcript."
- "Transcribe this attached recording." (audio or video file)
- "Transcribe https://example.com/interview.mp3."
- "Give me that transcript as an SRT file."
- "Show my earlier transcripts" and "How many credits do I have left?"

## Features

| Feature | What it does |
| --- | --- |
| YouTube transcripts | Fetches captions for a video link or ID, in the language you choose |
| Caption discovery | Lists the caption tracks a video offers before you fetch one |
| Speech-to-text | Transcribes attached audio or video files and direct media links |
| Search | Finds phrases inside transcripts you already pulled |
| Library | Reopens earlier transcripts from your TranscriptLab account |
| Export | Creates a download link (TXT, SRT, VTT and similar) valid for 10 minutes |
| Usage | Reports your plan, credits and monthly limits |

A YouTube video without captions is not transcribed automatically. Claude says so and offers
speech-to-text if you can attach the file or share a direct media link.

## Install

**Claude Code, as a plugin**

```bash
git clone https://github.com/hussamsi2040/transcriptlab-claude-plugin
claude --plugin-dir ./transcriptlab-claude-plugin
```

**Any MCP client, server only**

```bash
claude mcp add --transport http transcriptlab https://api.transcriptlab.dev/mcp
```

In claude.ai, add `https://api.transcriptlab.dev/mcp` as a custom connector. The server alone has
the tools; the plugin adds the skill that tells Claude which tool to use.

## Sign in

The first request opens a TranscriptLab consent page. Sign in with your TranscriptLab account, or
paste a TranscriptLab API key. No client ID or manual setup is needed, because the server supports
OAuth with dynamic client registration.

- Tokens are scoped `transcriptlab:read` and `transcriptlab:write`, last one hour, and refresh
  automatically.
- A connection made with an API key cannot create export links or upload files, because those tools
  need a signed-in session. Everything else works either way.
- If Claude asks you to reconnect, reconnect the connector once and retry.

## Credits and limits

Transcripts are metered in credits, and a first delivery of a transcript may use one. Searching a
transcript you already have, checking a job, listing your library and checking usage do not
deliver new transcripts. When you run out of credits or reach a monthly limit, Claude tells you
and stops instead of retrying. Plans are on [transcriptlab.dev](https://transcriptlab.dev).

## Privacy and data

- Everything the plugin does goes to `https://api.transcriptlab.dev`. Nothing else is fetched or
  contacted, and the skill runs no scripts and touches no local files.
- What is sent: the video links, attached files and media URLs you ask Claude to transcribe, and
  your API key if you choose to paste one on the consent page.
- Delivered transcripts are kept in your TranscriptLab library until you delete them.
- See the [privacy policy](https://transcriptlab.dev/privacy) and
  [terms](https://transcriptlab.dev/terms).

## Package contents

```
.claude-plugin/plugin.json      plugin manifest
.mcp.json                       remote MCP server declaration (streamable HTTP)
skills/transcriptlab/SKILL.md   routing and behavior instructions
assets/icon.png                 plugin icon
LICENSE                         MIT
```

## Support

- Docs: https://transcriptlab.dev/docs
- Issues: https://github.com/hussamsi2040/transcriptlab-claude-plugin/issues

## License

MIT, see [LICENSE](LICENSE).
