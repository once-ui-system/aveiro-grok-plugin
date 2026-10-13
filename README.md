# Aveiro plugin for Grok Build

Connect Grok Build to [Aveiro](https://aveiro.app), the publishing platform for hosted
websites, newsletters and social posts. Write and publish pages, organise collections,
put real screenshots on a page, and draft social posts and newsletter campaigns through
the hosted Aveiro MCP server.

## Installation

In Grok Build, open `/plugin`, search for **Aveiro**, and install.

On first connection, Grok opens the Aveiro sign-in in your browser. Sign in, pick the
organization Grok should work in, and approve. No API key is needed and nothing should
be pasted into chat.

## What you get

- **MCP server** `aveiro` at `https://www.aveiro.app/api/mcp/mcp` (Streamable HTTP,
  OAuth 2.1).
- **Skill** `aveiro`: how to use the Aveiro tools well (read the authoring guide before
  writing a page, never guess IDs, edits stay drafts until published, confirm
  deletions, check credit costs first) and how Aveiro pages are written.

### Tools

| Area | Tools |
|---|---|
| Account & guide | `aveiro_me`, `aveiro_authoring_guide` |
| Sites | `aveiro_list_sites`, `aveiro_list_templates`, `aveiro_create_site` |
| Pages | `aveiro_list_pages`, `aveiro_get_page`, `aveiro_create_page`, `aveiro_update_page`, `aveiro_delete_page`, `aveiro_reorder_pages` |
| Collections | `aveiro_list_collections`, `aveiro_create_collection`, `aveiro_delete_collection` |
| Publishing | `aveiro_list_changes`, `aveiro_publish_site` |
| Site media | `aveiro_list_media`, `aveiro_prepare_media_upload`, `aveiro_media_list_screenshot_styles`, `aveiro_media_create_screenshot`, `aveiro_media_get_screenshot`, `aveiro_media_start_screenshot_login` |
| Social | `aveiro_social_list_accounts`, `aveiro_social_list_posts`, `aveiro_social_get_post`, `aveiro_social_get_performance`, `aveiro_social_create_draft`, `aveiro_social_update_draft`, `aveiro_social_list_media`, `aveiro_social_list_org_media`, `aveiro_social_prepare_upload`, `aveiro_social_register_media`, `aveiro_social_generate_media` |
| Website video | `aveiro_video_list_bundles`, `aveiro_video_create_mockup`, `aveiro_video_get` |
| Newsletter | `aveiro_newsletter_list_campaigns`, `aveiro_newsletter_get_campaign`, `aveiro_newsletter_get_analytics`, `aveiro_newsletter_create_draft`, `aveiro_newsletter_update_draft` |

Every tool carries MCP annotations (`readOnlyHint`, `destructiveHint`, `idempotentHint`),
so Grok can tell a lookup from a deletion before calling it.

Some actions are deliberately not exposed: approving and publishing social posts, and
sending, scheduling or test-sending newsletter campaigns, stay in the Aveiro dashboard.
Agents draft; a person ships.

## Example prompts

- "List the pages on my Aveiro docs site."
- "Write a changelog page for this release on my Aveiro blog, then show me what's waiting to publish."
- "Add a screenshot of our pricing page to the getting-started article."
- "Draft a LinkedIn post announcing the new docs section."
- "Draft this month's newsletter from the last four blog posts."
- "How did last week's newsletter perform?"

## Authentication and network

The plugin connects only to Aveiro. Authentication is OAuth 2.1 authorization code
with PKCE and dynamic client registration; Grok handles the flow.

Network endpoints:

- `https://www.aveiro.app/api/mcp/mcp`: hosted MCP (Streamable HTTP)
- `https://www.aveiro.app/.well-known/oauth-protected-resource`: discovery metadata
  (RFC 9728). It names Aveiro's authorization server (Aveiro's Supabase Auth OAuth 2.1
  server), which handles client registration, authorization and tokens.
- `https://www.aveiro.app/oauth/consent`: human sign-in and organization consent
  during authorization

Credentials: an Aveiro account that is a member of the organization being connected.
On approval Aveiro issues a token scoped to that organization with these permissions:
`sites:read`, `sites:write`, `content:read`, `content:write`, `collections:write`,
`content:delete`, `publish:write`, `social:write`, `newsletter:read`,
`newsletter:write`. Each call is still checked against the organization's enabled
features and the member's own permissions. The connection appears under
**Organization → API tokens** as `MCP · <client name>`; revoke it there at any time.

Personal API tokens (`av_live_…`, created under **Organization → API tokens**) are also
accepted as `Authorization: Bearer` for clients that cannot run OAuth.

## Costs

Image generation, page screenshots and website videos spend the organization's AI
credits. The screenshot and video tools accept `estimate_only: true` to return the cost
without spending anything, and credits for a failed render are refunded automatically.

## Links

- Developer docs: https://aveiro.app/developers/mcp
- Support: support@aveiro.app

## License

MIT, see [LICENSE](LICENSE).
