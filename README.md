# Piano - Multi-Branch Deployment

This project uses GitHub Pages with multi-branch deployment strategy.

## 🚀 Quick Setup

1. Create the GitHub Actions workflow file:
   - Go to your repository on GitHub
   - Click "Add file" → "Create new file"
   - Name it: `.github/workflows/deploy.yml`
   - Copy the content from `deploy.yml` in this repository
   - Commit the file

2. Enable GitHub Pages:
   - Go to Settings → Pages
   - Source: GitHub Actions
   - Save

## Deployment Structure

- **Root Path (/)**: Deployed from `gh-pages` branch (production)
- **/develop**: Deployed from `develop` branch (staging/testing)
- **/feature/{branch-name}**: Deployed from `feature/*` branches (feature previews)

## Branches

### `gh-pages` (Production)
- Deployed to: `https://piano.orenoid.com/`
- Use for: Production-ready code

### `develop` (Staging)
- Deployed to: `https://piano.orenoid.com/develop`
- Use for: Testing and integration

### `feature/*` (Feature Branches)
- Deployed to: `https://piano.orenoid.com/feature/{branch-name}`
- Use for: Feature development and previews
- Example: Branch `feature/new-keys` → `https://piano.orenoid.com/feature/new-keys`

## How It Works

The GitHub Actions workflow automatically:

1. Triggers on push to `gh-pages`, `develop`, or `feature/*` branches
2. Determines the base path based on the branch name
3. Updates the `<base>` tag in `index.html` with the correct path
4. Deploys to GitHub Pages with the appropriate path

## Creating Feature Branches

```bash
# Create a new feature branch
git checkout -b feature/your-feature-name

# Make your changes
git add .
git commit -m "feat: add your feature"
git push origin feature/your-feature-name

# Your feature will be available at:
# https://piano.orenoid.com/feature/your-feature-name
```

## Merging to Production

```bash
# From feature branch to develop
git checkout develop
git merge feature/your-feature-name
git push origin develop

# From develop to production (gh-pages)
git checkout gh-pages
git merge develop
git push origin gh-pages
```

## Local Development

Open `index.html` directly in your browser to test changes locally before pushing.
