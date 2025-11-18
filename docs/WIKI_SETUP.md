# Wiki Documentation Generation Setup Guide

This guide explains how to set up and use the GitHub Wiki documentation generation workflows in your repository.

## Prerequisites

### 1. Enable GitHub Wiki
1. Go to your repository's Settings
2. Scroll down to the "Features" section
3. Check the "Wikis" checkbox to enable the wiki feature

### 2. Initialize the Wiki
The wiki must have at least one page before the workflows can access it:
1. Go to the "Wiki" tab in your repository
2. Click "Create the first page"
3. Add some initial content (even just a title is enough)
4. Save the page

## Workflow Files

### docs-wiki.yml
This is a reusable workflow that can be called from other repositories to generate wiki documentation.

**Usage in calling repository:**
```yaml
name: Generate Wiki Docs
on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  generate-docs:
    uses: Airy59/OntoQA/.github/workflows/docs-wiki.yml@main
    with:
      docs_dir: 'docs/from-wiki'  # Optional, default: 'docs/from-wiki'
    secrets:
      GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # For private repos
```

### wiki-standalone.yml
This workflow runs in the OntoQA repository itself when wiki pages are edited.

## Authentication for Private Repositories

### For Public Repositories
No additional setup is needed. The workflows will clone the wiki using public access.

### For Private Repositories
Private repository wikis require authentication:

1. **Using GITHUB_TOKEN (Recommended for wiki-standalone.yml):**
   - The default `GITHUB_TOKEN` is automatically available in workflows
   - No additional setup needed for repositories using the standalone workflow

2. **Using Personal Access Token (For cross-repository access):**
   - Create a Personal Access Token with `repo` scope
   - Add it as a repository secret named `GH_TOKEN`
   - The workflow will automatically use it for authentication

## Troubleshooting

### Error: "Repository not found"
**Causes and Solutions:**
1. **Wiki not enabled:** Enable wikis in repository settings
2. **Wiki not initialized:** Create at least one wiki page
3. **Private repository:** Add appropriate authentication (see above)
4. **Repository name typo:** Check the repository name in the workflow

### Error: "fatal: repository 'https://github.com/owner/repo.wiki/' not found"
This is the exact error you encountered. The wiki repository URL format is special:
- Correct format: `https://github.com/owner/repo.wiki.git` (note the `.git` extension)
- The workflows now use `git clone` directly instead of `actions/checkout` to handle this

### Wiki Changes Not Triggering Workflow
The `gollum` event only works for the repository where the wiki belongs. It cannot be used in reusable workflows from other repositories.

## How the Fix Works

The original issue was that `actions/checkout@v4` doesn't support the special URL format for GitHub wiki repositories. The fix:

1. **Uses git clone directly:** Instead of using the checkout action, we use `git clone` with the correct wiki URL format
2. **Handles missing wikis gracefully:** If the wiki doesn't exist, it creates a placeholder instead of failing
3. **Supports authentication:** Automatically uses tokens when available for private wikis
4. **Provides clear error messages:** Tells users exactly what needs to be fixed

## Generated Files

The workflows generate several file formats:
- `{repo-name}_embedded.md` - Markdown with base64-embedded images
- `{repo-name}_with_subfolder.md` - Markdown with images in subfolder
- `{repo-name}.pdf` - PDF version (generated from subfolder version)
- `{repo-name}.tex` - LaTeX source
- `images/` - Directory containing downloaded images (for subfolder version)

## Best Practices

1. **Initialize wiki before using workflows:** Always create at least one wiki page before running the workflows
2. **Use meaningful wiki page names:** Use numeric prefixes (01-, 02-) for ordering
3. **Test with public repos first:** Ensure the workflow works before adding authentication complexity
4. **Monitor workflow runs:** Check the Actions tab for any warnings or errors
5. **Keep images in the repository:** Store diagrams in the main repository (e.g., `Graphol/diagrams/`) rather than only in the wiki