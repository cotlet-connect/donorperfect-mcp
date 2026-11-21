# DonorPerfect MCP server for ChatGPT and Claude

Connect ChatGPT, Claude and any AI agent to your DonorPerfect account through Cotlet.dev. The hosted integration documents 14 operations for donors, gifts, pledges, contacts and thank-you emails.

## What can I ask?

> Find donors matching this deposit list and show ambiguous matches before entering gifts.

> Read this donor's gifts, pledges and contact history.

> Check the available codes and prepare pledge payments for my approval.

> Show gifts waiting for a receipt and prepare thank-you emails for my approval.

## Connect ChatGPT and Claude

1. [Get access](https://cotlet.dev/signup?app=donorperfect).
2. Open DonorPerfect in its Cotlet.dev Cloud Browser, sign in with your own account and enable it for agents. Complete any sign-in verification yourself.
3. Add `https://cotlet.dev/mcp/v1/connect` as the connector URL in ChatGPT or Claude and sign in with Cotlet.dev.

Codex, Claude Code and other clients using an API key connect to `https://cotlet.dev/mcp/v1`. Create a key in the Cotlet.dev console and configure your client:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

[Current setup and full DonorPerfect guide](https://cotlet.dev/integrations/donorperfect-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=donorperfect)

## Hosted operations

The live page documents these operations through `donorperfect_read` and `donorperfect_write`:

- `search_donors`
- `get_donor`
- `create_donor`
- `update_donor`
- `list_codes`
- `list_gifts`
- `add_gift`
- `list_receipts_due`
- `send_thank_you_email`
- `list_pledges`
- `create_pledge`
- `apply_pledge_payment`
- `list_contacts`
- `add_contact`

Reads run immediately. Six write operations ask for confirmation by default; `add_contact` logs an internal note immediately and sends nothing to a donor. Confirmation settings can be changed per tool. Search before creating a donor and check contact history before adding a contact to avoid duplicates.

Cotlet uses your own DonorPerfect login and its permissions. The tools do not cover card or EFT details, Monthly Giving, user settings, imports, reports or mail merge. Sending a thank-you email cannot be undone.

## Run the local stdio bridge

This repository includes a Python bridge to Cotlet.dev. It requires Python 3.10 or newer and uses the standard library.

```bash
export COTLET_API_KEY="your-key"
python3 donorperfect_agent.py serve
```

The local bridge exposes seven browser helper tools: `donorperfect_guide`, `donorperfect_status`, `donorperfect_open`, `donorperfect_read`, `donorperfect_click`, `donorperfect_type` and `donorperfect_close`. Use the hosted connector for the application operations listed above.

```bash
docker build -t donorperfect-mcp .
docker run -i --rm -e COTLET_API_KEY donorperfect-mcp
```

Without `COTLET_API_KEY`, a tool call returns instructions for obtaining a key. Keep the key out of source control.

## Does DonorPerfect have an API?

The live guide's check dated 27 October 2026 describes DonorPerfect's XML API for donors, gifts, pledges and contacts, with Custom API Access listed on Pro. The guide found no official DonorPerfect MCP server. Cotlet uses the signed-in browser rather than that XML API. Use DonorPerfect's own integrations first when they cover your task.

## Is this affiliated with DonorPerfect?

Cotlet.dev is independent and is not affiliated with, endorsed by or sponsored by DonorPerfect or SofterWare. DonorPerfect is a trademark of its owner. You use your own account and its existing plan and permissions.

[Get access](https://cotlet.dev/signup?app=donorperfect)
