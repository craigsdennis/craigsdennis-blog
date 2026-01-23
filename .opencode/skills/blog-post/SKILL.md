---
name: blog-post
description: Draft and publish blog posts for an Astro blog. Creates feature branches with preview URLs, then merges to main when ready to publish.
---

# Blog Post Skill

This skill drafts and publishes blog posts for an Astro-based blog. It operates autonomously without asking questions.

**Two workflows:**
1. **Draft** - Create a new blog post on a feature branch, push to origin for preview
2. **Publish** - Merge an approved draft branch into main and push to production

## Environment Variables

Check `.dev.vars.example` for configuration:
- `REPLICATE_API_TOKEN` - Enables AI-generated hero images via Replicate
- `BLOG_PREVIEW_URL_PATTERN` - Preview URL pattern with `{branch}` placeholder
- `BLOG_PRODUCTION_URL` - Production site URL

## Drafting a New Blog Post

IMPORTANT: Do NOT ask the user questions. Make decisions autonomously. The user will review and provide feedback on the draft.

### 1. Study Existing Content

Before writing, read existing blog posts in `src/content/blog/` to understand:
- Writing style and tone
- How posts are structured
- Level of technical depth
- Use of code examples, links, and formatting

Match the voice and style of the existing content.

### 2. Parse the User's Request

Extract from the user's message:
- **Topic** - What the post should be about
- **Title** - Infer a compelling title if not provided
- **Description** - Generate a 1-2 sentence SEO summary

If the topic involves current tools, APIs, libraries, or services, use `webfetch` to get the latest information from official documentation or websites. Do not rely on potentially outdated training data.

### 3. Research with WebFetch

Before writing, gather current information:
- Fetch official documentation for any tools/APIs mentioned
- Verify current syntax, features, and best practices
- Check for recent updates or changes

Example:
```
If writing about "Replicate API", fetch:
- https://replicate.com/docs
- Relevant model pages for current parameters
```

### 4. Generate Slug

Convert the title to a URL-friendly slug:
- Lowercase
- Replace spaces with hyphens
- Remove special characters
- Example: "My Awesome Post!" -> `my-awesome-post`

### 5. Create Feature Branch

```bash
git checkout main
git pull origin main
git checkout -b blog/<slug>
```

### 6. Generate Hero Image (If Replicate Available)

If Replicate MCP tools are available, generate a hero image automatically:

1. Create a prompt that matches the post topic and a modern tech aesthetic
2. Use `replicate_create_models_predictions`:
   - **model_owner**: `black-forest-labs`
   - **model_name**: `flux-schnell`
   - **Prefer**: `wait`
   - **input**: `{"prompt": "<descriptive prompt>", "aspect_ratio": "16:9"}`

3. Download the output image:
   ```bash
   curl -L -o src/assets/<slug>-header.webp "<output_url>"
   ```

### 7. Create Blog Post File

Create `src/content/blog/<slug>.md`:

```markdown
---
title: '<Title>'
description: '<Description>'
pubDate: '<Current Date: Mon DD YYYY format>'
heroImage: '../../assets/<slug>-header.webp'
---

<Well-structured content with headers, code examples, lists>
```

**Content guidelines:**
- Match the tone and style of existing posts in `src/content/blog/`
- Use markdown: headers, lists, code blocks, links
- Include working code examples where relevant
- Break up with subheadings every 2-3 paragraphs
- Link to official documentation

### 8. Commit and Push

```bash
git add src/content/blog/<slug>.md
git add src/assets/<slug>-header.webp
git commit -m "Draft: <Title>"
git push -u origin blog/<slug>
```

### 9. Report to User

After pushing, tell the user:

```
Draft pushed to branch `blog/<slug>`

Preview: https://<BLOG_PREVIEW_URL_PATTERN with {branch} replaced>/blog/<slug>

(Preview deployment takes 1-2 minutes)

Review the draft and let me know if you'd like any changes, or say "publish" when ready.
```

## Publishing a Draft

When user says "publish" or "publish it" or similar:

### 1. Identify Current Branch

Check if already on a blog branch, or find the most recent one:
```bash
git branch --show-current
git branch -r | grep 'origin/blog/'
```

### 2. Merge and Push

```bash
git checkout main
git pull origin main
git merge origin/blog/<slug> --no-edit
git push origin main
```

### 3. Clean Up

```bash
git push origin --delete blog/<slug>
git branch -d blog/<slug>
```

### 4. Confirm

```
Published! Merged to main and pushed.

Live at: <BLOG_PRODUCTION_URL>/blog/<slug>

(Deployment takes 1-2 minutes)
```

## Key Principles

1. **Never ask questions** - Make reasonable decisions, user reviews the output
2. **Study existing content first** - Read posts in `src/content/blog/` to match style
3. **Always use webfetch** - For any technical topics, fetch current docs
4. **Always use git** - Create branches, commit, push to origin
5. **Always generate images** - If Replicate is available, create a hero image
6. **Provide preview URL** - So user can review before publishing

## File Locations

- Blog posts: `src/content/blog/<slug>.md`
- Hero images: `src/assets/<slug>-header.webp`
- Site config: `src/consts.ts`

## Frontmatter Schema

Required:
- `title` (string)
- `description` (string)
- `pubDate` (string) - Format: `Jan 20 2026`

Optional:
- `heroImage` (string) - `../../assets/<filename>.webp`
- `updatedDate` (string)

## Git Commands Reference

**Draft:**
```bash
git checkout main && git pull origin main
git checkout -b blog/<slug>
# ... create files ...
git add .
git commit -m "Draft: <Title>"
git push -u origin blog/<slug>
```

**Publish:**
```bash
git checkout main && git pull origin main
git merge origin/blog/<slug> --no-edit
git push origin main
git push origin --delete blog/<slug>
git branch -d blog/<slug>
```
