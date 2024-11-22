# .editorconfig

```
# top-most EditorConfig file
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = tab
indent_size = 4
tab_width = 4
```

# .github/workflows/release-version.yml

```yml
name: Publish new theme version

on:
  push:
    # Sequence of patterns matched against refs/tags
    tags:
      - "*" # Push events to matching any tag format, i.e. 1.0, 20.15.10

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - name: Bundle
        id: bundle
        run: |
          ls
          echo "::set-output name=tag_name::$(git tag --sort version:refname | tail -n 1)"
      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          VERSION: ${{ github.ref }}
        with:
          tag_name: ${{ github.ref }}
          release_name: ${{ github.ref }}
          draft: false
          prerelease: false
      - name: Upload manifest.json
        id: upload-manifest
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: ./manifest.json
          asset_name: manifest.json
          asset_content_type: application/json
      - name: Upload theme.css
        id: upload-css
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: ./theme.css
          asset_name: theme.css
          asset_content_type: text/css

```

# .gitignore

```
# vscode
.vscode 

# Intellij
*.iml
.idea

# npm
node_modules

# Exclude sourcemaps
*.map

# Exclude macOS Finder (System Explorer) View States
.DS_Store

```

# manifest.json

```json
{
	"name": "Tailwindish",
	"version": "1.0.0",
	"minAppVersion": "1.0.0",
	"author": "Steven Hubert",
	"authorUrl": "https://www.drkpxl.com"
}
```

# package.json

```json
{
    "name": "sample-obsidian-theme",
    "version": "1.0.0",
	"scripts": {
		"version": "node version-bump.mjs && git add manifest.json versions.json"
	}
}

```

# README.md

```md
This is a sample theme for Obsidian ([https://obsidian.md](https://obsidian.md/)).

## First Time publishing a theme?

### Quick start

<img width="244" alt="Pasted image 20220822135601" src="https://user-images.githubusercontent.com/693981/186000386-4f4da987-fcaf-4aa5-aed4-e34b5901255d.png">

First, choose **Use this template**. That will create a copy of this repository (repo) under your Github profile. Then, you will want to _clone_ your new repository to your computer.

Once you have the repo locally on your computer, there are a couple of placeholder fields you will need to fill in.

1. Inside the `manifest.json` file, change the "name" field to whatever you want the name of your theme to be. For example:

  \`\`\`json
  {
    "name": "Moonstone",
    "version": "0.0.0",
    "minAppVersion": "1.0.0"
  }
  \`\`\`

2. Also inside the manifest.json file, you can include your name under next to the "author" field.

After you have those fields configured, all that's left to do is add your styles! All of your CSS needs to be inside the file `theme.css` which is located at root of your repository.

## Adding your theme to the Theme Gallery

### Add a screenshot thumbnail

Inside the repository, include a screenshot thumbnail of your theme. You can name the file anything, for example `screenshot.png`. This image will be used for the small preview in the theme list.

Your screenshot file should be `16:9` aspect ratio.
The recommended size is 512x288.

### Submit your theme for review

To have your theme included in the Theme Gallery, you will need to submit a Pull Request to [`obsidianmd/obsidian-releases`](https://github.com/obsidianmd/obsidian-releases#community-theme).

## Releasing Versions _(Optional)_

If your theme is getting more and more complex, you might want to start thinking about how your theme will stay compatible with different versions of Obsidian. Introduced in v0.16 of Obsidian, themes support [Github Releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository). This means that you can specify which versions of your theme are compatible with which versions of Obsidian.

### Steps for releasing the initial version of your theme (1.0.0)

1. From your theme's repository, click on "Releases".
   
<img width="235" alt="Pasted image 20220822145001" src="https://user-images.githubusercontent.com/693981/186000441-287a1a97-65f6-4b5f-ba66-810ceae91cd3.png">

2. On the Releases page, there should be a button to **Draft a new Release**. Press it.

<img width="202" alt="Pasted image 20220822145048" src="https://user-images.githubusercontent.com/693981/186000664-6c63ae14-f685-4d39-bfe6-324f95cd9669.png">

3. Fill out the Release information form.
	- **Choose a Tag**: Type in the name of the version number here. At the bottom of the dropdown should be a button to create a new tag with your latest theme changes. Choose this option.
		<img width="340" alt="Pasted image 20220822145648" src="https://user-images.githubusercontent.com/693981/186000848-bd1c2619-ea09-4e70-a886-40769cda6921.png">
	- **Release Title**: This can be the version number.
	- **Description** _Optional_: Anything that changed
	- **Files:** The most important part of this form is uploading the files. You can do this by dragging 'n dropping the `manifest.json` file and the `theme.css` file your for theme inside the file upload field.

<img width="946" alt="Pasted image 20220822145356" src="https://user-images.githubusercontent.com/693981/186000772-e689ecea-c3b7-4e9d-9204-7ad62c0123aa.png">

4. Click "Publish Release."
5. Make sure that `versions.json` is set up correctly. This file is a map.
  \`\`\`json
  {
    "1.0.0": "0.16.0"
  }
  \`\`\`
  
  This means that version 1.0.0 of your theme is compatible with version 0.16.0 of Obsidian. For the initial release of your theme, you shouldn't need to make any changes to this file.
 
### Steps for releasing new versions

Releasing a new version of your theme is the same as releasing the initial version.

1. From your theme's repository, click on "Releases."
2. On the Releases page, there should be a button to **Draft a new Release**. Press it.
3. Fill out the Release information form.
	- **Choose a Tag**: Type in the name of the version number here. At the bottom of the dropdown should be a button to create a new tag with your latest theme changes. Choose this option.
		<img width="333" alt="Pasted image 20220822145812" src="https://user-images.githubusercontent.com/693981/186000912-f494def9-0f67-4662-92bf-bd278082455f.png">
	- **Release Title**: This can be the version number.
	- **Description** _Optional_: Anything that changed
	- **Files:** The most important part of this form is uploading the files. You can do this by dragging 'n dropping the `manifest.json` file and the `theme.css` file your for theme inside the file upload field.

4. Click "Publish Release."
5. Update the `versions.json` file in your repository. For the initial release of your theme, you probably didn't need to make any changes to the `versions.json` file. When you release subsequent versions of your theme; however, it's best practice to include the new version as entry in the versions.json file. So this might look like:
  \`\`\`json
  {  
		"1.0.0": "0.16.0",
		"1.0.1": "0.16.0"
  }
  \`\`\`

  What's important to note here is: the new version is included as the "key" and the "value" is the minimum version of Obsidian that your theme compatible with. So if the new version of your theme is only compatible with an Insider version of Obsidian, it's important to set this value accordingly. This will prevent users on older versions of Obsidian from updating to the newer version of your theme.

```

# theme.css

```css
/* Light mode */
body.theme-light {
	--color-bg-primary: #ffffff;
	--color-bg-secondary: #f8f9fa;
	--color-text-primary: #1a202c;
	--color-text-secondary: #718096;
	--color-accent: #3182ce;
	--color-accent-hover: #2b6cb0;
	--color-border: #e2e8f0;
  
	background-color: var(--color-bg-primary);
	color: var(--color-text-primary);
  }
  
  body.theme-light .markdown-preview-view,
  body.theme-light .workspace-leaf-content {
	background-color: var(--color-bg-secondary);
	border: 1px solid var(--color-border);
  }
  
  /* Dark mode */
  body.theme-dark {
	--color-bg-primary: #1a202c;
	--color-bg-secondary: #2d3748;
	--color-text-primary: #edf2f7;
	--color-text-secondary: #a0aec0;
	--color-accent: #63b3ed;
	--color-accent-hover: #4299e1;
	--color-border: #4a5568;
  
	background-color: var(--color-bg-primary);
	color: var(--color-text-primary);
  }
  
  body.theme-dark .markdown-preview-view,
  body.theme-dark .workspace-leaf-content {
	background-color: var(--color-bg-secondary);
	border: 1px solid var(--color-border);
  }
  
  /* Shared styles */
  body {
	--radius-sm: 4px;
	--radius-md: 8px;
	--spacing-sm: 0.5rem;
	--spacing-md: 1rem;
	--font-sans: ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
	--font-mono: ui-monospace, "SFMono-Regular", Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
  
	font-family: var(--font-sans);
	line-height: 1.5;
  }
  
  /* Links */
  a {
	color: var(--color-accent);
	text-decoration: none;
  }
  
  a:hover {
	color: var(--color-accent-hover);
	text-decoration: underline;
  }
  
  /* Headings */
  h1, h2, h3, h4, h5, h6 {
	font-weight: 700;
	margin-top: var(--spacing-md);
	margin-bottom: var(--spacing-sm);
  }
  
  h1 {
	font-size: 1.875rem; /* Tailwind: text-3xl */
  }
  
  h2 {
	font-size: 1.5rem; /* Tailwind: text-2xl */
  }
  
  h3 {
	font-size: 1.25rem; /* Tailwind: text-xl */
  }
  
  /* Buttons */
  button {
	background-color: var(--color-accent);
	color: var(--color-bg-primary);
	border: none;
	border-radius: var(--radius-sm);
	padding: var(--spacing-sm) var(--spacing-md);
	font-size: 0.875rem; /* Tailwind: text-sm */
	cursor: pointer;
  }
  
  button:hover {
	background-color: var(--color-accent-hover);
  }
  
  /* Tables */
  table {
	width: 100%;
	border-collapse: collapse;
	margin: var(--spacing-md) 0;
  }
  
  table th,
  table td {
	padding: var(--spacing-sm);
	border: 1px solid var(--color-border);
  }
  
  table th {
	background-color: var(--color-bg-secondary);
	font-weight: bold;
  }
  
  /* Code blocks */
  pre,
  code {
	font-family: var(--font-mono);
	background-color: var(--color-bg-secondary);
	border: 1px solid var(--color-border);
	padding: var(--spacing-sm);
	border-radius: var(--radius-sm);
  }
  
  pre {
	overflow-x: auto;
  }
  
  /* Task lists */
  .task-list-item-checkbox {
	width: 1rem;
	height: 1rem;
	border-radius: var(--radius-sm);
	border: 1px solid var(--color-border);
	margin-right: var(--spacing-sm);
	background-color: var(--color-bg-primary);
  }
  
  /* Graph view colors */
  .graph-view .node {
	fill: var(--color-accent);
  }
  
  .graph-view .edge {
	stroke: var(--color-border);
  }
  
```

# version-bump.mjs

```mjs
/**
 * This script makes it slightly easier to release new versions of your
 * theme. If you are not using Github Releases with your theme, or
 * you are not interested in automating the process, you can safely ignore
 * this script.
 *
 * Usage: `$ npm run version`
 *
 * This script will automatically add a new entry to the versions.json file for
 * the current version of your theme.
 */

import { readFileSync, writeFileSync } from "fs";

const targetVersion = process.env.npm_package_version;

// read minAppVersion from manifest.json and bump version to target version
let manifest = JSON.parse(readFileSync("manifest.json", "utf8"));
const { minAppVersion } = manifest;
manifest.version = targetVersion;
writeFileSync("manifest.json", JSON.stringify(manifest, null, "\t"));

// update versions.json with target version and minAppVersion from manifest.json
let versions = JSON.parse(readFileSync("versions.json", "utf8"));
versions[targetVersion] = minAppVersion;
writeFileSync("versions.json", JSON.stringify(versions, null, "\t"));

```

# versions.json

```json
{
	"1.0.0": "1.0.0"
}
```

