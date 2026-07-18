# Flash Reader

An RSVP (Rapid Serial Visual Presentation) speed reader. Paste in any text and
it flashes one word at a time at a fixed point on screen, so your eyes never
have to move across a line — just hold focus and let the words come to you.

The whole thing is one file (`index.html` — markup, styles, vanilla JS; no
build, no backend, no libraries beyond two Google Fonts).

**Live:** https://demo.justintormey.com/rsvp-reader/

## How it works

- **ORP pivot**: each word is split at its "optimal recognition point" (roughly
  the character your eye would naturally fixate on) and that letter is
  highlighted in red, vertically aligned across every word so your gaze never
  has to hunt for the anchor.
- **Phantom words**: dimmed previews of the previous/next word sit just
  off-center, giving peripheral context without competing for focus.
- **Two text formats**:
  - Plain text — paragraphs separated by blank lines.
  - `@rsvp` directive format — supports `@title`, `@author`, `@chapter Name`,
    and `@para`, which populate a proper table of contents on the scrubber
    (chapter marks render in blue) and trigger a fading chapter-name overlay
    as you cross into a new one.
- **Gestures**: tap to pause/resume, swipe left/right to jump a paragraph,
  swipe up/down to nudge speed ±25 WPM, long-press to restart, drag the
  scrubber to jump anywhere, tap Mark to bookmark your spot. Keyboard
  equivalents work too (space, arrows, `b`).
- **Persistence**: text, speed, theme, and bookmarks are saved to
  `localStorage`, so reloading the page resumes exactly where you left off.
- Speed range 100–900 WPM with presets at 200/300/450/600; a punctuation-pause
  slider adds extra dwell time at commas and sentence ends (and more at
  paragraph/chapter boundaries) so the pacing doesn't feel robotic.

## Develop

It's a single static file — just open it, or serve it locally:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000/
```

## Deploy

Publishes `index.html` (and the thumbnail image) to the `rsvp-reader/` prefix
of the shared `s3://justintormey.com` bucket (fronted by CloudFront on
demo.justintormey.com), then invalidates that prefix.

```bash
./scripts/deploy --dry-run   # preview what would upload
./scripts/deploy             # apply + invalidate CloudFront
```

Deploy config (AWS profile, CloudFront id, S3 prefix) lives in
`.local/deploy.json`, which is **gitignored**. If you clone fresh, recreate it:

```json
{
  "aws_profile": "default",
  "cloudfront_id": "E1R27W2LA6BBEH",
  "s3_prefix": "rsvp-reader"
}
```

### Notes

- **Additive sync, no `--delete`.** The bucket is shared with sibling demos
  (`leftright/`, `retro/`, `sketchbook/`, `drift/`, …), so the deploy never
  deletes.
- Originally built in the claude.ai web interface and only lived as a
  downloaded HTML file until it was formalized into this repo.
