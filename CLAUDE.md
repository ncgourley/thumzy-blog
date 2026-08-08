# Skynat Dev Blog

Public development blog for the Skynat autonomous digital assistant project.
"Skynat" is the public name; "Thumzy" is the internal one, and the repo, this
directory and the GitHub remote all still say `thumzy-blog`. That is intended.

> Rewritten 2026-08-08. The previous version of this file was the repo's initial
> commit (2026-03-22) and had never been touched across 51 later commits. Nearly
> every concrete claim in it had gone wrong, including the site URL, which pointed
> at a hostname that does not resolve.

## Status: DORMANT since 2026-06-09

Do not assume this blog is live-updating. 47 posts ran from 2026-03-10 (Day 0)
to 2026-06-09 (Day 91), then stopped. Two separate reasons, in order:

1. The daemon stopped running the job in June. No config change, no commit; it
   just stopped. The last repo commit of any kind is 2026-06-19.
2. Since 2026-08-01 it is disabled on purpose. `~/git/thumzy/config.yaml` sets
   `quiet_mode: true`, and that flag's own comment names the daily blog as one of
   the jobs it turns off. Publishing will not resume until that is cleared.

Posting was never actually daily: 47 posts across a 92-day span, with a 37-day
gap from 2026-04-12 to 2026-05-18 and a 6-day gap after that.

## ⚑ The six newest posts are broken, and they are publicly visible

`2026-05-30` and `2026-06-05` through `2026-06-09` contain no prose at all. Their
entire body is the stats line plus a raw Codex error string. Day 91, the top post
on the live site right now, reads in full:

```
0 tasks completed, 0 failed (100.0% success rate)

error: You've hit your usage limit. Upgrade to Pro (...), visit ... to purchase
more credits or try again at Jun 10th, 2026 8:29 PM.
```

The generator wrote its own failure output to disk and published it. Anyone
reviving or retiring this blog should deal with those six files first. Several
earlier posts are content-free by construction too (Day 70: "nothing shipped...
operational calm"), because the job ran whether or not there was anything to say.

## Where it lives

- Cloudflare Pages project **`skynat-blog`**, live at **https://skynat-blog.pages.dev**.
- There is no `thumzy-blog.pages.dev`. That hostname does not resolve (checked
  2026-08-08). Only the repo and the local directory carry the old name.

## Structure

- `index.html` is the whole site. One page, no build step, no framework. It
  fetches `posts/index.json`, then each post, and renders the markdown with a
  handful of inline regexes. Fonts come from Google Fonts, so the page is not
  offline-capable.
- `posts/YYYY-MM-DD.md`, one post per day, YAML frontmatter `title`, `date`,
  `versions`, `stats`.
- `posts/index.json`, ordered newest first. 47 entries, in sync with the files
  on disk.

## Posts are generated, not written

Nothing here has been hand-authored since Day 12. The thumzy daemon writes and
ships each post: `_publish_daily_blog_for_local_day()` in
`~/git/thumzy/thumzy/jobs.py`. It builds a prompt from that day's logbook
entries, git commits, summit reports and success rate, writes the file, prepends
the index entry, commits as `blog: publish Skynat Day N`, pushes, and deploys.

Day 0 is hard-coded as 2026-03-10, so the day number is derived, not chosen.
Frontmatter is rendered mechanically from real numbers by
`_render_daily_blog_frontmatter()`; do not hand-edit it to say something the
stats do not.

To change how posts read, edit the prompt in `jobs.py`, not this file. The live
instruction is: concise, matter-of-fact, occasional dry humor, do not try too
hard to be clever, never em dashes, never the "Not X. Not Y." construction,
**120-180 words**, and 1-3 summary bullets before the prose. Measured across all
47 posts the median is 217 words and the longest is 252, so even the generator
overshoots its own target; nothing here has ever approached the 400-600 words the
old version of this file claimed.

## Deploying

Two paths, both to the same project, and both currently idle:

- The daemon runs `npx wrangler pages deploy . --project-name=skynat-blog
  --commit-dirty=true` straight after its push.
- `.github/workflows/deploy.yml` fires on push to `main` and does the same, using
  the `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID` repo secrets.

The deploy step can fail with `code 9109` while the build passes, which looks
like a clean push but silently does not go live. Check
`gh run list -R ncgourley/thumzy-blog`, not just whether the commit landed.

## Housekeeping

`.wrangler/cache/pages.json` and `.wrangler/cache/wrangler-account.json` are
still tracked even though `.gitignore` lists `.wrangler/`, because they were
committed before the ignore rule existed. They hold the Cloudflare account id and
account email. `git rm --cached` them if you touch this repo. This is the same
class of thing meta fixed for gear-scan in commit `8467d33`.
