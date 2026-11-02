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

## Submitting to the marketplace (CoachIQ maintainers)

The marketplace is the public repo https://github.com/xai-org/plugin-marketplace; listing is a
pull request reviewed by code owners.

1. Publish this folder as the root of a public GitHub repository (for example
   `CoachIQ-Development/grok-build-plugin`) and note the full 40-character commit SHA of the release commit.
2. Fork `xai-org/plugin-marketplace`, add an entry to `.grok-plugin/marketplace.json`:

   ```json
   {
     "name": "coachiq",
     "source": { "source": "url", "url": "https://github.com/CoachIQ-Development/grok-build-plugin", "sha": "<40-char commit sha>" },
     "description": "Manage a CoachIQ coaching business: bookings, clients, forms, lessons, inbox, programs, website.",
     "category": "productivity",
     "homepage": "https://www.coachiq.io",
     "keywords": ["coaching", "scheduling", "bookings", "crm"],
     "domains": ["coachiq.io"],
     "version": "1.0.0",
     "author": { "name": "CoachIQ", "email": "support@coachiq.io" }
   }
   ```

   Check the repo's current `marketplace.json` for the exact `source` object shape and the allowed
   `category` values before opening the PR; the validator is the authority.
3. Run `python3 scripts/generate-plugin-index.py` and `python3 scripts/validate-catalog.py`, commit
   the regenerated index, open the PR. Each new release needs a new PR bumping `sha` and `version`.
