# Vibe Coding - Document Index

Quick reference index for all templates. Use grep commands to navigate.

## 📖 How to Use

```bash
# Find any section
grep -n "## Section Name" filename.md -A 50

# List all sections in a file
grep -n "^## " filename.md
```

---

## 🎨 Frontend Design Template

**File:** `frontend/design-template.md` (2068 lines)

```bash
grep -n "^## " frontend/design-template.md
```

### Sections

- Introduction
- Design Principles
- Color Palette
- Typography
- Spacing System
- Layout System
- Component Design
- Animation & Motion
- Elevation & Shadows
- Border Radius
- Icons
- Responsive Design
- Empty States
- Error States
- Accessibility Guidelines
- Dark Mode
- Mobile-Specific Patterns
- Design Tokens
- Component Library
- Design Handoff
- Conclusion

### Quick Access

```bash
# Colors
grep -n "## Color Palette" frontend/design-template.md -A 100

# Typography
grep -n "## Typography" frontend/design-template.md -A 80

# Components
grep -n "## Component Design" frontend/design-template.md -A 800

# Animations
grep -n "## Animation" frontend/design-template.md -A 100
```

---

## 💻 Frontend Development Template

**File:** `frontend/template.md`

```bash
grep -n "^## " frontend/template.md
```

### Sections

- Introduction
- Language & Technology Choices
- Framework Recommendations
- Architecture Patterns
- Best Practices
- Code Style & Formatting
- Testing Approaches
- Deployment Considerations
- State Management
- Routing
- Performance Optimization
- Accessibility
- Build Tools and Development Environment
- Conclusion

### Quick Access

```bash
# Frameworks
grep -n "## Framework Recommendations" frontend/template.md -A 100

# Architecture
grep -n "## Architecture Patterns" frontend/template.md -A 80

# Testing
grep -n "## Testing Approaches" frontend/template.md -A 100

# Deployment
grep -n "## Deployment Considerations" frontend/template.md -A 80
```

---

## 📱 Mobile Templates

### iOS Design
**File:** `mobile-ios/design-template.md`

```bash
grep -n "^## " mobile-ios/design-template.md
```

### Android Design
**File:** `mobile-android/design-template.md`

```bash
grep -n "^## " mobile-android/design-template.md
```

### iOS Development
**File:** `mobile-ios/template.md`

```bash
grep -n "^## " mobile-ios/template.md
```

### Android Development
**File:** `mobile-android/template.md`

```bash
grep -n "^## " mobile-android/template.md
```

---

## 🔧 Backend Templates

### Backend API
**File:** `backend-api/template.md`

```bash
grep -n "^## " backend-api/template.md
```

### Backend Worker
**File:** `backend-worker/template.md`

```bash
grep -n "^## " backend-worker/template.md
```

---

## 🗄️ Database Template

**File:** `database-design/template.md`

```bash
grep -n "^## " database-design/template.md
```

---

## 🔍 Search Tips

### Find Specific Topics

```bash
# Search all files
grep -rn "authentication" *.md

# Search with context
grep -n "button" -C 5 frontend/design-template.md

# Case-insensitive
grep -in "color" frontend/design-template.md
```

### List All Sections

```bash
# Main sections (##)
grep -n "^## " filename.md

# Subsections (###)
grep -n "^### " filename.md
```

### Common Searches

```bash
# Design: Find button specs
grep -n "### Buttons" frontend/design-template.md -A 80

# Design: Find colors
grep -n "### Primary Colors" frontend/design-template.md -A 30

# Dev: Find testing
grep -n "## Testing" frontend/template.md -A 100

# Dev: Find deployment
grep -n "## Deployment" frontend/template.md -A 80
```

---

## 📚 Additional Resources

- **[README.md](README.md)** - Overview and features
- **[GETTING_STARTED.md](GETTING_STARTED.md)** - Quick start guide
- **[TEMPLATE_STRUCTURE.md](TEMPLATE_STRUCTURE.md)** - Template guidelines
- **[TEMPLATE_OUTLINE.md](TEMPLATE_OUTLINE.md)** - Base outline

---

**Remember:** `grep -n "## Section" file.md -A 50`
