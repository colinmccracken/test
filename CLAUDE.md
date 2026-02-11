# CLAUDE.md

## Project Overview

**Brevifolia** is a minimalist blog starter template built with [Gatsby](https://www.gatsbyjs.org/) (v2) and integrated with [Forestry CMS](https://forestry.io/). It generates a static blog site from Markdown content with image optimization, SCSS styling, and responsive design.

- **Framework**: Gatsby v2.13.9 (React-based static site generator)
- **Language**: JavaScript (React functional components with hooks)
- **Styling**: SCSS with CSS Modules
- **Content**: Markdown files with YAML front matter
- **CMS**: Forestry.io for content management
- **License**: MIT

## Directory Structure

```
src/
  components/       # Reusable React components (BlogList, Header, Layout)
  pages/            # Gatsby pages (index, info, 404) - auto-routed
  templates/        # Dynamic page templates (blog post)
  static_queries/   # Custom hooks wrapping useStaticQuery (useSiteMetadata, useBlogData)
  styles/           # SCSS styles organized by type
    components/     # Component-scoped CSS modules
    pages/          # Page-scoped CSS modules
    templates/      # Template-scoped CSS modules
    reset.scss      # CSS reset
    global.scss     # Global styles
content/
  posts/            # Markdown blog posts (title, author, date, hero_image front matter)
  data/             # YAML/JSON data (info.json, authors.yaml)
  images/           # Blog post images
static/             # Static assets (favicon, logos)
.forestry/          # Forestry CMS configuration and content templates
```

## Key Configuration Files

| File | Purpose |
|------|---------|
| `config.json` | Site metadata (title, description, contact info, colors) - editable via Forestry |
| `gatsby-config.js` | Gatsby plugins, source filesystem config, site metadata |
| `gatsby-node.js` | Dynamic page creation: generates `/blog/:slug` routes from Markdown files |
| `gatsby-browser.js` | Imports global stylesheets (reset + global SCSS) |
| `.prettierrc` | Prettier formatting rules |
| `.forestry/settings.yml` | Forestry CMS sidebar sections and preview configuration |

## Development Commands

```bash
# Start development server (opens browser automatically)
yarn dev

# Build for production
yarn build

# Serve production build locally
yarn serve

# Format source code
yarn format

# Forestry CMS preview server
yarn forestry-preview
```

## Code Conventions

### Components
- **Functional components only** using React hooks
- CSS Modules (`.module.scss`) for component-scoped styling
- Static data fetched via custom `useStaticQuery` hooks in `src/static_queries/`

### Styling
- SCSS as the CSS preprocessor
- CSS Modules for isolation (import as `styles` object)
- Mobile-first responsive design; breakpoint at `768px`
- Desktop: sidebar nav (30%) + content (70%); Mobile: stacked layout

### Prettier Configuration
- 2-space indentation
- No semicolons
- Double quotes
- Trailing commas (ES5)
- LF line endings

### Data Flow
- **Static queries**: `useSiteMetadata` and `useBlogData` hooks for global/list data
- **Page queries**: GraphQL queries in page/template components (e.g., blog template queries by `$slug`)
- **Dynamic pages**: `gatsby-node.js` creates blog pages from Markdown via `createPages` API

### Content
- Blog posts are Markdown files in `content/posts/` with front matter: `title`, `author`, `date`, `hero_image`
- Site data lives in `content/data/` as JSON/YAML
- Images in `content/images/` are optimized by `gatsby-image` + `gatsby-plugin-sharp`

## Architecture Notes

- **Page generation**: `gatsby-node.js` hooks into `onCreateNode` to add slug fields to Markdown nodes, then `createPages` generates a page per post using `src/templates/blog.js`
- **Blog navigation**: Posts link to the next post in a cycling pattern (last post links back to first)
- **Image handling**: Uses Gatsby's image pipeline (`gatsby-image`, `gatsby-plugin-sharp`, `gatsby-transformer-sharp`) for responsive, optimized images with blur-up placeholders
- **Content rendering**: Blog post HTML is rendered via `dangerouslySetInnerHTML` from transformed Markdown

## Dependencies

### Core
- `gatsby` v2.13.9, `react` v16.8.6, `react-dom` v16.8.6

### Key Gatsby Plugins
- `gatsby-source-filesystem` - Sources content from `content/` directories
- `gatsby-transformer-remark` - Transforms Markdown to HTML
- `gatsby-image` / `gatsby-plugin-sharp` / `gatsby-transformer-sharp` - Image optimization
- `gatsby-plugin-sass` - SCSS support
- `gatsby-plugin-react-helmet` - Document head management
- `gatsby-transformer-yaml` - YAML data transformation

### Dev
- `prettier` v1.18.2

## Testing

No formal test suite is configured. The `test` script is a placeholder:
```bash
yarn test  # Prints a reminder to add tests
```

## Notes for AI Assistants

- This is a small starter template (~300 lines of JS). Avoid over-engineering changes.
- No ESLint is configured; rely on Prettier for formatting (`yarn format`).
- Gatsby v2 is used (not v3+). APIs and plugin versions should match v2 documentation.
- Node.js v10 is the target runtime (per Forestry Docker config).
- The `node-sass` package (v4.12.0) is used, not `sass`/Dart Sass.
- When adding new blog posts, follow the existing front matter schema in `content/posts/`.
- When adding new components, create a matching `.module.scss` file in the appropriate `src/styles/` subdirectory.
