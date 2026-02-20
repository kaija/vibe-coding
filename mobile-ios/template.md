# Mobile iOS Project Template

## Introduction

This template provides comprehensive guidance for establishing iOS mobile application projects with modern best practices and consistent patterns. iOS projects focus on building native applications for iPhone, iPad, Apple Watch, and other Apple platforms using Swift or Objective-C.

Use this template when:
- Building native iOS applications for iPhone or iPad
- Creating universal apps that work across Apple devices
- Developing SwiftUI-first modern iOS applications
- Maintaining or modernizing existing UIKit-based applications
- Building apps that leverage Apple ecosystem features (iCloud, HealthKit, ARKit, etc.)

Key concerns for iOS projects include user experience, performance, memory management, App Store compliance, platform conventions, and device compatibility. This template addresses these concerns through structured recommendations for technology choices, architecture patterns, and development practices.

## Language & Technology Choices

### Recommended Languages

**Swift** (Recommended)

- Modern, type-safe language designed specifically for Apple platforms
- Excellent performance with compile-time optimizations
- Strong type system prevents common programming errors
- Concise syntax with powerful features (optionals, generics, protocol-oriented programming)
- First-class support for SwiftUI and modern Apple frameworks
- Active development with regular language improvements
- Large and growing community with extensive resources
- Trade-offs: Evolving language with occasional breaking changes, larger binary sizes than Objective-C

**Objective-C** (Legacy Support)
- Mature language with decades of iOS development history
- Required for maintaining legacy codebases
- Interoperable with Swift through bridging headers
- Smaller binary sizes and faster compilation for large projects
- Access to all iOS APIs and frameworks
- Trade-offs: Verbose syntax, manual memory management (pre-ARC), less modern language features, declining community support

### Technology Stack

**Core Technologies:**
- Xcode IDE for development and debugging
- Swift Package Manager, CocoaPods, or Carthage for dependency management
- iOS SDK with UIKit, SwiftUI, and system frameworks
- Foundation framework for core utilities
- Combine framework for reactive programming

**Build Tools:**
- Xcode Build System - integrated build and compilation
- fastlane - automation for building, testing, and releasing
- xcodebuild - command-line build tool for CI/CD
- SwiftLint - code style and conventions enforcement


## Framework Recommendations

### UI Frameworks

**SwiftUI** (Recommended for new projects)
- Declarative UI framework introduced in iOS 13
- Cross-platform support (iOS, macOS, watchOS, tvOS)
- Live preview in Xcode for rapid iteration
- Automatic support for Dark Mode and accessibility
- State management built into the framework
- Smaller codebase with less boilerplate
- Modern Swift features (property wrappers, result builders)
- Trade-offs: Limited backward compatibility (iOS 13+), some features still maturing, occasional bugs in early versions

**UIKit** (Mature and battle-tested)
- Traditional imperative UI framework since iOS 2
- Complete feature set with decades of refinement
- Extensive third-party library ecosystem
- Full control over UI behavior and performance
- Better for complex custom UI and animations
- Supports iOS 2+ for maximum compatibility
- Trade-offs: More verbose code, manual layout management, steeper learning curve for beginners

**Hybrid Approach** (Recommended for transitioning projects)
- Use SwiftUI for new features and screens
- Maintain UIKit for existing complex views
- Bridge between frameworks using UIHostingController and UIViewRepresentable
- Gradual migration path from UIKit to SwiftUI
- Leverage strengths of both frameworks

### Supporting Frameworks

**Networking:**
- URLSession (built-in) - native HTTP networking
- Alamofire - elegant HTTP networking library
- Moya - network abstraction layer built on Alamofire
- async/await (Swift 5.5+) - modern asynchronous programming

**Data Persistence:**
- Core Data - Apple's object graph and persistence framework
- SwiftData (iOS 17+) - modern Swift-native persistence
- Realm - mobile database with reactive queries
- UserDefaults - simple key-value storage
- Keychain - secure credential storage
- FileManager - file system access

**Reactive Programming:**
- Combine - Apple's reactive framework
- RxSwift - reactive extensions for Swift
- AsyncSequence - Swift's async iteration

**Dependency Injection:**
- Swinject - lightweight DI container
- Resolver - compile-time safe DI
- Manual DI with protocols and initializers

**Image Loading:**
- Kingfisher - async image downloading and caching
- SDWebImage - image loading with cache support
- Nuke - performant image loading

**Testing:**
- XCTest - Apple's testing framework
- Quick/Nimble - BDD-style testing
- SnapshotTesting - snapshot-based UI testing


## Architecture Patterns

### Model-View-Controller (MVC)

**Apple's Traditional Pattern:**
- Model: Data and business logic
- View: UI components (UIView, SwiftUI Views)
- Controller: Mediates between Model and View (UIViewController)
- Built into UIKit with view controller lifecycle
- Simple to understand and implement
- Good for small to medium applications
- Trade-offs: View Controllers can become massive ("Massive View Controller" problem), tight coupling, difficult to test

**Best Practices:**
- Keep view controllers focused on coordination
- Extract business logic into separate service classes
- Use child view controllers to break up complexity
- Limit view controller responsibilities

### Model-View-ViewModel (MVVM)

**Recommended for SwiftUI and Modern Apps:**
- Model: Data structures and business logic
- View: SwiftUI views or UIKit views
- ViewModel: Presentation logic and state management
- Natural fit for SwiftUI's data binding
- Improved testability (test ViewModels without UI)
- Clear separation of concerns
- Reactive updates with Combine or property observers
- Trade-offs: Additional layer of abstraction, learning curve for data binding

**Implementation Pattern:**
```swift
class UserViewModel: ObservableObject {
    @Published var users: [User] = []
    @Published var isLoading = false
    @Published var errorMessage: String?
    
    private let userService: UserService
    
    func fetchUsers() async {
        isLoading = true
        do {
            users = try await userService.getUsers()
        } catch {
            errorMessage = error.localizedDescription
        }
        isLoading = false
    }
}
```

### VIPER (View-Interactor-Presenter-Entity-Router)

**Recommended for Large, Complex Applications:**
- View: Displays data and forwards user actions
- Interactor: Business logic and data operations
- Presenter: Formats data for display and handles user input
- Entity: Data models
- Router: Navigation logic
- Maximum separation of concerns
- Highly testable with clear boundaries
- Scales well for large teams
- Trade-offs: High complexity, lots of boilerplate, overkill for simple apps

**Use When:**
- Large applications with complex business logic
- Multiple developers working on same features
- Need for maximum testability
- Long-term maintenance is priority

### The Composable Architecture (TCA)

**Recommended for Complex State Management:**
- Unidirectional data flow architecture
- Composable features with clear dependencies
- Built-in testing support
- Side effect management
- Time-travel debugging
- Excellent for complex state and navigation
- Trade-offs: Steep learning curve, verbose for simple features, third-party dependency

**Use When:**
- Complex state management requirements
- Need for predictable state updates
- Testing is critical priority
- Team familiar with Redux-like patterns

### Clean Architecture

**Layered Approach:**
- Presentation Layer: UI and view models
- Domain Layer: Business logic and use cases
- Data Layer: Repositories and data sources
- Dependency rule: inner layers don't depend on outer layers
- Framework-independent business logic
- Highly testable and maintainable
- Trade-offs: More files and abstractions, initial setup complexity

### Directory Structure

**Feature-Based Structure (Recommended):**
```plaintext
MyApp/
├── App/
│   ├── AppDelegate.swift
│   └── SceneDelegate.swift
├── Features/
│   ├── Authentication/
│   │   ├── Views/
│   │   ├── ViewModels/
│   │   ├── Models/
│   │   └── Services/
│   ├── Home/
│   └── Profile/
├── Core/
│   ├── Networking/
│   ├── Storage/
│   ├── Extensions/
│   └── Utilities/
├── Resources/
│   ├── Assets.xcassets
│   ├── Localizable.strings
│   └── Info.plist
└── Tests/
    ├── UnitTests/
    └── UITests/
```

**Layer-Based Structure:**
```plaintext
MyApp/
├── Models/
├── Views/
├── ViewModels/
├── Services/
├── Repositories/
├── Utilities/
└── Resources/
```


## Best Practices

### General Principles

**iOS Design Guidelines:**
- Follow Apple's Human Interface Guidelines (HIG)
- Use native UI components and patterns
- Respect platform conventions (navigation, gestures, controls)
- Support both light and dark mode
- Design for different screen sizes and orientations
- Provide appropriate feedback for user actions

**Code Organization:**
- Keep files focused on single responsibility
- Use extensions to organize code by functionality
- Group related types together
- Limit file size (under 300-400 lines as guideline)
- Use meaningful names that convey intent

**Performance:**
- Profile with Instruments before optimizing
- Lazy load data and views when appropriate
- Use background threads for heavy operations
- Implement efficient table/collection view cell reuse
- Optimize image loading and caching
- Monitor memory usage and fix leaks
- Reduce app launch time

**Memory Management:**
- Understand ARC (Automatic Reference Counting)
- Avoid retain cycles with weak and unowned references
- Use capture lists in closures [weak self]
- Profile with Instruments to detect leaks
- Be careful with delegates (use weak references)
- Release resources in deinit when needed

**Accessibility:**
- Provide accessibility labels for all interactive elements
- Support VoiceOver screen reader
- Ensure sufficient color contrast
- Support Dynamic Type for text sizing
- Test with accessibility features enabled
- Use semantic UI elements

**Security:**
- Store sensitive data in Keychain, not UserDefaults
- Use App Transport Security (HTTPS only)
- Validate all user input
- Implement certificate pinning for sensitive apps
- Use biometric authentication when appropriate
- Obfuscate sensitive code and API keys

### Common Pitfalls to Avoid

- Massive view controllers with too many responsibilities
- Retain cycles causing memory leaks
- Blocking the main thread with heavy operations
- Not handling errors and edge cases
- Ignoring memory warnings
- Hardcoding strings instead of using localization
- Not testing on real devices
- Ignoring App Store review guidelines
- Not supporting different screen sizes
- Over-engineering with unnecessary abstractions


## Code Style & Formatting

### Naming Conventions

**Types:**
- PascalCase for classes, structs, enums, protocols: `UserProfile`, `NetworkManager`, `Codable`
- Descriptive names that indicate purpose
- Protocol names often end with -able, -ing, or describe capability: `Drawable`, `Networking`, `DataSource`

**Functions and Variables:**
- camelCase for functions, methods, and variables: `fetchUserData`, `isLoading`, `userName`
- Boolean variables with is/has/should prefix: `isEnabled`, `hasData`, `shouldRefresh`
- Action methods start with verbs: `handleTap`, `didSelectRow`, `willAppear`

**Constants:**
- camelCase for constants: `maxRetryAttempts`, `defaultTimeout`
- Use static let in enums or structs for namespacing

**Enums:**
- PascalCase for enum name, camelCase for cases
```swift
enum NetworkError {
    case invalidURL
    case noConnection
    case serverError(code: Int)
}
```

**Files:**
- Match file name to primary type: `UserViewController.swift`, `NetworkManager.swift`
- Group related extensions: `String+Extensions.swift`
- Test files: `UserViewModelTests.swift`

### File Organization

**Use Extensions for Organization:**
```swift
// MARK: - UserViewController

class UserViewController: UIViewController {
    // Properties
    private let viewModel: UserViewModel
    
    // Lifecycle
    override func viewDidLoad() {
        super.viewDidLoad()
        setupUI()
    }
}

// MARK: - UI Setup
extension UserViewController {
    private func setupUI() {
        // UI setup code
    }
}

// MARK: - UITableViewDelegate
extension UserViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        // Implementation
    }
}
```

**Property Organization:**
- IBOutlets first
- Public properties
- Private properties
- Computed properties
- Lifecycle methods
- Public methods
- Private methods
- Protocol conformances in extensions

### Formatting Rules

**Use SwiftLint:**
- Enforce consistent code style across team
- Configure rules in .swiftlint.yml
- Integrate with Xcode for real-time feedback
- Run in CI/CD pipeline

**Code Style:**
- 4-space indentation (Xcode default)
- Max line length: 120 characters
- Use trailing closures when appropriate
- Prefer implicit returns in single-expression closures
- Use type inference when type is obvious
- Avoid force unwrapping (!) except in tests or guaranteed cases

**Swift Style Guidelines:**
- Follow Swift API Design Guidelines
- Use guard for early returns
- Prefer structs over classes when possible
- Use protocols for abstraction
- Leverage Swift's type system
- Use optionals appropriately

### Documentation Standards

**Code Documentation:**
```swift
/// Fetches user data from the remote server.
///
/// - Parameters:
///   - userId: The unique identifier for the user
///   - completion: Closure called with result or error
/// - Returns: A cancellable network request
/// - Throws: NetworkError if request fails
func fetchUser(id userId: String, completion: @escaping (Result<User, Error>) -> Void) -> Cancellable {
    // Implementation
}
```

**Comments:**
- Use /// for documentation comments
- Use // for implementation comments
- Explain "why" not "what"
- Use MARK: for section organization
- Use TODO: and FIXME: for temporary notes
- Keep comments up to date with code


## Testing Approaches

### Testing Strategy

Implement a comprehensive testing pyramid:
- Many unit tests for business logic and view models
- Moderate integration tests for services and data layer
- Focused UI tests for critical user flows
- Snapshot tests for visual regression testing

Focus on testing behavior and outcomes rather than implementation details.

### Unit Testing

**XCTest Framework:**
- Apple's built-in testing framework
- Test business logic, view models, and utilities
- Use XCTAssert functions for validation
- Organize tests with setUp() and tearDown()
- Use test doubles (mocks, stubs, fakes) for dependencies

**Example Approach:**
```swift
class UserViewModelTests: XCTestCase {
    var sut: UserViewModel!
    var mockUserService: MockUserService!
    
    override func setUp() {
        super.setUp()
        mockUserService = MockUserService()
        sut = UserViewModel(userService: mockUserService)
    }
    
    override func tearDown() {
        sut = nil
        mockUserService = nil
        super.tearDown()
    }
    
    func testFetchUsers_Success_UpdatesUsers() async {
        // Given
        let expectedUsers = [User(id: "1", name: "Test")]
        mockUserService.usersToReturn = expectedUsers
        
        // When
        await sut.fetchUsers()
        
        // Then
        XCTAssertEqual(sut.users, expectedUsers)
        XCTAssertFalse(sut.isLoading)
        XCTAssertNil(sut.errorMessage)
    }
    
    func testFetchUsers_Failure_SetsErrorMessage() async {
        // Given
        mockUserService.shouldFail = true
        
        // When
        await sut.fetchUsers()
        
        // Then
        XCTAssertTrue(sut.users.isEmpty)
        XCTAssertNotNil(sut.errorMessage)
    }
}
```

**Testing Best Practices:**
- Test one thing per test method
- Use descriptive test names: `test_WhatYouTest_Condition_ExpectedResult`
- Follow Arrange-Act-Assert (Given-When-Then) pattern
- Keep tests independent and isolated
- Use test doubles to isolate system under test
- Test edge cases and error conditions

### UI Testing

**XCUITest Framework:**
- Test user interactions and flows
- Run on simulator or real devices
- Test critical paths and happy flows
- Keep UI tests focused and maintainable

**Example Approach:**
```swift
class LoginUITests: XCTestCase {
    var app: XCUIApplication!
    
    override func setUp() {
        super.setUp()
        continueAfterFailure = false
        app = XCUIApplication()
        app.launch()
    }
    
    func testLogin_ValidCredentials_ShowsHomeScreen() {
        // Given
        let emailField = app.textFields["emailTextField"]
        let passwordField = app.secureTextFields["passwordTextField"]
        let loginButton = app.buttons["loginButton"]
        
        // When
        emailField.tap()
        emailField.typeText("test@example.com")
        passwordField.tap()
        passwordField.typeText("password123")
        loginButton.tap()
        
        // Then
        XCTAssertTrue(app.navigationBars["Home"].waitForExistence(timeout: 5))
    }
}
```

**UI Testing Best Practices:**
- Use accessibility identifiers for reliable element selection
- Wait for elements with waitForExistence(timeout:)
- Test on multiple device sizes
- Keep tests independent (reset state between tests)
- Use page object pattern for complex screens
- Limit UI tests to critical flows (they're slow)

### Snapshot Testing

**Visual Regression Testing:**
- Capture reference images of UI components
- Compare against reference on subsequent runs
- Detect unintended visual changes
- Test different configurations (light/dark mode, text sizes)

**Tools:**
- SnapshotTesting (Point-Free) - Swift snapshot testing
- iOSSnapshotTestCase (Uber) - FBSnapshotTestCase fork

**Example:**
```swift
func testUserProfileView_Snapshot() {
    let user = User(name: "John Doe", email: "john@example.com")
    let view = UserProfileView(user: user)
    let hostingController = UIHostingController(rootView: view)
    
    assertSnapshot(matching: hostingController, as: .image(on: .iPhone13))
}
```

### Integration Testing

**Testing Services and Networking:**
- Test API integration with mock servers
- Test data persistence layer
- Test service coordination
- Use URLProtocol for mocking network requests

**Testing Core Data:**
- Use in-memory store for tests
- Test CRUD operations
- Test relationships and cascading deletes
- Test migrations

### Test Coverage Expectations

- Aim for 70-80% code coverage overall
- 90%+ coverage for business logic and view models
- Focus on meaningful tests over coverage metrics
- Test critical paths thoroughly
- Don't test framework code or simple getters/setters
- Test error handling and edge cases

### Testing Tools

**XCTest** - Apple's testing framework
**Quick/Nimble** - BDD-style testing with expressive matchers
**SnapshotTesting** - Snapshot-based testing
**OHHTTPStubs** - Stub network requests
**Mockingbird** - Mocking framework for Swift


## Deployment Considerations

### Build Configuration

**Build Schemes:**
- Debug: Development with debugging symbols, no optimization
- Release: Production with optimizations, stripped symbols
- Staging: Testing environment with production-like settings
- Configure schemes in Xcode for different environments

**Configuration Files:**
- Use .xcconfig files for build settings
- Separate configurations for Dev, Staging, Production
- Store environment-specific values (API URLs, keys)
- Never commit secrets to version control

**Environment Variables:**
```swift
enum Environment {
    static let apiBaseURL: String = {
        #if DEBUG
        return "https://api-dev.example.com"
        #elseif STAGING
        return "https://api-staging.example.com"
        #else
        return "https://api.example.com"
        #endif
    }()
}
```

### Code Signing and Provisioning

**Certificates:**
- Development certificate for local testing
- Distribution certificate for App Store and TestFlight
- Manage in Apple Developer Portal
- Store securely, don't commit to repository

**Provisioning Profiles:**
- Development profile for device testing
- Ad Hoc profile for limited distribution
- App Store profile for App Store distribution
- Automatic signing (recommended) or manual signing

**Automatic Signing:**
- Let Xcode manage certificates and profiles
- Simpler for small teams
- Requires Apple Developer account access
- Recommended for most projects

**Manual Signing:**
- Full control over certificates and profiles
- Required for complex enterprise setups
- More maintenance overhead
- Better for large teams with dedicated DevOps

### TestFlight Distribution

**Internal Testing:**
- Up to 100 internal testers
- No review required
- Instant distribution
- Test new builds quickly

**External Testing:**
- Up to 10,000 external testers
- Requires App Review (lighter than full review)
- Public link distribution option
- Collect feedback from beta testers

**Best Practices:**
- Include release notes for each build
- Use groups to organize testers
- Monitor crash reports and feedback
- Test on multiple device types and iOS versions

### App Store Submission

**Preparation:**
- Increment version and build numbers
- Create App Store Connect listing
- Prepare screenshots for all required device sizes
- Write compelling app description
- Set pricing and availability
- Configure App Store metadata

**Review Process:**
- Submit for review through App Store Connect
- Typical review time: 24-48 hours
- Address any rejection reasons promptly
- Follow App Store Review Guidelines strictly

**Release Options:**
- Manual release after approval
- Automatic release after approval
- Scheduled release at specific date/time
- Phased release (gradual rollout)

### CI/CD Patterns

**Continuous Integration:**
- Run tests on every commit
- Lint code with SwiftLint
- Build verification for all schemes
- Automated dependency updates

**Continuous Deployment:**
- Automated TestFlight uploads
- Deploy to TestFlight on merge to main
- Automated App Store submission (optional)
- Automated screenshot generation

**Recommended Tools:**
- fastlane - iOS automation tool (highly recommended)
- GitHub Actions - CI/CD integrated with GitHub
- Bitrise - mobile-focused CI/CD
- CircleCI - flexible CI/CD platform
- Xcode Cloud - Apple's CI/CD service

**fastlane Example:**
```ruby
lane :beta do
  increment_build_number
  build_app(scheme: "MyApp")
  upload_to_testflight
  slack(message: "New beta build uploaded!")
end

lane :release do
  increment_version_number
  build_app(scheme: "MyApp")
  upload_to_app_store
  add_git_tag
end
```

### Monitoring and Crash Reporting

**Crash Reporting:**
- Firebase Crashlytics - comprehensive crash reporting
- Sentry - error tracking and monitoring
- Xcode Organizer - basic crash reports from App Store

**Analytics:**
- Firebase Analytics - free, comprehensive analytics
- Mixpanel - advanced product analytics
- Amplitude - behavioral analytics
- Apple App Analytics - built-in basic analytics

**Performance Monitoring:**
- Firebase Performance Monitoring
- New Relic Mobile - APM for mobile
- Xcode Instruments - profiling and debugging
- MetricKit - Apple's performance metrics framework

**What to Monitor:**
- Crash-free rate
- App launch time
- Network request performance
- Screen load times
- Memory usage
- Battery impact
- User engagement metrics

### Versioning Strategy

**Semantic Versioning:**
- MAJOR.MINOR.PATCH (e.g., 1.2.3)
- MAJOR: Breaking changes or major features
- MINOR: New features, backward compatible
- PATCH: Bug fixes and minor improvements

**Build Numbers:**
- Increment for every build
- Use sequential numbers or timestamps
- Required to be unique for App Store

**Version Management:**
- Use fastlane for automated versioning
- Tag releases in Git
- Maintain CHANGELOG.md
- Document breaking changes


## UI Development

### UIKit vs SwiftUI

**When to Use SwiftUI:**
- New projects targeting iOS 13+
- Rapid prototyping and iteration
- Cross-platform Apple development
- Simple to moderate UI complexity
- Team comfortable with declarative programming
- Apps that benefit from live preview

**When to Use UIKit:**
- Need to support iOS 12 or earlier
- Complex custom animations and transitions
- Fine-grained control over UI behavior
- Existing large UIKit codebase
- Third-party libraries require UIKit
- Team expertise in UIKit

**Hybrid Approach:**
- Use SwiftUI for new features
- Wrap UIKit views with UIViewRepresentable
- Embed SwiftUI in UIKit with UIHostingController
- Gradual migration strategy

### SwiftUI Patterns

**State Management:**
```swift
struct ContentView: View {
    @State private var count = 0              // Local state
    @StateObject private var viewModel = VM() // Owned object
    @ObservedObject var sharedData: Data      // Passed object
    @EnvironmentObject var settings: Settings // Environment object
    @Binding var isPresented: Bool            // Two-way binding
    
    var body: some View {
        // View code
    }
}
```

**View Composition:**
- Break complex views into smaller components
- Use ViewBuilder for conditional views
- Extract reusable components
- Use view modifiers for styling

**Navigation:**
- NavigationStack for hierarchical navigation (iOS 16+)
- NavigationView for older iOS versions
- Sheet for modal presentation
- FullScreenCover for full-screen modals
- Alert and ConfirmationDialog for dialogs

### UIKit Patterns

**View Controller Lifecycle:**
```swift
class MyViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        // Initial setup
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        // Before view appears
    }
    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        // After view appears
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        // Before view disappears
    }
}
```

**Auto Layout:**
- Use NSLayoutConstraint programmatically
- Use Layout Anchors (recommended)
- Use SnapKit or other DSL libraries
- Support different screen sizes and orientations

**Table and Collection Views:**
- Implement efficient cell reuse
- Use diffable data sources (iOS 13+)
- Implement prefetching for performance
- Use compositional layout for complex layouts

### Responsive Design

**Adaptive Layout:**
- Use size classes (compact, regular)
- Support both portrait and landscape
- Test on different device sizes
- Use stack views for flexible layouts

**Dynamic Type:**
- Support user-selected text sizes
- Use system fonts with text styles
- Test with accessibility text sizes
- Avoid fixed heights for text

**Dark Mode:**
- Use semantic colors (label, systemBackground)
- Provide dark mode assets in asset catalog
- Test in both light and dark modes
- Respect user's appearance preference


## Data Persistence

### Core Data

**Apple's Object Graph Framework:**
- ORM for SQLite database
- Powerful querying with NSFetchRequest
- Relationship management
- Migration support
- iCloud sync support
- Background context for performance

**When to Use:**
- Complex data models with relationships
- Need for advanced querying
- Large datasets
- Offline-first applications
- iCloud sync requirements

**Best Practices:**
```swift
// Use background context for heavy operations
persistentContainer.performBackgroundTask { context in
    // Perform operations
    try? context.save()
}

// Use NSFetchedResultsController for table views
let fetchedResultsController = NSFetchedResultsController(
    fetchRequest: fetchRequest,
    managedObjectContext: context,
    sectionNameKeyPath: nil,
    cacheName: nil
)
```

**Common Patterns:**
- Use one context per view controller
- Save on background thread
- Use batch operations for bulk updates
- Implement proper error handling
- Use lightweight migration when possible

### SwiftData (iOS 17+)

**Modern Swift-Native Persistence:**
- Declarative schema with Swift macros
- Type-safe queries
- Automatic migration
- SwiftUI integration
- Simpler than Core Data

**Example:**
```swift
@Model
class User {
    var id: UUID
    var name: String
    var email: String
    var createdAt: Date
    
    init(name: String, email: String) {
        self.id = UUID()
        self.name = name
        self.email = email
        self.createdAt = Date()
    }
}

// Usage in SwiftUI
@Query var users: [User]
```

### Realm

**Mobile Database Alternative:**
- Fast and efficient
- Reactive queries with notifications
- Easy to use API
- Cross-platform (iOS, Android)
- Realm Sync for backend synchronization

**When to Use:**
- Need for reactive data updates
- Cross-platform mobile development
- Real-time data synchronization
- Simpler API than Core Data

### UserDefaults

**Simple Key-Value Storage:**
- Store small amounts of data
- User preferences and settings
- App configuration
- Simple data types (String, Int, Bool, Data)

**Best Practices:**
```swift
// Use property wrappers for type safety
@AppStorage("isDarkMode") var isDarkMode = false

// Or create a settings manager
class Settings {
    static let shared = Settings()
    private let defaults = UserDefaults.standard
    
    var username: String? {
        get { defaults.string(forKey: "username") }
        set { defaults.set(newValue, forKey: "username") }
    }
}
```

**What NOT to Store:**
- Sensitive data (use Keychain)
- Large amounts of data
- Complex objects (use proper database)

### Keychain

**Secure Credential Storage:**
- Store passwords, tokens, certificates
- Encrypted by system
- Persists across app reinstalls
- Can be shared between apps (with keychain groups)

**Usage:**
```swift
// Use KeychainAccess or similar library
let keychain = Keychain(service: "com.example.app")
keychain["authToken"] = "secret_token"
let token = keychain["authToken"]
```

**What to Store:**
- Authentication tokens
- API keys
- Passwords
- Encryption keys
- Certificates

### File System

**Document Directory:**
- User-generated content
- Files visible to user
- Backed up by iCloud
- Use for documents, images, exports

**Caches Directory:**
- Temporary data
- Can be deleted by system
- Not backed up
- Use for downloaded images, cached data

**Best Practices:**
- Use FileManager for file operations
- Handle file access errors
- Clean up temporary files
- Respect user's storage space
- Use background tasks for large file operations


## Networking

### URLSession

**Apple's Native Networking:**
- Built-in HTTP client
- Support for REST APIs
- Background downloads and uploads
- Authentication handling
- Certificate pinning support

**Modern async/await Pattern:**
```swift
func fetchUsers() async throws -> [User] {
    let url = URL(string: "https://api.example.com/users")!
    let (data, response) = try await URLSession.shared.data(from: url)
    
    guard let httpResponse = response as? HTTPURLResponse,
          (200...299).contains(httpResponse.statusCode) else {
        throw NetworkError.invalidResponse
    }
    
    let users = try JSONDecoder().decode([User].self, from: data)
    return users
}
```

**Traditional Completion Handler Pattern:**
```swift
func fetchUsers(completion: @escaping (Result<[User], Error>) -> Void) {
    let url = URL(string: "https://api.example.com/users")!
    
    URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
            completion(.failure(error))
            return
        }
        
        guard let data = data else {
            completion(.failure(NetworkError.noData))
            return
        }
        
        do {
            let users = try JSONDecoder().decode([User].self, from: data)
            completion(.success(users))
        } catch {
            completion(.failure(error))
        }
    }.resume()
}
```

### Third-Party Libraries

**Alamofire:**
- Elegant HTTP networking
- Request/response serialization
- Parameter encoding
- Authentication
- Network reachability
- Request retry and adaptation

**Moya:**
- Network abstraction layer
- Type-safe API endpoints
- Built on Alamofire
- Testable networking
- Plugin architecture

**Example with Moya:**
```swift
enum UserAPI {
    case getUsers
    case getUser(id: String)
    case createUser(name: String, email: String)
}

extension UserAPI: TargetType {
    var baseURL: URL { URL(string: "https://api.example.com")! }
    
    var path: String {
        switch self {
        case .getUsers: return "/users"
        case .getUser(let id): return "/users/\(id)"
        case .createUser: return "/users"
        }
    }
    
    var method: Moya.Method {
        switch self {
        case .getUsers, .getUser: return .get
        case .createUser: return .post
        }
    }
    
    var task: Task {
        switch self {
        case .getUsers, .getUser:
            return .requestPlain
        case .createUser(let name, let email):
            return .requestParameters(
                parameters: ["name": name, "email": email],
                encoding: JSONEncoding.default
            )
        }
    }
}
```

### API Client Architecture

**Service Layer Pattern:**
```swift
protocol UserServiceProtocol {
    func fetchUsers() async throws -> [User]
    func fetchUser(id: String) async throws -> User
    func createUser(name: String, email: String) async throws -> User
}

class UserService: UserServiceProtocol {
    private let session: URLSession
    private let baseURL: URL
    
    init(session: URLSession = .shared, baseURL: URL) {
        self.session = session
        self.baseURL = baseURL
    }
    
    func fetchUsers() async throws -> [User] {
        let url = baseURL.appendingPathComponent("/users")
        let (data, _) = try await session.data(from: url)
        return try JSONDecoder().decode([User].self, from: data)
    }
}
```

### Error Handling

**Network Error Types:**
```swift
enum NetworkError: Error {
    case invalidURL
    case noConnection
    case invalidResponse
    case serverError(statusCode: Int)
    case decodingError(Error)
    case unknown(Error)
}
```

**Retry Logic:**
- Implement exponential backoff
- Retry on specific error codes
- Limit retry attempts
- Handle rate limiting

### Request/Response Handling

**Codable for JSON:**
```swift
struct User: Codable {
    let id: String
    let name: String
    let email: String
    let createdAt: Date
    
    enum CodingKeys: String, CodingKey {
        case id
        case name
        case email
        case createdAt = "created_at"
    }
}

// Custom date decoding
let decoder = JSONDecoder()
decoder.dateDecodingStrategy = .iso8601
```

**Request Building:**
- Use URLComponents for URL construction
- Implement request interceptors for auth
- Add common headers (Content-Type, Authorization)
- Handle multipart form data for file uploads

### Caching

**URLCache:**
- Built-in HTTP caching
- Configure cache size and policy
- Respect cache-control headers

**Custom Caching:**
- Cache responses in memory or disk
- Implement cache invalidation
- Use cache for offline support

### Network Monitoring

**Reachability:**
- Monitor network connectivity
- Handle offline scenarios
- Queue requests when offline
- Use NWPathMonitor (iOS 12+)

```swift
import Network

class NetworkMonitor {
    static let shared = NetworkMonitor()
    private let monitor = NWPathMonitor()
    private(set) var isConnected = false
    
    func startMonitoring() {
        monitor.pathUpdateHandler = { [weak self] path in
            self?.isConnected = path.status == .satisfied
        }
        monitor.start(queue: DispatchQueue.global())
    }
}
```


## App Lifecycle

### Application Lifecycle

**App States:**
- Not Running: App hasn't been launched
- Inactive: App is in foreground but not receiving events
- Active: App is in foreground and receiving events
- Background: App is executing code in background
- Suspended: App is in background but not executing code

**AppDelegate Methods:**
```swift
class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(_ application: UIApplication, 
                    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // App launched - initialize services
        return true
    }
    
    func applicationWillResignActive(_ application: UIApplication) {
        // About to move from active to inactive
        // Pause ongoing tasks, disable timers
    }
    
    func applicationDidEnterBackground(_ application: UIApplication) {
        // Save data, release resources
        // App has ~5 seconds before suspension
    }
    
    func applicationWillEnterForeground(_ application: UIApplication) {
        // Undo changes made on entering background
    }
    
    func applicationDidBecomeActive(_ application: UIApplication) {
        // Restart paused tasks, refresh UI
    }
}
```

### Scene-Based Lifecycle (iOS 13+)

**SceneDelegate:**
```swift
class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    var window: UIWindow?
    
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, 
              options connectionOptions: UIScene.ConnectionOptions) {
        // Configure and attach window to scene
    }
    
    func sceneDidBecomeActive(_ scene: UIScene) {
        // Scene became active
    }
    
    func sceneWillResignActive(_ scene: UIScene) {
        // Scene about to become inactive
    }
    
    func sceneDidEnterBackground(_ scene: UIScene) {
        // Scene entered background
    }
}
```

**Multi-Window Support:**
- Support multiple windows on iPad
- Handle window creation and destruction
- Manage state per window
- Test multi-window scenarios

### Background Tasks

**Background Modes:**
- Audio: Continue playing audio
- Location: Track location updates
- VoIP: Receive VoIP calls
- Background fetch: Periodic content updates
- Remote notifications: Process push notifications
- Background processing: Longer background tasks

**Background Fetch:**
```swift
func application(_ application: UIApplication, 
                performFetchWithCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
    // Fetch new data
    fetchNewData { result in
        switch result {
        case .success(let hasNewData):
            completionHandler(hasNewData ? .newData : .noData)
        case .failure:
            completionHandler(.failed)
        }
    }
}
```

**Background Processing (iOS 13+):**
```swift
import BackgroundTasks

// Register task
BGTaskScheduler.shared.register(
    forTaskWithIdentifier: "com.example.refresh",
    using: nil
) { task in
    self.handleAppRefresh(task: task as! BGAppRefreshTask)
}

// Schedule task
func scheduleAppRefresh() {
    let request = BGAppRefreshTaskRequest(identifier: "com.example.refresh")
    request.earliestBeginDate = Date(timeIntervalSinceNow: 15 * 60) // 15 minutes
    
    try? BGTaskScheduler.shared.submit(request)
}

// Handle task
func handleAppRefresh(task: BGAppRefreshTask) {
    scheduleAppRefresh() // Schedule next refresh
    
    let queue = OperationQueue()
    queue.maxConcurrentOperationCount = 1
    
    task.expirationHandler = {
        queue.cancelAllOperations()
    }
    
    let operation = RefreshOperation()
    operation.completionBlock = {
        task.setTaskCompleted(success: !operation.isCancelled)
    }
    
    queue.addOperation(operation)
}
```

### Push Notifications

**Remote Notifications:**
- Register for push notifications
- Handle device token
- Process notification payloads
- Handle notification actions

**Implementation:**
```swift
// Request authorization
UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) { granted, error in
    if granted {
        DispatchQueue.main.async {
            UIApplication.shared.registerForRemoteNotifications()
        }
    }
}

// Receive device token
func application(_ application: UIApplication, 
                didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    let token = deviceToken.map { String(format: "%02.2hhx", $0) }.joined()
    // Send token to server
}

// Handle notification
func userNotificationCenter(_ center: UNUserNotificationCenter, 
                           didReceive response: UNNotificationResponse, 
                           withCompletionHandler completionHandler: @escaping () -> Void) {
    let userInfo = response.notification.request.content.userInfo
    // Handle notification tap
    completionHandler()
}
```

**Local Notifications:**
- Schedule notifications locally
- Use for reminders, alarms
- Support notification actions
- Handle notification permissions

### State Restoration

**Preserve and Restore State:**
- Save UI state when app terminates
- Restore state when app relaunches
- Provide seamless user experience
- Use restoration identifiers

**Implementation:**
```swift
// Enable state restoration
func application(_ application: UIApplication, 
                shouldSaveSecureApplicationState coder: NSCoder) -> Bool {
    return true
}

func application(_ application: UIApplication, 
                shouldRestoreSecureApplicationState coder: NSCoder) -> Bool {
    return true
}

// In view controller
override func encodeRestorableState(with coder: NSCoder) {
    super.encodeRestorableState(with: coder)
    coder.encode(selectedIndex, forKey: "selectedIndex")
}

override func decodeRestorableState(with coder: NSCoder) {
    super.decodeRestorableState(with: coder)
    selectedIndex = coder.decodeInteger(forKey: "selectedIndex")
}
```


## Performance Optimization

### Memory Management

**Understanding ARC:**
- Automatic Reference Counting manages memory
- Strong references increase retain count
- Weak references don't increase retain count
- Unowned references assume object exists

**Avoiding Retain Cycles:**
```swift
// Retain cycle - BAD
class ViewController: UIViewController {
    var closure: (() -> Void)?
    
    func setup() {
        closure = {
            self.view.backgroundColor = .red // Captures self strongly
        }
    }
}

// Fixed with capture list - GOOD
class ViewController: UIViewController {
    var closure: (() -> Void)?
    
    func setup() {
        closure = { [weak self] in
            self?.view.backgroundColor = .red
        }
    }
}
```

**Delegate Pattern:**
```swift
protocol MyDelegate: AnyObject {
    func didComplete()
}

class MyClass {
    weak var delegate: MyDelegate? // Always weak for delegates
}
```

**Memory Warnings:**
```swift
override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
    // Release cached data, images
    imageCache.removeAll()
}
```

**Best Practices:**
- Profile with Instruments (Leaks, Allocations)
- Use weak self in closures
- Release large objects when not needed
- Implement didReceiveMemoryWarning
- Use autoreleasepool for loops creating many objects
- Avoid creating unnecessary objects

### Launch Time Optimization

**App Launch Phases:**
1. Pre-main: Dynamic library loading, runtime initialization
2. Main: Application initialization
3. First frame: UI rendering

**Optimization Strategies:**
- Minimize work in application:didFinishLaunching
- Defer non-critical initialization
- Reduce number of frameworks
- Avoid static initializers
- Use lazy loading for resources
- Optimize image assets

**Measuring Launch Time:**
```swift
// Add environment variable: DYLD_PRINT_STATISTICS
// View launch time in console
```

**Target Times:**
- Cold launch: < 400ms (ideal)
- Warm launch: < 200ms
- Resume: < 100ms

### UI Performance

**Smooth Scrolling:**
- Implement efficient cell reuse
- Avoid complex layouts in cells
- Cache cell heights
- Load images asynchronously
- Use prefetching for data

**Rendering Performance:**
- Avoid transparent views when possible
- Reduce view hierarchy depth
- Use opaque views (set isOpaque = true)
- Avoid offscreen rendering
- Cache complex drawings

**Image Optimization:**
- Use appropriate image sizes
- Downscale images to display size
- Use image formats efficiently (HEIC, WebP)
- Implement progressive loading
- Cache decoded images

**Profiling Tools:**
- Instruments Time Profiler
- Instruments Core Animation
- Xcode View Debugger
- Xcode Memory Graph Debugger

### Battery Optimization

**Power-Hungry Operations:**
- Network requests
- Location services
- Background processing
- Animations
- Screen brightness

**Optimization Strategies:**
- Batch network requests
- Use significant location changes instead of continuous
- Defer non-critical work
- Reduce animation complexity
- Use efficient algorithms

**Location Services:**
```swift
// Use appropriate accuracy
locationManager.desiredAccuracy = kCLLocationAccuracyHundredMeters

// Stop when not needed
locationManager.stopUpdatingLocation()

// Use significant change monitoring
locationManager.startMonitoringSignificantLocationChanges()
```

**Background Tasks:**
- Complete background work quickly
- Use background task assertions
- Monitor battery state
- Reduce work when battery is low

### Network Performance

**Optimization Techniques:**
- Implement request caching
- Use HTTP/2 or HTTP/3
- Compress request/response data
- Batch API requests
- Implement pagination
- Use CDN for static assets

**Offline Support:**
- Cache API responses
- Queue failed requests
- Sync when connection restored
- Provide offline UI feedback

### Code Optimization

**Swift Performance:**
- Use value types (structs) when appropriate
- Avoid unnecessary copying
- Use lazy properties
- Implement copy-on-write for large structs
- Profile before optimizing

**Algorithm Efficiency:**
- Choose appropriate data structures
- Avoid nested loops when possible
- Use binary search for sorted data
- Cache computed values
- Implement pagination for large datasets


## App Store Guidelines

### Review Guidelines Overview

**Key Areas:**
- Safety: User privacy, data security, objectionable content
- Performance: Complete app, accurate metadata, working features
- Business: Payments, subscriptions, in-app purchases
- Design: Minimum functionality, completeness, accurate description
- Legal: Privacy policy, terms of service, intellectual property

### Common Rejection Reasons

**Incomplete Information:**
- Missing privacy policy
- Incomplete app description
- Missing demo account credentials
- Insufficient app functionality
- Placeholder content

**Privacy Violations:**
- Accessing data without permission
- Missing privacy manifest (iOS 17+)
- Not explaining data usage
- Sharing data without consent
- Accessing clipboard without user action

**Crashes and Bugs:**
- App crashes on launch
- Broken core functionality
- Unfinished features
- Poor performance
- Memory leaks

**Design Issues:**
- Looks unfinished or placeholder
- Doesn't follow HIG
- Confusing navigation
- Poor user experience
- Missing required features

### Privacy Requirements

**Privacy Manifest (iOS 17+):**
```xml
<!-- PrivacyInfo.xcprivacy -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>NSPrivacyTracking</key>
    <false/>
    <key>NSPrivacyTrackingDomains</key>
    <array/>
    <key>NSPrivacyCollectedDataTypes</key>
    <array>
        <dict>
            <key>NSPrivacyCollectedDataType</key>
            <string>NSPrivacyCollectedDataTypeEmailAddress</string>
            <key>NSPrivacyCollectedDataTypeLinked</key>
            <true/>
            <key>NSPrivacyCollectedDataTypeTracking</key>
            <false/>
            <key>NSPrivacyCollectedDataTypePurposes</key>
            <array>
                <string>NSPrivacyCollectedDataTypePurposeAppFunctionality</string>
            </array>
        </dict>
    </array>
</dict>
</plist>
```

**Permission Descriptions:**
```xml
<!-- Info.plist -->
<key>NSCameraUsageDescription</key>
<string>We need camera access to take profile photos</string>

<key>NSPhotoLibraryUsageDescription</key>
<string>We need photo library access to select profile photos</string>

<key>NSLocationWhenInUseUsageDescription</key>
<string>We need your location to show nearby restaurants</string>

<key>NSMicrophoneUsageDescription</key>
<string>We need microphone access for voice messages</string>
```

**Data Collection:**
- Declare all data collected
- Explain how data is used
- Provide opt-out mechanisms
- Implement data deletion
- Follow GDPR and privacy laws

### In-App Purchases

**StoreKit Integration:**
- Use StoreKit for all purchases
- Implement receipt validation
- Handle purchase restoration
- Support family sharing
- Test with sandbox accounts

**Subscription Guidelines:**
- Clearly explain subscription terms
- Provide easy cancellation
- Show pricing and renewal info
- Handle subscription status
- Implement subscription management

**Purchase Flow:**
```swift
import StoreKit

class StoreManager: NSObject, SKProductsRequestDelegate, SKPaymentTransactionObserver {
    func purchase(productId: String) {
        let payment = SKPayment(product: product)
        SKPaymentQueue.default().add(payment)
    }
    
    func paymentQueue(_ queue: SKPaymentQueue, updatedTransactions transactions: [SKPaymentTransaction]) {
        for transaction in transactions {
            switch transaction.transactionState {
            case .purchased:
                // Unlock content
                SKPaymentQueue.default().finishTransaction(transaction)
            case .failed:
                // Handle failure
                SKPaymentQueue.default().finishTransaction(transaction)
            case .restored:
                // Restore purchase
                SKPaymentQueue.default().finishTransaction(transaction)
            default:
                break
            }
        }
    }
}
```

### App Metadata

**App Name:**
- Maximum 30 characters
- Unique and descriptive
- No generic terms
- No price or promotional text

**Description:**
- Clear and accurate
- Highlight key features
- No misleading claims
- Proper grammar and spelling
- Update for new features

**Keywords:**
- Relevant to app functionality
- No competitor names
- No category names
- Comma-separated
- Maximum 100 characters

**Screenshots:**
- Show actual app functionality
- Required for all device sizes
- No placeholder content
- High quality images
- Localized if possible

**App Preview Videos:**
- Show app in use
- Maximum 30 seconds
- No promotional content
- Actual device footage
- Optional but recommended

### Age Ratings

**Content Rating:**
- Accurately rate content
- Consider all content types
- Include user-generated content
- Update if content changes
- Be conservative with ratings

**Parental Controls:**
- Implement age gates if needed
- Restrict mature content
- Follow COPPA for children's apps
- Provide parental controls

### Submission Checklist

**Before Submission:**
- [ ] Test on multiple devices and iOS versions
- [ ] Fix all crashes and major bugs
- [ ] Complete all app metadata
- [ ] Add privacy policy URL
- [ ] Provide demo account if needed
- [ ] Add all required screenshots
- [ ] Test in-app purchases in sandbox
- [ ] Verify all permissions have descriptions
- [ ] Check for placeholder content
- [ ] Review App Store guidelines
- [ ] Test accessibility features
- [ ] Verify deep links work
- [ ] Test push notifications
- [ ] Check app size and performance

**Review Notes:**
- Provide clear testing instructions
- Include demo account credentials
- Explain non-obvious features
- Note any special requirements
- Mention any third-party content

### Post-Approval

**Monitoring:**
- Watch crash reports
- Monitor user reviews
- Track app analytics
- Check for issues
- Respond to user feedback

**Updates:**
- Fix critical bugs quickly
- Add requested features
- Improve based on feedback
- Keep app current with iOS
- Regular maintenance releases

**Marketing:**
- Respond to reviews
- Promote new features
- Engage with users
- Track app store optimization
- Monitor competitor apps


## Conclusion

This template provides a comprehensive foundation for iOS mobile application projects. Adapt these recommendations to your specific project needs, team size, and requirements. The key is to establish consistent patterns early and evolve them as your project grows.

Remember:
- Follow Apple's Human Interface Guidelines and platform conventions
- Prioritize user experience and performance from the start
- Write tests for critical functionality and business logic
- Implement proper memory management to avoid leaks
- Keep up with iOS updates and new frameworks
- Monitor crash reports and user feedback
- Plan for App Store submission from day one
- Profile and optimize before assuming performance issues
- Support accessibility features for all users
- Iterate based on user feedback and analytics

**Key Takeaways:**
- Choose SwiftUI for new projects, UIKit for legacy support
- Implement MVVM or Clean Architecture for maintainability
- Use Core Data or SwiftData for complex data persistence
- Leverage async/await for modern networking
- Test thoroughly with XCTest and UI tests
- Optimize for memory, battery, and launch time
- Follow App Store guidelines to avoid rejection
- Monitor performance with Instruments and analytics

For questions or suggestions about this template, consult with your team and adapt it to your organization's specific needs and constraints. Stay current with Apple's latest technologies and best practices through WWDC sessions, documentation, and the developer community.

