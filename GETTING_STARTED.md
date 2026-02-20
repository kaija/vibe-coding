# Getting Started with Design Templates

Quick guide to navigating and using the design templates and documentation.

## 📚 What's Available

### Design System Templates
- **[Frontend Design](frontend/design-template.md)** - Complete UI/UX design system (2000+ lines)
- **[Mobile iOS Design](mobile-ios/design-template.md)** - iOS-specific design patterns
- **[Mobile Android Design](mobile-android/design-template.md)** - Android-specific design patterns

### Development Templates
- **[Frontend](frontend/template.md)** - React, Vue, Angular best practices
- **[Backend API](backend-api/template.md)** - REST API development
- **[Backend Worker](backend-worker/template.md)** - Background job processing
- **[Database Design](database-design/template.md)** - Database architecture
- **[Mobile iOS](mobile-ios/template.md)** - iOS development
- **[Mobile Android](mobile-android/template.md)** - Android development

## 🔍 How to Find Anything

### The Key Pattern

Use grep to search by section title:

```bash
grep -n "### Section Title" filename.md -A 50
```

- `-n` shows line numbers
- `-A 50` shows 50 lines after the match
- Works even when documents change

## 📖 Common Searches

### Design System

**Find color palette:**
```bash
grep -n "### Primary Colors" frontend/design-template.md -A 30
grep -n "### Background Colors" frontend/design-template.md -A 20
```

**Find typography:**
```bash
grep -n "### Type Scale" frontend/design-template.md -A 40
grep -n "### Font Families" frontend/design-template.md -A 20
```

**Find component specs:**
```bash
grep -n "### Buttons" frontend/design-template.md -A 80
grep -n "### Input Fields" frontend/design-template.md -A 60
grep -n "### Modal" frontend/design-template.md -A 60
grep -n "### Tables" frontend/design-template.md -A 80
grep -n "### Cards" frontend/design-template.md -A 60
```

**Find spacing and layout:**
```bash
grep -n "## Spacing System" frontend/design-template.md -A 40
grep -n "## Layout System" frontend/design-template.md -A 50
```

**Find animations:**
```bash
grep -n "## Animation" frontend/design-template.md -A 100
```

### Development Templates

**Find framework recommendations:**
```bash
grep -n "## Framework Recommendations" frontend/template.md -A 100
```

**Find architecture patterns:**
```bash
grep -n "## Architecture Patterns" frontend/template.md -A 80
```

**Find testing approaches:**
```bash
grep -n "## Testing Approaches" frontend/template.md -A 100
```

**Find deployment info:**
```bash
grep -n "## Deployment Considerations" frontend/template.md -A 80
```

## 🎯 Quick Examples

### List All Sections
```bash
# Main sections (##)
grep -n "^## " frontend/design-template.md

# Subsections (###)
grep -n "^### " frontend/design-template.md
```

### Search by Keyword
```bash
# Find all mentions of "color"
grep -n "color" -i frontend/design-template.md

# Search with context (5 lines before/after)
grep -n "button" -i -C 5 frontend/design-template.md

# Search all files
grep -rn "authentication" *.md
```

### Find Specific Values
```bash
# Find color codes
grep -n "#[0-9A-F]\{6\}" frontend/design-template.md

# Find spacing values
grep -n "px\|rem" frontend/design-template.md | grep spacing

# Find font sizes
grep -n "font-size\|[0-9]\+px" frontend/design-template.md
```

## 💡 Pro Tips

### Create Aliases

Add to `.bashrc` or `.zshrc`:

```bash
# Quick search function
design() {
  grep -n "$1" -i -C 5 --color=always frontend/design-template.md | less -R
}

# Usage: design "button"
```

### Open in Editor
```bash
# Find line number and open
LINE=$(grep -n "### Buttons" frontend/design-template.md | cut -d: -f1)
code --goto frontend/design-template.md:$LINE

# Or with vim
vim +$LINE frontend/design-template.md
```

### Colored Output
```bash
grep -n "button" --color=always frontend/design-template.md | less -R
```

## 🎨 Design System Overview

The design templates include:

- **Color Palette** - Primary, secondary, semantic colors with 50+ tokens
- **Typography** - Font families, scales, weights for web and mobile
- **Spacing** - 4px-based spacing system
- **Layout** - 12-column grid, breakpoints, responsive patterns
- **30+ Components** - Complete specs with all states and variants
- **Animations** - Motion principles, durations, easing functions
- **Accessibility** - WCAG AA compliance guidelines
- **Dark Mode** - Complete dark mode specifications
- **Mobile Patterns** - Touch gestures, mobile-specific UI

## 🛠️ Development Templates Overview

Each template covers:

- **Technology Choices** - Language and framework recommendations
- **Architecture** - Proven patterns for scalable apps
- **Best Practices** - Industry standards and conventions
- **Code Examples** - Real-world, practical examples
- **Testing** - Unit, integration, E2E strategies
- **Deployment** - CI/CD, hosting, monitoring
- **Common Pitfalls** - What to avoid and why

## 📱 Quick Reference

| What You Need | Command |
|---------------|---------|
| List sections | `grep -n "^## " filename.md` |
| Find topic | `grep -n "topic" filename.md -A 50` |
| Search all | `grep -rn "term" *.md` |
| With context | `grep -n "term" -C 5 filename.md` |
| Case-insensitive | `grep -in "term" filename.md` |

## 🚀 Next Steps

1. Browse the design templates to understand the system
2. Try a few grep searches to get comfortable
3. Create your own aliases for frequent searches

**Remember:** `grep -n "### Section" file.md -A 50` is your friend!
