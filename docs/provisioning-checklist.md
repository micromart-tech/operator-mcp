# Provisioning checklist (internal)

Run this once per beta operator before sending the invite email. Requires a
Super admin.

## Per operator

1. **User row** on `classic.micromart.com`:
   - [ ] `portal_operator_id` set to their operator account
   - [ ] `enterprise-api` role granted (add `enterprise-api-write` only if a
         write tool ever ships; beta is read-only)
   - [ ] `beta-feature-enterprise-api` tag (unlocks the API docs page)
   - [ ] Email address on file (required for OAuth authorization)
2. **Classic login works.** The OAuth consent screen bounces to the classic
   `/login` page. If they've never used it, send a password reset first and
   confirm they can sign in.
3. **Send the invite** from `docs/invite-email-template.md`, matching their
   client, attaching `dist/micromart-analysis.zip` for Claude users.
4. **Log who was invited** (operator name, user email, client, date) so
   reconnects at GA can reach everyone.

## Before operator #1 (do once)

- [ ] Full OAuth flow end to end with a test operator account in ChatGPT dev
      mode and in Claude web. Confirms consent screen, token issuance, and
      tool calls against live data.
- [ ] Freeze the canonical URL. Tokens are bound to
      `classic.micromart.com/api/enterprise/v1/mcp`; do not change the host or
      path during the beta or everyone re-onboards.
- [ ] Decide how feedback lands (reply to invite email, shared channel, or a
      tracking doc).

## Revoking access

- Remove the `enterprise-api` role (or the `portal_operator_id`). Access dies
  within an hour (access token TTL) and refresh stops working immediately.
- The user can also disconnect on their side in the app's connector settings.

## Known limits during beta

- OAuth client registrations issued via dynamic registration expire after 90
  days. If the beta runs past ~90 days from first invite, plan a reconnect or
  extend the DCR client TTL server-side.
- `GET` and `DELETE` on the MCP endpoint return 405 by design (stateless
  Streamable HTTP, POST only).
