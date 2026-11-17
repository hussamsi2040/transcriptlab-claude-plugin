# Installing TranscriptLab in Cline

TranscriptLab is a **remote** MCP server over Streamable HTTP. There is nothing to
install locally: no npm package, no Docker image, no API key required to start.

- Endpoint: `https://api.transcriptlab.dev/mcp`
- Transport: Streamable HTTP
- Auth: OAuth 2.0 with dynamic client registration (a browser consent page opens on
  the first tool call). An API key also works if you prefer a static header.

## Install with the Cline CLI

```bash
cline mcp add transcriptlab https://api.transcriptlab.dev/mcp --transport http --yes
```

## Install from the Cline UI

1. Open the MCP Servers panel and choose **Remote Servers**.
2. Name it `transcriptlab`.
3. Transport: **Streamable HTTP**.
4. URL: `https://api.transcriptlab.dev/mcp`.
5. Save, then approve the OAuth consent page that opens in your browser.

## Install by editing the settings file

Add this entry to `cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "transcriptlab": {
      "type": "streamableHttp",
      "url": "https://api.transcriptlab.dev/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

To use an API key instead of OAuth, add:

```json
"headers": { "Authorization": "Bearer ytgw_your_key_here" }
```

API keys cannot create export links or upload files, because those tools need a
signed-in session. Every other tool works.

## Verify

Ask Cline: *"Get the transcript of https://www.youtube.com/watch?v=dQw4w9WgXcQ"*.
A successful call returns the transcript text. If Cline reports an auth error,
reconnect the server and finish the consent page.

## What the tools do

| Tool | Purpose |
| --- | --- |
| `transcriptlab_get_transcript` | YouTube transcript from captions |
| `transcriptlab_discover_captions` | List available caption tracks |
| `transcriptlab_search_transcript` | Search a transcript you already pulled |
| `transcriptlab_transcribe_file` | Speech-to-text for an attached file |
| `transcriptlab_transcribe_media_url` | Speech-to-text for a direct media URL |
| `transcriptlab_list_transcripts` | Your transcript library |
| `transcriptlab_get_usage` | Plan, credits and limits |
| `transcriptlab_export_link` | TXT / SRT / VTT download link (session only) |

Full list: 19 tools, documented at https://transcriptlab.dev/docs

## Notes

- A YouTube video without captions is not transcribed automatically.
- The first delivery of a transcript variant may spend credits; re-reads of the
  same variant are free.
- Nothing is contacted except `https://api.transcriptlab.dev`.
