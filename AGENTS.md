# AGENTS.md

## Documentation Rules

### Heading Anchors
**ALWAYS add `<a name="anchor-id"></a>` anchors before each `##` heading in documentation files.**

- Anchors are required for all H2 sections
- Example: `<a name="usage"></a>` before `## Usage`
- This has been repeatedly removed by agents - always verify anchors exist after editing

### Verify Before Documenting
- Never document methods that don't exist in the source code
- Check the actual implementation before writing documentation
- If uncertain, grep the codebase to verify method signatures

### API Reference Links
- Do NOT hallucinate API reference pages
- If documentation mentions "see API reference", verify the page exists first
- If no API reference page exists, remove the link rather than creating dead links

### Code Examples
- Keep examples simple and developer-friendly
- Use arrow functions (`fn() =>`) where appropriate
- Remove redundant imports
- Ensure examples are copy-paste friendly

### File Consistency
- When merging content from one file to another, remove the old file
- Update `documentation.md` navigation to remove links to deleted files
- When adding content, add anchors, don't remove existing ones

### Laravel 13 Compatibility
- Use `App\Models\User` not `App\User`
- Use `Application::configure()->withExceptions()` pattern
- Exception namespace is `Yajra\DataTables\Exceptions\Exception` (plural)

### addColumns Method
- Eloquent-only helper method
- Maps column names to model attributes
- Accepts array where integer keys = attribute name, string keys = column => attribute mapping
