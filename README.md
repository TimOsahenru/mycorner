# MyCorner

Welcome to **MyCorner**, a Jekyll-based blog and personal site project. This repository aims to provide a simple, customizable platform for sharing posts, showcasing work, and hosting content with minimal overhead.

## 🚀 Project Overview

MyCorner leverages [Jekyll](https://jekyllrb.com/) for static site generation, making it easy to create and manage a personal website or blog. The project is structured for clarity and easy contribution.

## 🛠️ Tech Stack

- **Jekyll**: Static site generator (Ruby-based)
- **HTML/CSS**: For layouts and styling
- **Liquid**: Templating language used by Jekyll
- **Markdown**: For writing posts and documentation

## ⚡ Setup Instructions

1. **Fork the repository:**

   - Click the "Fork" button at the top right of the GitHub page to create your own copy of the repository under your GitHub account.

2. **Clone the repository:**

   ```bash
   git clone https://github.com/TimOsahenru/mycorner.git
   cd mycorner
   ```

3. **Install dependencies:**

   - Make sure you have [Ruby](https://www.ruby-lang.org/en/documentation/installation/) and [Bundler](https://bundler.io/) installed.
   - Then run:
     ```bash
     bundle install
     ```

4. **Serve the site locally:**
   ```bash
   bundle exec jekyll serve
   ```
   Visit `http://localhost:4000` in your browser.

## 🔄 Basic Git Workflow

1. **Create a new branch for your changes:**
   ```bash
   git checkout -b feature/short-description
   ```
2. **Make your changes and commit:**
   ```bash
   git add .
   git commit -m "feat: Add new post about Jekyll basics"
   ```
3. **Push your branch:**
   ```bash
   git push origin feature/short-description
   ```
4. **Open a Pull Request (PR):**
   - Follow PR naming convention: `feat:`, `fix:`, `docs:`, etc.
   - Provide a concise description of your changes.

## 🤝 Contributing Guidelines

- **Branch Naming:** Use `feature/`, `fix/`, or `docs/` prefixes.
- **Commit Messages:** Use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/):
  - `feat:`, `fix:`, `docs:`, `style:`, `refactor:`, etc.
- **Pull Request Titles:** Start with the relevant type (`feat:`, `fix:`, etc.), followed by a short description.
- **Review:** All PRs should be reviewed and verified by at least one other contributor before merging.

## 📁 Directory Structure

```text
mycorner/
├── _posts/        # Blog posts in Markdown
├── _layouts/      # HTML layouts for pages
├── assets/        # Static assets (images, CSS, JS)
├── _site/         # Compiled site output (auto-generated)
├── _config.yml    # Jekyll configuration
├── index.html       # Home page content
└── README.md      # Project documentation
```

- **\_posts/**: Add your blog posts here (`YYYY-MM-DD-title.md`).
- **\_layouts/**: Custom page and post layouts.
- **assets/**: Images, CSS, and JavaScript files.
- **\_site/**: Generated site files (do not edit directly).
- **\_config.yml**: Site settings.
- **index.html**: Main landing page content.

---

✨ **Got an idea, suggestion, or bug to squash? Jump in and help shape MyCorner—your contribution makes this space awesome!**
