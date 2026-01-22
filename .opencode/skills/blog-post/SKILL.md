---
name: blog-post
description: Draft and publish blog posts for an Astro blog. Creates feature branches with preview URLs, then merges to main when ready to publish.
---

# Blog Post Skill

This skill helps draft and publish blog posts for an Astro-based blog. It supports two workflows:

1. **Draft** - Create a new blog post on a feature branch, push to origin for automatic preview deployment
2. **Publish** - Merge an approved draft branch into main and push to deploy to production

## Environment Variables

Check `.dev.vars.example` for available environment variables:
- `REPLICATE_API_TOKEN` - If set, enables AI-generated hero images via Replicate
- `BLOG_PREVIEW_URL_PATTERN` - Preview URL pattern with `{branch}` placeholder (e.g., `{branch}-my-blog.pages.dev`)
- `BLOG_PRODUCTION_URL` - Production site URL (e.g., `https://myblog.dev`)

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

### 3. Create Feature Branch with Git

IMPORTANT: Always use git to create and push branches.

```bash
# Ensure we're on main and up to date
git checkout main
git pull origin main

# Create the feature branch
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

**Date Format**: Use format like `Jan 20 2026`.

**Content Style**: 
- Use markdown with headers, lists, code blocks as appropriate
- Write in a conversational, educator-friendly tone
- Include code examples when relevant
- Break up content with subheadings

### 5. Generate Hero Image (Optional)

If the Replicate MCP tools are available (check for `replicate_*` tools) and user wants a hero image:

1. Use the `replicate_create_models_predictions` tool to generate an image:
   - **model_owner**: `black-forest-labs`
   - **model_name**: `flux-schnell`
   - **input**: `{"prompt": "<your prompt>", "aspect_ratio": "16:9"}`
   - **Prefer**: `wait` (to wait for the result)

2. The prediction response will contain an `output` array with image URL(s)

3. Download the generated image and save it to `src/assets/<slug>-header.webp`:
   ```bash
   curl -o src/assets/<slug>-header.webp "<output_url>"
   ```

4. Include the `heroImage` frontmatter field pointing to `../../assets/<slug>-header.webp`

**Prompt Tips for Hero Images**:
- Be specific about style, composition, and colors
- Consider the blog's aesthetic (check `src/styles/global.css` for design cues)
- Example: "Minimalist illustration of a robot writing code, soft gradients, modern tech aesthetic"

### 6. Commit and Push with Git

IMPORTANT: Always commit and push the branch to origin.

```bash
# Stage the new files
git add src/content/blog/<slug>.md
git add src/assets/<slug>-header.webp  # If hero image was generated

# Commit with a descriptive message
git commit -m "Draft: <Title>"

# Push the branch to origin (this triggers preview deployment)
git push -u origin blog/<slug>
```

### 7. Provide Preview URL

After pushing, construct the preview URL using the `BLOG_PREVIEW_URL_PATTERN` environment variable.

Replace `{branch}` with the branch name, converting `/` to `-`:
- Branch: `blog/my-awesome-post`
- URL branch segment: `blog-my-awesome-post`

Tell the user:
```
Your draft has been pushed to origin.

Preview URL: https://<preview-url-pattern>/blog/<slug>

Note: It may take 1-2 minutes for the preview deployment to complete.

When you're ready to publish, just ask me to publish this post.
```

## Publishing a Draft

When the user wants to publish a drafted post:

### 1. Identify the Draft

If the user doesn't specify which draft, list available draft branches:
```bash
git fetch origin
git branch -r | grep 'origin/blog/'
```

### 2. Merge to Main with Git

IMPORTANT: Use git to merge and push.

```bash
# Ensure main is up to date
git checkout main
git pull origin main

# Merge the draft branch
git merge origin/blog/<slug> --no-edit

# Push to deploy to production
git push origin main
```

### 3. Clean Up Branch

After successful merge, offer to delete the draft branch:
```bash
# Delete remote branch
git push origin --delete blog/<slug>

# Delete local branch
git branch -d blog/<slug>
```

### 4. Confirm Deployment

Tell the user:
```
Published! Your post has been merged to main and pushed.

Production URL: <BLOG_PRODUCTION_URL>/blog/<slug>

Deployment typically completes within 1-2 minutes.
```

## File Locations Reference

- Blog posts: `src/content/blog/<slug>.md`
- Hero images: `src/assets/<slug>-header.webp`
- Site constants: `src/consts.ts`
- Content schema: `src/content.config.ts`

## Frontmatter Schema

Required fields:
- `title` (string) - Post title
- `description` (string) - Brief summary
- `pubDate` (string) - Publication date like "Jan 20 2026"

Optional fields:
- `heroImage` (string) - Relative path to hero image: `../../assets/<filename>.webp`
- `updatedDate` (string) - Last updated date

## Example Blog Post

```markdown
---
title: 'Building with AI APIs'
description: 'A practical guide to integrating AI APIs into your projects without the complexity.'
pubDate: 'Jan 22 2026'
heroImage: '../../assets/building-with-ai-apis-header.webp'
---

Introduction paragraph that hooks the reader...

## Why This Matters

Context and background for your topic.

## Getting Started

Step-by-step instructions with code examples:

\`\`\`javascript
const example = "code here";
\`\`\`

## Key Takeaways

- Main point one
- Main point two
- Main point three

## Conclusion

Wrap up and call to action.
```

## Git Workflow Summary

**Draft:**
1. `git checkout main && git pull origin main`
2. `git checkout -b blog/<slug>`
3. Create/edit files
4. `git add . && git commit -m "Draft: <Title>"`
5. `git push -u origin blog/<slug>`

**Publish:**
1. `git checkout main && git pull origin main`
2. `git merge origin/blog/<slug> --no-edit`
3. `git push origin main`
4. `git push origin --delete blog/<slug>` (cleanup)
