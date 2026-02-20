# Vibe Coding Templates

A comprehensive collection of project creation templates that guide developers through establishing new repositories with consistent patterns and best practices.

## Overview

This template system provides structured guidance for six distinct project types:

- **Frontend** - UI development projects (React, Vue, Svelte, etc.)
- **Backend API** - API service projects (REST, GraphQL, gRPC)
- **Backend Worker** - Background job processing projects
- **Database Design** - Database schema and data modeling projects
- **Mobile iOS** - Native iOS application projects (Swift, SwiftUI, UIKit)
- **Mobile Android** - Native Android application projects (Kotlin, Jetpack Compose)

Each template serves as both an initial project setup guide and ongoing reference documentation, covering everything from language selection through deployment.

## Directory Structure

```plaintext
vibe-coding-templates/
├── README.md                    # This file
├── TEMPLATE_STRUCTURE.md        # Defines the standard template structure
├── TEMPLATE_OUTLINE.md          # Base outline for creating new templates
├── frontend/
│   └── template.md             # Frontend project template
├── backend-api/
│   └── template.md             # Backend API project template
├── backend-worker/
│   └── template.md             # Backend worker project template
├── database-design/
│   └── template.md             # Database design template
├── mobile-ios/
│   └── template.md             # Mobile iOS project template
└── mobile-android/
    └── template.md             # Mobile Android project template
```

## Using the Templates

### For Project Creation

1. **Choose your project type** - Select the folder that matches your project (frontend, backend-api, backend-worker, database-design, mobile-ios, or mobile-android)
2. **Read the template** - Review the complete template for your project type
3. **Adapt to your needs** - Use the template as a guide, adapting recommendations to your specific requirements
4. **Reference ongoing** - Keep the template as a reference throughout development

### For Template Creation

If you need to create a new template or modify existing ones:

1. **Review TEMPLATE_STRUCTURE.md** - Understand the required sections and formatting guidelines
2. **Use TEMPLATE_OUTLINE.md** - Start with the base outline as your template
3. **Follow the checklist** - Ensure all required sections are complete
4. **Maintain consistency** - Keep formatting and structure consistent with other templates

## Template Features

### Comprehensive Coverage
Each template covers all essential aspects of project setup:
- Language and technology choices with rationale
- Framework recommendations with trade-offs
- Architecture patterns and design approaches
- Best practices and common pitfalls
- Code style and formatting conventions
- Testing strategies and approaches
- Deployment considerations and CI/CD

### Adaptable & Reusable
Templates are designed to be:
- General enough to apply across different projects
- Flexible with options rather than prescriptive solutions
- Focused on patterns and principles
- Suitable for both small projects and large teams

### Well-Structured
All templates follow a consistent structure:
- Clear section hierarchy for easy navigation
- Logical, progressive organization
- Consistent formatting throughout
- Standalone documents that can be copied or referenced

## Project-Specific Content

Each template includes specialized sections for its project type:

**Frontend Templates** cover:
- State management patterns
- Component architecture
- Accessibility and performance
- Build tools and bundling

**Backend API Templates** cover:
- API design patterns (REST, GraphQL, gRPC)
- Authentication and authorization
- Error handling and logging
- Security best practices

**Backend Worker Templates** cover:
- Queue management
- Job scheduling and retry logic
- Concurrency and worker pools
- Monitoring and observability

**Database Design Templates** cover:
- Schema design and normalization
- Migration management
- Indexing and query optimization
- Backup and recovery strategies

**Mobile iOS Templates** cover:
- UIKit vs SwiftUI patterns
- iOS app architecture (MVC, MVVM, VIPER, TCA)
- Data persistence (Core Data, SwiftData, Realm)
- App Store submission and TestFlight

**Mobile Android Templates** cover:
- Views vs Jetpack Compose patterns
- Android app architecture (MVVM, MVI, Clean Architecture)
- Data persistence (Room, DataStore)
- Play Store submission and internal testing

## Contributing

When creating or updating templates:

1. Ensure all required sections are present and complete
2. Include rationale for all recommendations
3. Provide trade-offs between different approaches
4. Keep content general and reusable
5. Follow the formatting guidelines in TEMPLATE_STRUCTURE.md
6. Validate against the template checklist

## Design Principles

The template system is built on these core principles:

- **Consistency** - All templates follow the same structural pattern
- **Comprehensiveness** - Each template covers all essential aspects
- **Adaptability** - Templates provide guidance that can be tailored
- **Clarity** - Content is organized logically with clear headers
- **Actionability** - Recommendations are concrete and implementable

## License

[Add your license information here]
