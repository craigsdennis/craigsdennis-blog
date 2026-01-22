---
name: blog-post
description: Draft and publish blog posts for craigsdennis.dev. Creates feature branches with preview URLs, then merges to main when ready to publish.
---

# Blog Post Skill

This skill helps draft and publish blog posts for craigsdennis.dev. It supports two workflows:

1. **Draft** - Create a new blog post on a feature branch with automatic preview deployment
2. **Publish** - Merge an approved draft into main and deploy to production

## Environment Variables

The skill uses these optional environment variables:
- `REPLICATE_API_TOKEN` - If set, enables AI-generated hero images via Replicate
- `BLOG_PREVIEW_URL_PATTERN` - Preview URL pattern (default: `{branch}-craigsdennis-blog.craigsdennis.workers.dev`)

## Drafting a New Blog Post

When the user wants to draft a new blog post:

### 1. Gather Information

Ask the user for:
- **Title** (required) - The blog post title
- **Description** (required) - A brief summary for SEO/social sharing (1-2 sentences)
- **Topic/Content** (required) - What the post should be about, key points to cover
- **Hero Image** (optional) - Whether to generate a hero image, and if so, what style/prompt

### 2. Generate Slug

Convert the title to a URL-friendly slug:
- Lowercase
- Replace spaces with hyphens
- Remove special characters
- Example: "My Awesome Post!" -> `my-awesome-post`

### 3. Create Feature Branch

```bash
git checkout main
git pull origin main
git checkout -b blog/<slug>
```

### 4. Create Blog Post File

Create the file at `src/content/blog/<slug>.md` with this frontmatter:

```markdown
---
title: '<Title>'
description: '<Description>'
pubDate: '<Current Date in format: Mon DD YYYY>'
heroImage: '../../assets/<slug>-header.webp'  # Only if hero image is generated
---

<Content goes here>
```

**Date Format**: Use format like `Jan 20 2026` (matches existing posts).

**Content Style**: 
- Use markdown with headers, lists, code blocks as appropriate
- Keep Craig's conversational, educator-friendly tone
- Include code examples when relevant
- Break up content with subheadings

### 5. Generate Hero Image (Optional)

If `REPLICATE_API_TOKEN` is available and user wants a hero image:

1. Use the `replicate_create_models_predictions` tool with `black-forest-labs/flux-schnell`
2. Save the output image to `src/assets/<slug>-header.webp`
3. Include the `heroImage` frontmatter field

**Prompt Tips for Hero Images**:
- Match the retro-terminal aesthetic of the site
- Include style keywords: "synthwave", "retro", "neon", "80s", "terminal aesthetic"
- Be specific about composition and colors

### 6. Commit and Push

```bash
git add src/content/blog/<slug>.md
git add src/assets/<slug>-header.webp  # If hero image was generated
git commit -m "Draft: <Title>"
git push -u origin blog/<slug>
```

### 7. Provide Preview URL

Tell the user their preview URL:
```
Preview URL: https://<branch>-craigsdennis-blog.craigsdennis.workers.dev/blog/<slug>
```

Where `<branch>` is the branch name with `/` replaced by `-` (e.g., `blog-my-awesome-post`).

**Note**: It may take 1-2 minutes for Cloudflare to build and deploy the preview.

Inform the user:
- The preview is live at the URL above
- They can review and request changes
- When ready, use this skill again to publish

## Publishing a Draft

When the user wants to publish a drafted post:

### 1. Identify the Draft

Ask which draft to publish if not specified. List available draft branches:
```bash
git branch -r | grep 'origin/blog/'
```

### 2. Merge to Main

```bash
git checkout main
git pull origin main
git merge origin/blog/<slug> --no-edit
git push origin main
```

### 3. Clean Up (Optional)

Ask if the user wants to delete the draft branch:
```bash
git push origin --delete blog/<slug>
git branch -d blog/<slug>
```

### 4. Confirm Deployment

Tell the user:
```
Published! Your post is now live at:
https://craigsdennis.dev/blog/<slug>

Deployment typically completes within 1-2 minutes.
```

## File Locations Reference

- Blog posts: `src/content/blog/<slug>.md`
- Hero images: `src/assets/<slug>-header.webp`
- Site constants: `src/consts.ts`

## Frontmatter Schema

Required fields:
- `title` (string) - Post title
- `description` (string) - Brief summary
- `pubDate` (string) - Publication date like "Jan 20 2026"

Optional fields:
- `heroImage` (string) - Relative path to hero image
- `updatedDate` (string) - Last updated date

## Example Blog Post

```markdown
---
title: 'Building with AI APIs'
description: 'A practical guide to integrating AI APIs into your projects without the complexity.'
pubDate: 'Jan 22 2026'
heroImage: '../../assets/building-with-ai-apis-header.webp'
---

Introduction paragraph here...

## First Section

Content with **bold** and `code` formatting.

### Subsection

- Bullet points
- More points

## Code Examples

\`\`\`javascript
const example = "code here";
\`\`\`

## Conclusion

Wrap up the post.
```
