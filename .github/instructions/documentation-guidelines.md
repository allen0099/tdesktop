---
applyTo: "docs/**/*.md,*.md,.github/**/*.md"
---

# Documentation Guidelines

## Documentation Files in This Project

### Key Documentation
- `README.md` - Project overview and quick start
- `CONTRIBUTING.md` - Contribution guidelines
- `features.md` - List of 64Gram-specific features
- `docs/building-*.md` - Platform-specific build instructions
- `docs/api_credentials.md` - API credential instructions

## Modifying Documentation

### General Principles
- Keep documentation accurate and up-to-date
- Use clear, simple language
- Include examples where helpful
- Follow existing formatting patterns
- Test any commands or procedures you document

### Markdown Formatting
- Use `#` for headers (ATX-style)
- Use code blocks with language specifiers: ```bash or ```cpp
- Use **bold** for emphasis, *italics* sparingly
- Use `backticks` for inline code, commands, file paths
- Use proper link syntax: `[text](url)`

### Build Documentation
When updating build instructions:
- Include all prerequisites clearly
- Provide exact version numbers where relevant
- Include platform-specific notes
- Test commands before documenting them
- Include troubleshooting tips if known

### Code Examples
- Ensure code examples are correct
- Use syntax highlighting (```language)
- Keep examples concise and focused
- Show both command and expected output when relevant

### Links
- Use relative links for internal documentation
- Use absolute URLs for external resources
- Ensure links are not broken
- Prefer official documentation links

### Structure
- Use clear heading hierarchy (don't skip levels)
- Include table of contents for longer documents
- Use lists for step-by-step instructions
- Group related information together

### Common Patterns

#### Command Examples
```bash
# Show the command with comment explaining it
git clone --recursive https://github.com/TDesktop-x64/tdesktop.git

# For multi-line commands, use line continuations
docker run --rm -it \
    -u $(id -u) \
    -v "$PWD:/usr/src/tdesktop" \
    tdesktop:centos_env
```

#### Build Instructions Format
1. **Prepare folder** - Where to build
2. **Obtain API credentials** - How to get them
3. **Install prerequisites** - What to install
4. **Clone and prepare** - Getting the code
5. **Build** - Actual build commands
6. **Run** - How to run the result

#### Placeholders
- Use UPPERCASE for placeholders: `YOUR_API_ID`, `YOUR_API_HASH`
- Use ***BuildPath*** for path placeholders
- Explain placeholders when first introduced

### What to Document
✅ Do document:
- New build steps or requirements
- Changed commands or procedures
- Prerequisites and dependencies
- Platform-specific quirks
- Common errors and solutions

❌ Don't document:
- Implementation details (those go in code comments)
- Temporary workarounds
- Deprecated procedures (remove them instead)
- Internal-only information

### Updating Build Documentation
When build process changes:
1. Update relevant `docs/building-*.md` file
2. Update `README.md` if it affects quick start
3. Test the updated instructions
4. Consider if `CONTRIBUTING.md` needs updates

### Feature Documentation
When documenting features in `features.md`:
- Keep entries concise (one line per feature)
- Number features sequentially
- Include link to announcement if available
- Strike through deprecated features with ~~strikethrough~~

### Contributing Documentation
`CONTRIBUTING.md` should cover:
- What contributions are accepted
- How to submit pull requests
- Code review process
- Commit message guidelines
- Testing requirements

### API Documentation
When updating `docs/api_credentials.md`:
- Keep instructions current with Telegram's process
- Include screenshots if they help
- Provide troubleshooting steps
- Link to official Telegram documentation

### Style Guidelines
- Use present tense ("Configure" not "Configuring")
- Use imperative mood for instructions ("Run the command")
- Be concise but complete
- Use active voice ("The build creates..." not "The output is created...")
- Avoid jargon, or explain it when necessary

### Validation Checklist
Before submitting documentation changes:
- [ ] Spell check completed
- [ ] Links are not broken
- [ ] Commands have been tested (if applicable)
- [ ] Formatting is consistent with rest of document
- [ ] All placeholders are clearly marked
- [ ] No sensitive information included
- [ ] Follows existing style and structure

## Special Documentation

### README.md
- First impression for new users
- Keep it concise
- Link to detailed docs
- Include badges if relevant
- Show a preview/screenshot

### CONTRIBUTING.md  
- Critical for open source projects
- Set clear expectations
- Explain what is/isn't accepted
- Link to related resources
- Keep it welcoming but firm

### CHANGELOG
- Found in `changelog.txt` and `64gram_changelog.txt`
- Document user-visible changes
- Follow existing format
- Include version and date
- Group by type (Added, Fixed, Changed, etc.)

## Localization Documentation
Translation docs should:
- Link to official localization repo
- Explain the translation process
- Provide template links
- Describe file naming conventions

## Examples

### Good Documentation
```markdown
## Building on Windows

1. Install Visual Studio 2022 with C++ support
2. Open **x64 Native Tools Command Prompt for VS 2022**
3. Run the following commands:
   ```bash
   git clone --recursive https://github.com/TDesktop-x64/tdesktop.git
   cd tdesktop
   Telegram\build\prepare\win.bat
   ```
```

### Bad Documentation  
```markdown
## Building on Windows
just install VS and build it
```

The first example is clear, specific, and actionable. The second is too vague.
