---
name: markdown-image-uploader
description: Convert local images referenced by Markdown files into online URL links. Use when Codex needs to scan Markdown image references, upload local image files to a configured image host or object storage provider, replace local paths with remote URLs, preserve unrelated Markdown content, or produce an image URL mapping for Obsidian notes, documentation, blogs, or portable Markdown archives.
---

# Markdown Image Uploader

## Overview

Use this skill to turn local Markdown image references into remote URL references while keeping the Markdown text otherwise unchanged. Prefer a deterministic script for scanning, uploading, replacing, and recording mappings once a storage provider is selected.

## Workflow

1. Identify the Markdown files and their base directories.
2. Scan image references, including Markdown images and HTML `img` tags.
3. Skip references that already use `http://`, `https://`, or `data:`.
4. Resolve local image paths relative to the Markdown file.
5. Upload each resolved image to the configured provider.
6. Replace only the matched image URL/path text.
7. Write or update an image URL mapping file when useful.
8. Verify that all intended local image references were handled.

## Guardrails

- Run a dry run first when modifying existing notes or documentation.
- Preserve unrelated Markdown content, whitespace, frontmatter, and link text.
- Do not upload files that are not referenced unless the user explicitly asks for a broader migration.
- Do not delete local image files unless explicitly requested.
- Ask for provider details when no upload target is configured.
- Prefer hash-based deduplication so repeated local images map to one remote URL.

## Expected Script

When implementing this skill, place the main automation at:

```text
scripts/upload_markdown_images.py
```

Keep the script narrow:

- Accept one or more Markdown paths.
- Support `--dry-run`.
- Support an explicit output mode, such as in-place with backup or writing to a new file.
- Require provider configuration through arguments or environment variables.
- Emit a clear summary of found, skipped, uploaded, reused, missing, and replaced images.

## Validation

After changes, check that:

- The Markdown renders remote image URLs where local images were intended.
- No unrelated text changed.
- Missing images are reported instead of silently ignored.
- The mapping file is enough to audit or rerun the migration.
