# Template Structure Guide

This document defines the standard structure that all vibe coding templates follow. Each template is a comprehensive markdown document covering all aspects of project setup and ongoing development.

## Required Sections

Every template MUST include the following sections in this order:

### 1. Introduction
- Overview of the project type
- When to use this template
- Key concerns and considerations for this project type

### 2. Language & Technology Choices
- Recommended languages/platforms with rationale
- Trade-offs and alternatives
- Version recommendations
- Technology stack overview

### 3. Framework Recommendations
- Primary framework suggestions with descriptions
- Supporting libraries and tools
- Integration considerations
- Ecosystem maturity assessment

### 4. Architecture Patterns
- Recommended structural patterns (MVC, layered, hexagonal, etc.)
- Component organization strategies
- Dependency management approaches
- Scalability considerations

### 5. Best Practices
- General principles for this project type
- Common pitfalls to avoid
- Performance considerations
- Security guidelines

### 6. Code Style & Formatting
- Naming conventions (files, variables, functions, classes)
- File and directory organization
- Code formatting rules
- Documentation standards
- Linting and formatting tool recommendations

### 7. Testing Approaches
- Testing strategy overview
- Unit testing patterns and tools
- Integration testing patterns and tools
- End-to-end testing considerations (where applicable)
- Test coverage expectations
- Mocking and test data strategies

### 8. Deployment Considerations
- Deployment strategies and patterns
- Environment configuration management
- CI/CD pipeline patterns
- Monitoring and observability
- Rollback and recovery procedures
- Infrastructure considerations

### 9. Project-Specific Sections
Additional sections unique to each project type:

**Frontend Projects:**
- State Management
- Routing Patterns
- Component Patterns
- Accessibility
- Performance Optimization
- Build Tools & Bundling

**Backend API Projects:**
- API Design Patterns (REST/GraphQL/gRPC)
- Authentication & Authorization
- Rate Limiting & Throttling
- Error Handling & Logging
- API Documentation
- Security Best Practices

**Backend Worker Projects:**
- Queue Management
- Job Scheduling
- Retry Logic & Error Handling
- Dead Letter Queues
- Concurrency & Worker Pools
- Monitoring & Observability

**Database Design Projects:**
- Schema Design & Normalization
- Migration Management & Versioning
- Indexing Strategies
- Constraints & Data Integrity
- Query Optimization
- Backup & Recovery

## Formatting Guidelines

### Header Hierarchy
- Use `#` for the template title
- Use `##` for main sections (Introduction, Language & Technology Choices, etc.)
- Use `###` for subsections within main sections
- Use `####` for detailed topics within subsections
- Never skip header levels

### Lists
- Use `-` for unordered lists
- Use `1.` for ordered lists when sequence matters
- Maintain consistent indentation (2 spaces per level)

### Code Blocks
- Use triple backticks with language identifier: ```typescript
- Use inline code with single backticks for: `variable names`, `function names`, `file paths`

### Emphasis
- Use **bold** for important terms and concepts
- Use *italics* sparingly for subtle emphasis
- Use > blockquotes for important notes or warnings

### Links
- Include links to official documentation where relevant
- Use descriptive link text, not "click here"

## Content Guidelines

### Rationale
- Always explain WHY a recommendation is made, not just WHAT
- Discuss trade-offs between different approaches
- Provide context for when to choose one option over another

### Generality
- Keep content general enough to apply across different projects
- Use placeholders like `[your-project-name]` for project-specific details
- Focus on patterns and principles rather than specific implementations
- Provide options rather than single prescriptive solutions

### Actionability
- Make recommendations concrete and implementable
- Include example configurations where helpful
- Reference specific tools and versions
- Provide clear next steps

### Completeness
- Cover all aspects of project setup and development
- Address both initial setup and ongoing maintenance
- Include considerations for different scales (small projects vs. large teams)
- Don't assume prior knowledge - explain concepts clearly

## Template Validation Checklist

Before considering a template complete, verify:

- [ ] All 8 required sections are present
- [ ] Project-specific sections are included and comprehensive
- [ ] Language/technology choices include rationale
- [ ] Framework recommendations explain trade-offs
- [ ] Architecture patterns are clearly described
- [ ] Best practices are actionable and specific
- [ ] Code style guidelines are comprehensive
- [ ] Testing approaches cover multiple test types
- [ ] Deployment considerations address CI/CD and monitoring
- [ ] Header hierarchy is correct (no skipped levels)
- [ ] Formatting is consistent throughout
- [ ] Content is general enough to be reusable
- [ ] All recommendations include rationale
- [ ] Links to documentation are included where relevant
- [ ] Template can stand alone as a complete reference

## Usage

When creating a new template:

1. Copy this structure as your starting point
2. Fill in each required section with content specific to the project type
3. Add project-specific sections as defined above
4. Review against the validation checklist
5. Ensure consistency with other templates in formatting and tone
