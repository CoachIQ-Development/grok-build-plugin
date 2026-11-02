# CoachIQ plugin for Grok Build

A remote-MCP-only plugin: `.mcp.json` points Grok Build at `https://mcp.coachiq.io/mcp`. Signing
in happens through CoachIQ's OAuth consent page on first use (Admin and Coach staff accounts only;
one connection per CoachIQ group).

Install once listed:

```bash
grok plugin install coachiq --trust
```

Without the marketplace, any Grok user can add the server at https://grok.com/connectors
(**New Connector → Custom**, paste the URL). Grok Business/Enterprise admins provision it once in
the cloud console (Grok → Connector management) before members can use it.

## Network endpoints and permissions

- The only network endpoint is `https://mcp.coachiq.io/mcp` (Streamable HTTP). Its OAuth
  authorization server is the same host (`/.well-known/oauth-authorization-server`,
  `/authorize`, `/token`, `/register`). Nothing else is contacted.
- Credentials: none are stored in the plugin. Grok Build registers itself as a public OAuth client
  (dynamic client registration, PKCE S256) and the user signs in on CoachIQ's consent page. Scopes:
  `coachiq:mcp:read` always; `coachiq:mcp:write` only if the user ticks it at consent.
- Permissions: the connection acts as the signed-in CoachIQ Admin or Coach, limited to the one
  CoachIQ group chosen at consent, re-authorized live on every call. Writes are confirmed per
  request and logged in CoachIQ under Settings > Logs.
- The plugin ships no code, hooks, skills or commands: only this MCP server configuration.

## License

MIT (see `LICENSE`). The CoachIQ service itself is governed by https://www.coachiq.io/terms.

## Submitting to the marketplace (CoachIQ maintainers)

The marketplace is the public repo https://github.com/xai-org/plugin-marketplace; listing is a
pull request reviewed by code owners.

1. Publish this folder as the root of a public GitHub repository (for example
   `CoachIQ-Development/grok-build-plugin`) and note the full 40-character commit SHA of the release commit.
2. Fork `xai-org/plugin-marketplace`, add an entry to `.grok-plugin/marketplace.json`:

   ```json
   {
     "name": "coachiq",
     "description": "Manage a CoachIQ coaching business from Grok Build: bookings, clients, forms, lessons, inbox, programs, website. Remote MCP server with OAuth sign-in.",
     "category": "productivity",
     "source": { "source": "url", "url": "https://github.com/CoachIQ-Development/grok-build-plugin.git", "sha": "<40-char commit sha of the release commit>" },
     "homepage": "https://github.com/CoachIQ-Development/grok-build-plugin",
     "keywords": ["coachiq", "coachiq mcp", "coachiq bookings"],
     "domains": ["coachiq.io"]
   }
   ```

   Field shape copied from the existing `vercel` entry (2026-11-02); categories in use are
   database, deployment, development, media, monitoring, observability, productivity, security.
   Keywords must be brand-specific (reviewers narrowed another plugin's generic terms).
3. Run `python3 scripts/generate-plugin-index.py`, `python3 scripts/validate-catalog.py` and
   `python3 scripts/generate-plugin-index.py --check`; commit the regenerated
   `.grok-plugin/plugin-index.json` with the catalog change (never hand-edit the index).
4. Open the PR using the repo template (What this PR does, Ownership, Checklist, Security, Notes
   for reviewers); state the endpoint and auth from the section above. Model: PR #1629 (Expo).
   Each new release needs a new PR bumping `sha`.
