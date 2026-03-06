# VibeGram

## Current State
VibeGram is a full-featured social media app with: auth, home feed, stories, reels, posts, DMs with audio/video call, explore, hashtag discovery, notifications, settings, profile with Posts/Reels/Tagged/Saved tabs, story creative tools, close friends, and media creation hub.

## Requested Changes (Diff)

### Add
- **Story Highlights** on Profile page: A horizontally scrollable row of highlight circles below the bio/stats area (above the posts tabs). Each highlight has a cover image and a title. A "+" button to create a new highlight (opens a sheet to select stories and set a title). Tapping a highlight opens a story viewer for all stories in that highlight. Highlights are stored in localStorage (`vg_highlights`). Each highlight has: id, title, coverEmoji or coverColor, and storyIds[].
- **Profile Share** button on Profile page: A share icon button in the profile header (next to settings). Tapping it opens a Share Profile sheet/modal showing: the user's @username + profile link (e.g. "vibegram.app/@username"), a QR-code-like placeholder card, and a "Copy Link" button that copies the profile URL to clipboard and shows a toast.

### Modify
- ProfilePage header: Add a Share icon button alongside the existing Notifications bell and Settings icons.
- ProfilePage bio/stats section: Insert the Highlights row between the Edit Profile button and the Tabs section.

### Remove
- Nothing removed.

## Implementation Plan
1. Add `HighlightsRow` component:
   - Horizontal scroll row with "+" circle to create highlight + existing highlight circles.
   - "+ New" opens `CreateHighlightSheet`: title input, emoji/color picker for cover, and a list of the user's stories (from posts with caption `__story__`) to select from.
   - Tapping existing highlight opens `HighlightViewerSheet` which plays through the highlight's stories using the existing story viewer style.
   - Store/read highlights from `localStorage("vg_highlights")`.

2. Add `ShareProfileSheet` component:
   - Slide-up sheet showing profile handle, a styled card with username + a decorative gradient QR placeholder.
   - "Copy Link" button copies `https://vibegram.app/@<username>` to clipboard, shows toast.
   - "Share" via Web Share API if available.

3. Update `ProfilePage`:
   - Import and render `HighlightsRow` between the Edit Profile button and the tabs.
   - Add share icon button in header (Share2 from lucide-react), controlled by `shareOpen` state.
   - Render `ShareProfileSheet` controlled by `shareOpen`.
