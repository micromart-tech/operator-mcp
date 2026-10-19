# Micromart for AI assistants

**Private beta.** This connector is invite-only. Your Micromart account must be
provisioned for MCP access before you can connect. If you haven't received an
invite, contact your Micromart representative.

Ask questions about your Micromart smart stores in plain English: sales trends,
product rankings, stockouts, restock priorities, planograms. It works inside
ChatGPT or Claude in your browser, phone, or desktop app. All access is
read-only and limited to your own operator account.

## What you can ask

- "How were sales last week versus the week before?"
- "Which of my stores are underperforming this month?"
- "What are my top products at the downtown location?"
- "Which cabinets should I restock first today?"
- "What planogram changes are pending on cabinet M-1234?"

More examples: [prompts/cookbook.md](prompts/cookbook.md)

## Setup

You only need a name, a URL, and your Micromart sign-in. Everything below takes
about a minute.

| Field | Value |
| --- | --- |
| Name | `Micromart` |
| Server URL | `https://classic.micromart.com/api/enterprise/v1/mcp` |
| Authentication | `OAuth` |

> Beta note: the URL will move to `api.micromart.com` at general availability.
> When that happens you'll be asked to reconnect, which takes about a minute.

### ChatGPT

Requires any paid ChatGPT plan (Plus, Pro, Business, Enterprise).

**Workspace rollout (Business / Enterprise / Edu). Recommended for teams.** A
workspace admin sets it up once for everyone:

1. Open workspace settings → Apps/Plugins → **Add custom MCP server**.
2. Use the values from the table above, Authentication: **OAuth**.
3. Publish to the workspace. Members see "Micromart" in their Apps list and
   click **Connect** to sign in with their Micromart credentials. No manual
   setup needed.

**Individual setup (Plus / Pro, developer mode):**

1. Settings → Apps & Connectors → Advanced → enable **Developer mode**.
2. Apps/Plugins → **+** → **Add custom MCP server**.
3. Name `Micromart`, paste the Server URL, Authentication: **OAuth**.
4. Sign in with your Micromart credentials on the consent screen.

### Claude

Works on every plan, including Free.

**Individual (Free / Pro / Max):**

1. Customize → Connectors → **+** → **Custom** → **Web**.
2. Name `Micromart`, paste the Server URL → **Add** → **Connect** → sign in with
   your Micromart credentials.

**Team / Enterprise:** an org **Owner** adds the connector under
Organization settings → Connectors → Add → Custom → Web (same values). Members
then find it under Customize → Connectors and click **Connect** to sign in with
their own account.

**Optional: the analysis skill.** `micromart-analysis.zip` teaches Claude which
Micromart tool to reach for and how to present results. It's attached to your
invite email (this repo is private, so it isn't downloadable here).

- Individuals: Customize → Skills → upload the zip.
- Team / Enterprise owners can upload it once under Organization settings →
  Plugins & skills to provision it for everyone.
- Requires "Code execution and file creation" enabled in Capabilities.

## Access & security

- **Read-only.** The connector can never change prices, planograms, or anything
  else in your account.
- **Your data only.** Access is scoped to your own operator account, enforced
  server-side per signed-in user.
- **Revocable.** Disconnect the connector on your side at any time, or ask
  Micromart to revoke access.

## Feedback

Reply to your invite email or contact your Micromart representative. The tool
list is growing during the beta. See [CHANGELOG.md](CHANGELOG.md).

---

## For technical staff

Everything below is for engineers (yours or ours), not required for operators.
These packages are how the connector and the analysis skill get installed in
developer agents, and they're ready-made for the public directories at GA.

Installable one-command packages:

```sh
# Claude Code (MCP server + micromart-analysis skill)
claude plugin marketplace add micromart-tech/operator-mcp
claude plugin install micromart@micromart

# Codex / ChatGPT desktop (same bundle)
codex plugin marketplace add micromart-tech/operator-mcp
# then pick "micromart" in the Plugins Directory

# Gemini CLI (MCP server + GEMINI.md context)
gemini extensions install https://github.com/micromart-tech/operator-mcp
```

This repo is private; your GitHub account needs access to run the commands
above.

### Bundling the plugin for ChatGPT workspaces

A custom MCP server created in ChatGPT is an app only; skills ride along when
it's packaged as a plugin. To bundle `micromart-analysis` for workspace users:

1. Create the custom MCP server in ChatGPT (steps above), then copy its
   technical ID from the browser URL. It starts with `plugin_asdk_app`.
2. Create `plugins/micromart/.app.json` in a checkout of this repo:

   ```json
   {
     "apps": {
       "micromart": {
         "id": "plugin_asdk_app_your_id_here"
       }
     }
   }
   ```

3. Install the plugin from this marketplace in the ChatGPT desktop app or
   Codex as above.

### Cursor

[Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=micromart&config=eyJ1cmwiOiJodHRwczovL2NsYXNzaWMubWljcm9tYXJ0LmNvbS9hcGkvZW50ZXJwcmlzZS92MS9tY3AifQ%3D%3D)

Or paste into `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "micromart": {
      "type": "http",
      "url": "https://classic.micromart.com/api/enterprise/v1/mcp"
    }
  }
}
```

### VS Code

[Install MCP server](vscode:mcp/install?%7B%22name%22%3A%22micromart%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A//classic.micromart.com/api/enterprise/v1/mcp%22%7D)

### Repo layout

```text
plugins/micromart/            installable plugin (Claude Code + Codex formats)
  .mcp.json                   MCP server config shared by both manifests
  skills/micromart-analysis/  analysis playbook skill
  assets/logo.png
.agents/plugins/              Codex marketplace catalog
.claude-plugin/               Claude Code marketplace catalog
gemini-extension.json         Gemini CLI extension (root of repo)
GEMINI.md                     Gemini context file
server.json                   MCP registry metadata, for GA
icon.svg                      connector icon
prompts/cookbook.md           example questions for operators
dist/micromart-analysis.zip   claude.ai skill upload
```
