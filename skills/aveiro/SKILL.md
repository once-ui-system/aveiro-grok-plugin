---
name: aveiro
description: >-
  Write, update and publish pages on hosted Aveiro sites through the Aveiro MCP
  server: list sites and pages, create and edit page MDX, organise collections,
  publish, add page screenshots and images, draft social posts and newsletter
  campaigns, and read their performance. Use whenever "Aveiro", "aveiro.app", an
  Aveiro site, or the user's Aveiro pages, posts or newsletters come up.
---

# Aveiro

Aveiro is a publishing platform: hosted websites, a newsletter and social posting
from one place. The `aveiro` MCP server bundled with this plugin acts as the
signed-in Aveiro user, inside the organization they approved when they connected.

## Before you act

1. **Read the authoring guide before your first page.** Call `aveiro_authoring_guide`
   once per session. It holds the component catalog, the exact MDX shapes the editor
   accepts and worked examples. Do not write page MDX from memory.
2. **Never guess IDs.** Discover them: `aveiro_list_sites` for site IDs,
   `aveiro_list_pages` and `aveiro_get_page` for pages, `aveiro_list_collections` for
   collections, `aveiro_social_list_accounts` for social accounts. Call `aveiro_me`
   when you need to know which sites and permissions the connection has.
3. **Edits are drafts until published.** Creating or updating a page does not change
   the live site. `aveiro_list_changes` shows what is waiting. Only call
   `aveiro_publish_site` when the user asks to publish, and say which changes go live.
4. **Confirm destructive actions.** `aveiro_delete_page` and `aveiro_delete_collection`
   need an explicit yes from the user first. Their tool annotations
   (`destructiveHint`) mark them.
5. **Credits.** `aveiro_social_generate_media`, `aveiro_media_create_screenshot` and
   `aveiro_video_create_mockup` spend the organization's AI credits. The screenshot and
   video tools take `estimate_only: true` to show the cost first; use it when the user
   has not already agreed to spend.

## How Aveiro pages are written

Pages are linear editorial documents: markdown headings and prose first, registered
MDX components only where they earn their place.

- No custom heroes, rows of buttons or nested layout scaffolding on content pages.
- Reference an outside site with one `<OgCard url="…" />` where it is discussed, not a
  list of bare links at the end.
- Link related pages on the same site with `<Collection />`, not markdown link lists.
- Show numbers as a `<LineChart />`, following its data contract in the guide.
- Use real images only: uploaded, generated or screenshotted through the tools, placed
  with the `<Media />` block shape from the guide. Never invent image URLs.

## Choosing a tool

| You want to | Use |
|---|---|
| Find sites, pages, collections | `aveiro_list_sites`, `aveiro_list_pages`, `aveiro_get_page`, `aveiro_list_collections` |
| Start a new site | `aveiro_list_templates`, then `aveiro_create_site` |
| Write or edit a page | `aveiro_create_page`, `aveiro_update_page` |
| Organise pages | `aveiro_create_collection`, `aveiro_reorder_pages` |
| See and ship pending edits | `aveiro_list_changes`, then `aveiro_publish_site` |
| Put a screenshot of a real page on a site | `aveiro_media_create_screenshot`, then poll `aveiro_media_get_screenshot` |
| Upload or reuse site images | `aveiro_prepare_media_upload`, `aveiro_list_media` |
| Draft a social post | `aveiro_social_list_accounts`, `aveiro_social_create_draft`, `aveiro_social_update_draft` |
| Make images or a website video for a post | `aveiro_social_generate_media`, `aveiro_video_create_mockup` (poll `aveiro_video_get`) |
| See how posts performed | `aveiro_social_list_posts`, `aveiro_social_get_performance` |
| Draft a newsletter | `aveiro_newsletter_create_draft`, `aveiro_newsletter_update_draft` |
| See how a campaign did | `aveiro_newsletter_list_campaigns`, `aveiro_newsletter_get_analytics` |

## What stays in the dashboard

Approving and publishing social posts, and sending newsletter campaigns, happen in the
Aveiro dashboard. The tools create and revise drafts; tell the user where to finish.

## Asynchronous renders

Screenshots and website videos render in the background. The create call returns a
pending render with no image. Poll the matching get tool until it reports `done`, then
use the URL or asset ID it returns. Read its `warnings` before using a screenshot: a
page that redirected to a sign-in form produces a clean picture of the sign-in form.
If a page needs a login, `aveiro_media_start_screenshot_login` returns a link for the
user to open. Never ask for their password.
