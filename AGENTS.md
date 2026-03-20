# AGENTS.md

## Agent Guidelines for Documentation

When working with this documentation repository, ALWAYS follow these rules:

### Source of Truth
- **Read the `src` folder first** - This folder contains the actual code being documented
- Never document methods that don't exist in the source code
- Verify method signatures by grepping the codebase before documenting
- **Always scan source code for undocumented features** - Before documenting, search for:
  - New traits, methods, or properties in the source
  - Compare source with existing documentation
  - Document any method not yet documented
  - Check return types, parameters, and enum support in the actual code

### Frontmatter
**ALWAYS add VitePress YAML frontmatter at the top of every documentation file.**

```yaml
---
title: "Page Title"
description: "Brief description for SEO"
---
```

- Frontmatter must be the FIRST content in the file (before any headings)
- Include appropriate description for SEO
- The `title` in frontmatter should match the H1 heading

### Heading Anchors
**ALWAYS add `<a name="anchor-id"></a>` anchors before each `##` heading.**

- Anchors are required for all H2 sections
- Example: `<a name="usage"></a>` before `## Usage`
- Verify anchors exist after every edit - they get repeatedly removed

### Internal Links
**Never use `.md` extension in internal links.**

- Correct: `[Link](filename)`
- Wrong: `[Link](filename.md)`
- External URLs (e.g., Packagist, GitHub) may keep their full URL

### API Reference Links
- Do NOT hallucinate API reference pages
- If no API reference page exists, remove the link rather than creating dead links

### Code Examples
- Keep examples simple and developer-friendly
- Use the latest PHP syntax (PHP 8.3+)
- Ensure examples are copy-paste friendly

### Version Constraints
- Use `^13` for laravel-datatables version constraints
- The package follows Laravel's major version: Laravel 13 → laravel-datatables 13
- The package plugins follows Laravel's major version: Laravel 13 → laravel-datatables-<plugin> 13

### File Consistency
- When merging content, remove the old file and update `documentation.md`
- When creating a document, add it to `documentation.md`
- When adding content, add anchors—don't remove existing ones
