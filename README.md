# flavio.aiello.ch

A minimalistic personal blog built with Jekyll and Bootstrap 5.

## Theme

This site uses a custom lightweight theme designed for simplicity and performance:

- **CSS Framework:** [Bootswatch Lux](https://bootswatch.com/lux/) (Bootstrap 5.3.3)
- **Syntax Highlighting:** Rouge with GitHub-style colors
- **No build tools required** — just Jekyll and a CDN

## Features

- Responsive sidebar layout with author profile
- Bootstrap-native styling for tables, code blocks, and blockquotes
- Disqus comments integration
- RSS feed and SEO optimization
- Mobile-friendly with collapsible sidebar

## Structure

```
├── _config.yml          # Site configuration
├── _includes/
│   ├── navbar.html      # Top navigation
│   ├── sidebar.html     # Author profile & links
│   ├── footer.html      # Mobile footer
│   └── disqus.html      # Comments
├── _layouts/
│   ├── default.html     # Base layout
│   ├── home.html        # Homepage with post list
│   ├── post.html        # Blog post template
│   └── page.html        # Static page template
├── _posts/              # Blog posts (Markdown)
├── assets/
│   ├── css/syntax.css   # Rouge syntax highlighting
│   └── images/          # Site images
└── index.html           # Homepage
```

## Local Development

```bash
bundle install
bundle exec jekyll serve
```

Visit `http://127.0.0.1:4000/`

## Dependencies

- Jekyll 3.9+
- Plugins: jekyll-feed, jekyll-sitemap, jekyll-paginate, jekyll-seo-tag

## License

Content © Flavio Aiello. Theme components under MIT License.
