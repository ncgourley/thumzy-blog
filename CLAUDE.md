# Skynat Dev Blog

Public development blog for the Skynat (internally: Thumzy) autonomous digital assistant project.

## Structure
- index.html - single-page blog, loads posts from posts/index.json
- posts/YYYY-MM-DD.md - daily blog posts with YAML frontmatter
- posts/index.json - ordered list of posts (newest first)

## Hosting
Cloudflare Pages at thumzy-blog.pages.dev. Auto-deploys on push to main.

## Writing New Posts
1. Create posts/YYYY-MM-DD.md with frontmatter (title, date, versions, stats)
2. Add entry to posts/index.json (newest first)
3. Commit and push

## Style
- Technical but engaging, third-person dev log voice
- Include metrics (commits, success rate, versions shipped)
- Highlight key decisions and their rationale
- 400-600 words per post
- No em dashes
- Use "Skynat" for the public-facing name
