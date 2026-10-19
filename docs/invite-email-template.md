# Beta invite email template

Copy the section matching the operator's client, attach
`dist/micromart-analysis.zip` for Claude users, and delete the rest. Fill in
`<name>` before sending.

---

Subject: You're invited: ask your Micromart data questions in ChatGPT / Claude

Hi <name>,

You're in the first group of operators invited to the Micromart AI beta. It
connects your Micromart account to ChatGPT or Claude, so you can ask questions
about your stores in plain English: sales trends, top products, stockouts,
restock priorities, planograms.

It is read-only. It can never change prices, planograms, or anything else in
your account, and it only sees your own operator account.

Setup takes about a minute. Pick your app:

## If you use ChatGPT (paid plan required)

If your company has a ChatGPT Business, Enterprise, or Edu workspace, forward
this email to your workspace admin instead. They can publish it for your whole
team:

- Workspace settings -> Apps/Plugins -> Add custom MCP server
- Name: Micromart
- Server URL: https://classic.micromart.com/api/enterprise/v1/mcp
- Authentication: OAuth
- Publish. Each teammate clicks Connect and signs in with their Micromart
  credentials.

On a personal plan (Plus / Pro):

1. Settings -> Apps & Connectors -> Advanced -> enable Developer mode
2. Apps/Plugins -> + -> Add custom MCP server
3. Name: Micromart, Server URL: https://classic.micromart.com/api/enterprise/v1/mcp,
   Authentication: OAuth
4. Sign in with your Micromart credentials on the consent screen

## If you use Claude (any plan, including free)

1. Customize -> Connectors -> + -> Custom -> Web
2. Name: Micromart, Server URL: https://classic.micromart.com/api/enterprise/v1/mcp
   -> Add -> Connect -> sign in with your Micromart credentials

If your company uses Claude Team or Enterprise, your org owner can add it once
under Organization settings -> Connectors, and everyone connects from their own
account.

Optional but recommended: the attached micromart-analysis.zip teaches Claude
which Micromart tool to use for each question. Upload it under Customize ->
Skills (requires "Code execution and file creation" turned on in Capabilities).

## Things to try first

- "How were sales last week versus the week before?"
- "Which of my stores are underperforming this month?"
- "Which cabinets should I restock first today?"
- "What are my top products at <store name>?"

## Notes

- Read-only: nothing you ask can change your account.
- Your data only: the connector is scoped to your own operator account.
- Beta: the server URL will move to api.micromart.com at general availability;
  we'll ask you to reconnect then, which takes about a minute.
- To disconnect: remove the connector in your app's settings, or reply and
  we'll revoke access.

Reply to this email with feedback, issues, or questions.
