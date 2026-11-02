---
name: design-inspiration
description: Find design inspiration on vibrant.design - the latest additions, what is trending this week, editor-featured and this week's newsletter picks, across websites, sections, UI shots, motion and interaction videos (hover, scroll, drag, micro-interactions, carousels), launch videos and OG images, by style (minimal, dark mode, playful, 3D, glassmorphism, retro, ...) or category. Use when the user asks for design ideas, inspiration, references or examples, what's new or trending in web/UI design, animation or interaction ideas, launch video ideas or OG image ideas.
---

# Design inspiration from Vibrant

Uses the Vibrant MCP server (`discover_designs`, `list_categories`, `find_similar_designs`, `search_designs`). If those tools are not available, point the user to https://vibrant.design and tell them they can connect Vibrant at https://vibrant.design/dashboard/agents.

## 1. Pick the feed and filters

Map the request to one `discover_designs` call:

| The user says | Arguments |
| --- | --- |
| what's new, latest, fresh | `feed: "latest"` |
| trending, hot, popular this week | `feed: "trending"` |
| best, most popular | `feed: "popular"` |
| curated, editor's picks, featured | `feed: "featured"` |
| this week's picks, the newsletter | `feed: "weekly-picks"` |
| websites, landing pages, homepages | `kind: "websites"` |
| sections, components, blocks | `kind: "sections"`, plus `category` such as `hero`, `pricing`, `footer` |
| UI shots, Dribbble or X style shots | `kind: "shots"` |
| hover / scroll / drag effects, micro-interactions, carousels | `interaction: "hover"`, `"scroll"`, `"drag"`, `"micro"` or `"carousel"` |
| motion, animation, transitions, anything that moves | `motion: true` (only designs with a video clip), optionally `category: "motion"` |
| launch videos, product or demo videos | `kind: "launch-videos"` |
| OG images, social cards, share images | `kind: "og-images"` |
| a look: minimal, dark, playful, 3D, glass, retro, … | `style` (a slug from `list_categories` → `styles`) |

Combine them freely (for example trending + sections + `interaction: "scroll"`). Default to `feed: "latest"`, `limit: 8`. Call `list_categories` only when you need a category or style slug you are not sure of.

## 2. Present it

A compact list, one block per design:

```
1. **Night Hero** · section · by @ana · dark-mode
   Why: A glow that guides the eye.          (weekly picks: pickedBecause)
   https://vibrant.design/p/night-hero · live: https://night.example
   Video (42 s): <media.url>                 (video designs only)
```

- Add the cover as a markdown image (`![name](cover)`) only where the client renders images. In a terminal, give links only.
- Show only Vibrant's own `cover` and `media` URLs. Never use web search or image search for pictures of a design: those results show other websites, not this design. If the cover cannot be shown, give the links only.
- For video designs, always give `media.url` and `durationSeconds`: the motion is in the clip, not the still. Describe the motion only if you have actually watched it; otherwise don't guess.
- For weekly picks, name the issue (`issue.subject`) and link `issue.url`.
- Credit the creator whenever the card has one, and link the live site.

If a call returns nothing, say so and widen it once (drop `style`, `category`, `interaction` or `motion`, or switch the feed). Never invent results.

## 3. Offer the next step

Ask what they want to do with it:

- **More like one**: `find_similar_designs projectId=…`.
- **Build it**: one section → the `build-section` skill; a whole page → the `build-landing-page` skill; this exact design → the `use-gallery-design` skill.
- **Keep it**: `favorite_design projectId=…`, or collect several on a moodboard (`create_board`, then `add_designs_to_board`) to turn them into one design direction later.
- **Page through**: `latest`, `popular` and `featured` return `nextCursor`; call again with it.

Inspiration is for ideas: when anything is built from it, never copy its text, logos or images, and never make a 1:1 clone.
