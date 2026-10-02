# SABER project page

Single-file static site (`index.html`) + `static/` figures + `videos/` clips. No build step.

## 1. Add your clips

Drop MP4s into `videos/` with these names (or edit the `src` attributes in `index.html`):

| File | Card |
|---|---|
| `indoor_stepping_stones.mp4` | Semantic stepping stones (boxes vs rails) |
| `indoor_pipe_30cm.mp4` | 30 cm pipe |
| `indoor_turf_sideways.mp4` | Turf patches, forward/backward/sideways |
| `indoor_three_low_pipes.mp4` | Three low pipes |
| `outdoor_pipe_deck.mp4` | Pipe on timber deck |
| `outdoor_grass_strip.mp4` | Grass strip beside pavement |
| `outdoor_tiles_with_semantics.mp4` | Comparison, left |
| `outdoor_tiles_without_semantics.mp4` | Comparison, right |

Optional: a poster frame `videos/<same name>.jpg` shows instantly while the clip loads.

Recommended encode (keeps each clip well under GitHub's 25 MB/file comfort zone and autoplays on iOS):

```bash
ffmpeg -i input.mov -vf "scale=1280:-2,fps=30" -c:v libx264 -crf 23 -preset slow \
       -pix_fmt yuv420p -movflags +faststart -an videos/indoor_pipe_30cm.mp4
# poster frame at t=2s
ffmpeg -ss 2 -i videos/indoor_pipe_30cm.mp4 -frames:v 1 -q:v 3 videos/indoor_pipe_30cm.jpg
```

Trim first if a clip is long: add `-ss <start> -to <end>` before `-i`. Aim for 10–30 s per clip; the cards loop.

**Click-to-enlarge:** once a clip loads, its tile becomes clickable and opens a large player that continues from the same moment (Esc or a click outside closes it). The two comparison clips open together, in sync. A tile whose file is missing just shows the striped placeholder. Keep the two comparison clips the same length so they stay aligned.

## 2. Fill the placeholders

Search `index.html` for:

- `href="#"` in the author block → homepages / Google Scholar
- `EMAIL_PLACEHOLDER` (footer)
- `SITE_URL` — appears in `index.html`, `robots.txt`, `sitemap.xml` and `llms.txt`. Once you know the final URL, replace it everywhere in one go:
  `sed -i 's#SITE_URL#https://saber-locomotion.github.io#g' index.html robots.txt sitemap.xml llms.txt` (no trailing slash)

## 3. Publish on GitHub Pages

Option A — clean project URL (like omniretarget.github.io): create a GitHub **organization** named e.g. `saber-locomotion`, then a repo in it named exactly `saber-locomotion.github.io`.

Option B — under your username: repo `saber` → served at `https://<username>.github.io/saber/`.

```bash
cd site
git init && git add . && git commit -m "SABER project page"
git branch -M main
git remote add origin git@github.com:<org-or-username>/<repo>.git
git push -u origin main
```

Then: repo → Settings → Pages → Source: *Deploy from a branch* → `main` / `/ (root)` → Save. Live in ~1 minute.

## 4. Get indexed (AI tools search through these)

1. **Google Search Console** (search.google.com/search-console): add the site (URL-prefix), verify with the HTML-tag method by pasting the `<meta name="google-site-verification">` tag into `<head>`, then *Sitemaps → submit `sitemap.xml`*, and *URL inspection → Request indexing* for `/`.
2. **Bing Webmaster Tools** (bing.com/webmasters): *Import from Google Search Console* (one click), then submit the sitemap. Bing feeds several AI search products.
3. Optional checks: paste the URL into Google's Rich Results Test (validates the JSON-LD) and Schema.org validator.
4. Add the abstract + links to the **YouTube description** and upload captions (.srt) — transcripts are indexed and are what NotebookLM ingests from a video link.
5. Claim the paper at **huggingface.co/papers/2609.21572** and on your Semantic Scholar / Google Scholar profiles.

## 5. Before you announce

- Make the YouTube video public (the embed works while unlisted, so you can preview first).
- Add `Project page: <url>` to the arXiv Comments field in your v2 (the one that also fixes the last author's name).
- Check the page on a phone; the clip grid collapses to one column.
