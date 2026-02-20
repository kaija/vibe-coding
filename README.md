# Vibe Coding - Design & Development Templates

A collection of comprehensive templates for modern software development, including detailed design systems and development best practices.

## 📚 Contents

### 🎨 Design Templates
- **[Frontend Design Guide](frontend/design-template.md)** - Complete UI/UX design system (2000+ lines)
- **[Mobile iOS Design Guide](mobile-ios/design-template.md)** - iOS-specific design patterns
- **[Mobile Android Design Guide](mobile-android/design-template.md)** - Android-specific design patterns

### 💻 Development Templates
- **[Frontend Template](frontend/template.md)** - React, Vue, Angular best practices
- **[Backend API Template](backend-api/template.md)** - REST API development guide
- **[Backend Worker Template](backend-worker/template.md)** - Background job processing
- **[Database Design Template](database-design/template.md)** - Database architecture patterns
- **[Mobile iOS Template](mobile-ios/template.md)** - iOS development guide
- **[Mobile Android Template](mobile-android/template.md)** - Android development guide

## 🎯 Quick Start

**New here? Read [GETTING_STARTED.md](GETTING_STARTED.md)** for a quick guide.

### The Key Pattern

Find anything with this simple grep command:

```bash
grep -n "### Section Title" filename.md -A 50
```

This works even when documents change - no need to track line numbers!

## 🔍 Common Searches

### Design System

```bash
# Find color palette
grep -n "### Primary Colors" frontend/design-template.md -A 30

# Find button specs
grep -n "### Buttons" frontend/design-template.md -A 80

# Find typography
grep -n "### Type Scale" frontend/design-template.md -A 40

# List all sections
grep -n "^## " frontend/design-template.md
```

### Development Templates

```bash
# Find framework recommendations
grep -n "## Framework Recommendations" frontend/template.md -A 100

# Find testing strategies
grep -n "## Testing Approaches" frontend/template.md -A 100

# Find deployment info
grep -n "## Deployment Considerations" frontend/template.md -A 80
```

## 🎨 Design System Features

The design templates include comprehensive specifications for:

- **Color Palette** - Primary, secondary, semantic, and neutral colors with dark mode
- **Typography** - Font families, type scales, weights, and usage guidelines
- **Spacing System** - Consistent 4px-based spacing scale
- **Layout System** - 12-column grid, breakpoints, and responsive patterns
- **30+ Components** - Buttons, inputs, modals, tables, charts, navigation, and more
- **Animations** - Motion principles, durations, easing, and micro-interactions
- **Accessibility** - WCAG AA compliance guidelines and implementation
- **Mobile Patterns** - Touch gestures, bottom sheets, and mobile-specific UI
- **Design Tokens** - CSS variables, JavaScript/TypeScript, and Tailwind config

## 🛠️ Development Templates

Each template includes:

- **Technology Recommendations** - Language and framework choices with trade-offs
- **Architecture Patterns** - Proven patterns for scalable applications
- **Best Practices** - Industry-standard practices and conventions
- **Code Examples** - Real-world, copy-paste ready examples
- **Testing Strategies** - Unit, integration, and E2E testing approaches
- **Deployment Guidelines** - CI/CD, hosting, and monitoring
- **Common Pitfalls** - What to avoid and why

## 📁 Directory Structure

```plaintext
vibe-coding/
├── README.md                           # This file
├── GETTING_STARTED.md                  # Quick start guide
├── TEMPLATE_STRUCTURE.md               # Template structure guidelines
├── TEMPLATE_OUTLINE.md                 # Base outline for new templates
├── frontend/
│   ├── template.md                     # Frontend development template
│   └── design-template.md              # Frontend design system (2000+ lines)
├── backend-api/
│   └── template.md                     # Backend API template
├── backend-worker/
│   └── template.md                     # Background worker template
├── database-design/
│   └── template.md                     # Database design template
├── mobile-ios/
│   ├── template.md                     # iOS development template
│   └── design-template.md              # iOS design system
└── mobile-android/
    ├── template.md                     # Android development template
    └── design-template.md              # Android design system
```

## 💡 Pro Tips

### Create Search Aliases

Add to your `.bashrc` or `.zshrc`:

```bash
# Quick search function
design() {
  grep -n "$1" -i -C 5 --color=always frontend/design-template.md | less -R
}

# Usage: design "button"
```

### Open in Editor at Specific Line

```bash
# Find and open in VS Code
LINE=$(grep -n "### Buttons" frontend/design-template.md | cut -d: -f1)
code --goto frontend/design-template.md:$LINE

# Or with vim
vim +$LINE frontend/design-template.md
```

### Search All Files

```bash
# Search all markdown files
grep -rn "authentication" *.md

# Search design guides only
grep -rn "color palette" */design-template.md
```

## 📖 Using the Templates

### For Project Creation

1. **Choose your project type** - Select the appropriate template
2. **Read the template** - Review the complete guide
3. **Adapt to your needs** - Customize recommendations for your project
4. **Reference ongoing** - Keep as a reference throughout development

### For Design Implementation

1. **Review design system** - Understand the color, typography, and spacing
2. **Implement design tokens** - Set up CSS variables or design tokens
3. **Build components** - Follow component specifications
4. **Test accessibility** - Ensure WCAG compliance

## 🤝 Contributing

Contributions welcome! Please:
1. Follow the existing structure and formatting
2. Include practical examples
3. Add grep-friendly section headers (## and ### format)
4. Test all examples before submitting

## 📄 License

MIT License - feel free to use and adapt for your projects

## 💬 Support

For questions or suggestions:
- Open an issue on GitHub
- Submit a pull request
- Check [GETTING_STARTED.md](GETTING_STARTED.md) for common questions

---

**Happy Coding!** 🚀 Build better with consistent design and development patterns.
