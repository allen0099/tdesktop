---
applyTo: "Telegram/SourceFiles/**/*.{cpp,h}"
---

# C++ Code Guidelines for 64Gram

## Code Modification Rules

When modifying C++ code in this project:

### Minimal Changes
- Make the **smallest possible changes** to achieve the goal
- Do not refactor or "clean up" unrelated code
- Only modify code directly related to the bug fix or improvement
- Preserve existing code structure and patterns

### Code Quality
- Follow existing naming conventions in the file you're modifying
- Use descriptive variable and function names (no abbreviations)
- Keep functions focused and single-purpose
- Avoid deep nesting - prefer early returns

### Memory Management
- Use smart pointers (`std::unique_ptr`, `std::shared_ptr`) for ownership
- Avoid raw `new`/`delete` where possible
- Be careful with Qt object ownership (parent-child relationships)
- Clean up resources in destructors

### Qt Framework Patterns
- Use Qt signals and slots for event handling
- Respect Qt's parent-child object ownership
- Use Qt containers when working with Qt code (`QVector`, `QList`, etc.)
- Use `QObject::connect` for signal-slot connections
- Follow Qt naming conventions for Qt-related code

### Safety
- Check pointers before dereferencing
- Validate array/container indices
- Handle edge cases and error conditions
- Use const references for read-only parameters
- Avoid buffer overflows with proper bounds checking

### Modern C++ Practices
- Use `auto` for complex iterator types, but not to obscure simple types
- Prefer `nullptr` over `NULL` or `0`
- Use range-based for loops when appropriate
- Use `constexpr` for compile-time constants
- Prefer `enum class` over plain `enum`

### Include Organization
- Include headers in this order:
  1. Corresponding header (for .cpp files)
  2. Qt headers
  3. Standard library headers
  4. Project headers
- Use forward declarations in headers to reduce dependencies
- Include only what you use

### Comments
- Don't add comments unless they match existing style
- Comment complex algorithms or non-obvious code
- Use `//` for single-line comments
- Use `/* */` for multi-line comments
- Document public API functions in headers

### Testing Before Submitting
- Build the project successfully
- Test the specific functionality you changed
- Verify no crashes or memory leaks
- Check that related features still work

### Common Patterns in This Codebase
- Check existing similar code for patterns to follow
- Many files use specific coding patterns - replicate them
- Qt meta-object system is used extensively (Q_OBJECT, signals, slots)
- Resource management often uses Qt's parent-child ownership

## What NOT to Do

❌ Do not:
- Add new features without upstream approval
- Change UI/UX without Telegram team approval  
- Modify translation strings (use official translation platform)
- Reformat code or change whitespace unrelated to your fix
- Add new third-party dependencies without strong justification
- Change public APIs without careful consideration
- Break backward compatibility

## Build Verification

After making changes:
```bash
# Configure if needed
cmake -B out -D TDESKTOP_API_ID=YOUR_API_ID -D TDESKTOP_API_HASH=YOUR_API_HASH

# Build
cmake --build out --config Debug

# Run and test
./out/Debug/Telegram  # Or .exe on Windows
```

## Example Good Practices

### Good: Focused fix
```cpp
// Fix null pointer dereference
if (widget && widget->isVisible()) {
    widget->update();
}
```

### Bad: Unnecessary refactoring
```cpp
// Don't refactor unrelated code in the same change
auto w = widget;  // Unnecessary change
if (w != nullptr && w->isVisible()) {  // Unnecessary style change
    w->update();
}
```

### Good: Descriptive names
```cpp
const auto messageCount = messages.size();
const auto isGroupChat = chat->isGroup();
```

### Bad: Unclear abbreviations
```cpp
const auto cnt = msgs.size();  // Unclear
const auto grp = ch->isGrp();  // Unclear
```
