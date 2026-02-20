# Mobile Android Project Template

## Introduction

This template provides comprehensive guidance for establishing Android mobile application projects with modern best practices and consistent patterns. Android projects focus on building native applications for Android phones, tablets, wearables, and other Android-powered devices using Kotlin or Java.

Use this template when:
- Building native Android applications for phones or tablets
- Creating apps that work across different Android devices and versions
- Developing Jetpack Compose-first modern Android applications
- Maintaining or modernizing existing View-based applications
- Building apps that leverage Android ecosystem features (Google Play Services, ML Kit, etc.)

Key concerns for Android projects include user experience, performance, memory management, Play Store compliance, Material Design principles, and device fragmentation. This template addresses these concerns through structured recommendations for technology choices, architecture patterns, and development practices.

## Language & Technology Choices

### Recommended Languages

**Kotlin** (Recommended)

- Modern, concise language officially recommended by Google
- Excellent interoperability with Java code
- Null safety built into the type system
- Coroutines for asynchronous programming
- Extension functions and higher-order functions
- First-class support for Jetpack Compose and modern Android libraries
- Active development with regular language improvements
- Large and growing community with extensive resources
- Trade-offs: Slightly longer compilation times, learning curve for Java developers

**Java** (Legacy Support)
- Mature language with decades of Android development history
- Required for maintaining legacy codebases
- Fully interoperable with Kotlin
- Extensive documentation and resources
- Familiar to many developers
- Trade-offs: More verbose syntax, no null safety, lacks modern language features, declining community support for new projects

### Technology Stack

**Core Technologies:**
- Android Studio IDE for development and debugging
- Gradle for build automation and dependency management
- Android SDK with Jetpack libraries
- Kotlin Standard Library
- AndroidX libraries for backward compatibility

**Build Tools:**
- Gradle Build System - flexible build configuration
- Android Gradle Plugin - Android-specific build tasks
- R8/ProGuard - code shrinking and obfuscation
- Lint - static code analysis for Android

## Framework Recommendations

### UI Frameworks

**Jetpack Compose** (Recommended for new projects)
- Declarative UI toolkit introduced in 2021
- Modern Kotlin-first approach to UI development
- Less boilerplate code than Views
- Powerful state management with remember and State
- Live preview in Android Studio for rapid iteration
- Automatic recomposition when state changes
- Material Design 3 support built-in
- Growing ecosystem of libraries
- Trade-offs: Requires Android 5.0+ (API 21), learning curve for View-based developers, some advanced features still evolving

**Views (XML Layouts)** (Mature and battle-tested)
- Traditional imperative UI framework since Android 1.0
- Complete feature set with years of refinement
- Extensive third-party library ecosystem
- Visual layout editor in Android Studio
- Full control over view behavior and rendering
- Supports all Android versions
- Trade-offs: More verbose code, manual view binding, steeper learning curve for complex UIs

**Hybrid Approach** (Recommended for transitioning projects)
- Use Compose for new features and screens
- Maintain Views for existing complex layouts
- Bridge between frameworks using ComposeView and AndroidView
- Gradual migration path from Views to Compose
- Leverage strengths of both frameworks

### Supporting Frameworks

**Jetpack Libraries:**
- Lifecycle - lifecycle-aware components
- ViewModel - UI-related data holder
- LiveData - observable data holder
- Room - SQLite database abstraction
- Navigation - in-app navigation
- WorkManager - background task scheduling
- DataStore - modern data storage solution
- Paging - paginated data loading
- Hilt - dependency injection

**Networking:**
- Retrofit - type-safe HTTP client
- OkHttp - HTTP client with interceptors
- Ktor Client - Kotlin-first HTTP client
- Kotlin Coroutines - asynchronous programming

**Data Persistence:**
- Room - SQLite ORM with compile-time verification
- DataStore - modern key-value and typed storage
- SharedPreferences - simple key-value storage
- SQLite - direct database access

**Reactive Programming:**
- Kotlin Coroutines & Flow - structured concurrency
- RxJava - reactive extensions for JVM
- LiveData - lifecycle-aware observables

**Dependency Injection:**
- Hilt - recommended DI framework built on Dagger
- Koin - lightweight DI framework for Kotlin
- Dagger - compile-time DI framework
- Manual DI with constructor injection

**Image Loading:**
- Coil - Kotlin-first image loading library
- Glide - fast and efficient image loading
- Picasso - simple image loading

**Testing:**
- JUnit - unit testing framework
- Espresso - UI testing framework
- MockK - mocking library for Kotlin
- Robolectric - unit test framework that runs on JVM

## Architecture Patterns

### Model-View-ViewModel (MVVM)

**Google's Recommended Pattern:**
- Model: Data layer with repositories and data sources
- View: UI layer (Activities, Fragments, Composables)
- ViewModel: Holds UI state and handles business logic
- Natural fit for Jetpack libraries (ViewModel, LiveData, Flow)
- Lifecycle-aware components prevent memory leaks
- Clear separation of concerns
- Highly testable (test ViewModels without UI)
- Trade-offs: Additional layer of abstraction, requires understanding of lifecycle

**Implementation Pattern:**
```kotlin
class UserViewModel(
    private val userRepository: UserRepository
) : ViewModel() {
    private val _users = MutableStateFlow<List<User>>(emptyList())
    val users: StateFlow<List<User>> = _users.asStateFlow()
    
    private val _isLoading = MutableStateFlow(false)
    val isLoading: StateFlow<Boolean> = _isLoading.asStateFlow()
    
    private val _error = MutableStateFlow<String?>(null)
    val error: StateFlow<String?> = _error.asStateFlow()
    
    fun fetchUsers() {
        viewModelScope.launch {
            _isLoading.value = true
            try {
                _users.value = userRepository.getUsers()
                _error.value = null
            } catch (e: Exception) {
                _error.value = e.message
            } finally {
                _isLoading.value = false
            }
        }
    }
}
```

### Model-View-Intent (MVI)

**Unidirectional Data Flow Pattern:**
- Model: Represents UI state
- View: Renders state and emits user intents
- Intent: User actions that trigger state changes
- Single source of truth for UI state
- Predictable state management
- Easy to debug and test
- Time-travel debugging possible
- Trade-offs: More boilerplate, steeper learning curve, can be overkill for simple screens

**Use When:**
- Complex UI state management
- Need for predictable state updates
- Multiple sources of state changes
- Team familiar with Redux-like patterns

### Clean Architecture

**Layered Approach:**
- Presentation Layer: UI, ViewModels, Compose/Views
- Domain Layer: Use cases, business logic, domain models
- Data Layer: Repositories, data sources, API clients, database
- Dependency rule: inner layers don't depend on outer layers
- Framework-independent business logic
- Highly testable and maintainable
- Scales well for large applications
- Trade-offs: More files and abstractions, initial setup complexity, can be over-engineering for small apps

**Layer Structure:**
```kotlin
// Domain Layer
interface UserRepository {
    suspend fun getUsers(): List<User>
}

class GetUsersUseCase(private val repository: UserRepository) {
    suspend operator fun invoke(): Result<List<User>> {
        return try {
            Result.success(repository.getUsers())
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}

// Data Layer
class UserRepositoryImpl(
    private val apiService: ApiService,
    private val userDao: UserDao
) : UserRepository {
    override suspend fun getUsers(): List<User> {
        return apiService.getUsers().map { it.toDomain() }
    }
}

// Presentation Layer
class UserViewModel(
    private val getUsersUseCase: GetUsersUseCase
) : ViewModel() {
    // ViewModel implementation
}
```

### Directory Structure

**Feature-Based Structure (Recommended):**
```plaintext
app/
├── src/
│   ├── main/
│   │   ├── java/com/example/app/
│   │   │   ├── MainActivity.kt
│   │   │   ├── MyApplication.kt
│   │   │   ├── di/
│   │   │   │   └── AppModule.kt
│   │   │   ├── features/
│   │   │   │   ├── auth/
│   │   │   │   │   ├── ui/
│   │   │   │   │   ├── viewmodel/
│   │   │   │   │   ├── domain/
│   │   │   │   │   └── data/
│   │   │   │   ├── home/
│   │   │   │   └── profile/
│   │   │   ├── core/
│   │   │   │   ├── network/
│   │   │   │   ├── database/
│   │   │   │   ├── util/
│   │   │   │   └── ui/
│   │   │   └── navigation/
│   │   ├── res/
│   │   │   ├── layout/
│   │   │   ├── drawable/
│   │   │   ├── values/
│   │   │   └── navigation/
│   │   └── AndroidManifest.xml
│   ├── test/
│   └── androidTest/
└── build.gradle.kts
```

**Layer-Based Structure:**
```plaintext
app/
├── src/main/java/com/example/app/
│   ├── data/
│   │   ├── repository/
│   │   ├── remote/
│   │   └── local/
│   ├── domain/
│   │   ├── model/
│   │   ├── usecase/
│   │   └── repository/
│   ├── presentation/
│   │   ├── ui/
│   │   └── viewmodel/
│   └── di/
```

## Best Practices

### General Principles

**Material Design Guidelines:**
- Follow Material Design 3 principles
- Use Material Components and theming
- Implement consistent navigation patterns
- Support dynamic color (Material You)
- Provide appropriate visual feedback
- Design for different screen sizes and orientations

**Code Organization:**
- Keep classes focused on single responsibility
- Use Kotlin extensions to organize utility functions
- Group related functionality together
- Limit file size (under 300-400 lines as guideline)
- Use meaningful names that convey intent
- Prefer composition over inheritance

**Performance:**
- Profile with Android Profiler before optimizing
- Lazy load data and UI components when appropriate
- Use background threads for heavy operations
- Implement efficient RecyclerView with DiffUtil
- Optimize image loading and caching
- Monitor memory usage and fix leaks
- Reduce app startup time
- Use R8 for code shrinking and optimization

**Memory Management:**
- Avoid memory leaks with lifecycle-aware components
- Use weak references for callbacks when needed
- Release resources in onDestroy/onCleared
- Profile with Memory Profiler to detect leaks
- Be careful with static references to Context
- Use Application context for long-lived objects
- Avoid holding references to Activities/Fragments

**Accessibility:**
- Provide content descriptions for all UI elements
- Support TalkBack screen reader
- Ensure sufficient color contrast (WCAG guidelines)
- Support font scaling
- Test with accessibility features enabled
- Use semantic UI components
- Provide alternative text for images

**Security:**
- Store sensitive data in EncryptedSharedPreferences or Keystore
- Use HTTPS for all network communication
- Validate all user input
- Implement certificate pinning for sensitive apps
- Use biometric authentication when appropriate
- Obfuscate code with R8/ProGuard
- Never hardcode API keys or secrets

### Common Pitfalls to Avoid

- Memory leaks from holding Activity/Fragment references
- Blocking the main thread with heavy operations
- Not handling configuration changes properly
- Ignoring Android lifecycle
- Hardcoding strings instead of using resources
- Not testing on different screen sizes and Android versions
- Ignoring battery optimization
- Over-engineering with unnecessary abstractions
- Not handling edge cases and errors
- Ignoring Play Store policies and guidelines



## Code Style & Formatting

### Naming Conventions

**Types:**
- PascalCase for classes, interfaces, objects: `UserProfile`, `NetworkManager`, `Serializable`
- Descriptive names that indicate purpose
- Interface names describe capability: `Clickable`, `Serializable`, `UserRepository`

**Functions and Variables:**
- camelCase for functions, methods, and variables: `fetchUserData`, `isLoading`, `userName`
- Boolean variables with is/has/should prefix: `isEnabled`, `hasData`, `shouldRefresh`
- Action methods start with verbs: `handleClick`, `onItemSelected`, `onCreate`

**Constants:**
- UPPER_SNAKE_CASE for compile-time constants: `MAX_RETRY_ATTEMPTS`, `DEFAULT_TIMEOUT`
- camelCase for runtime constants: `const val maxRetryAttempts = 3`

**Packages:**
- lowercase with no underscores: `com.example.myapp.feature.auth`
- Use meaningful package names that reflect functionality

**Resources:**
- snake_case for all resource names
- Prefix with type: `activity_main.xml`, `fragment_user.xml`, `ic_launcher.png`
- Drawable prefixes: `ic_` (icons), `bg_` (backgrounds), `img_` (images)
- Layout prefixes: `activity_`, `fragment_`, `item_`, `dialog_`

**Files:**
- Match file name to primary type: `UserViewModel.kt`, `NetworkManager.kt`
- Activity files: `MainActivity.kt`
- Fragment files: `UserFragment.kt`
- Adapter files: `UserAdapter.kt`
- Test files: `UserViewModelTest.kt`

### File Organization

**Class Structure:**
```kotlin
class UserViewModel(
    private val userRepository: UserRepository
) : ViewModel() {
    
    // Companion object
    companion object {
        private const val TAG = "UserViewModel"
    }
    
    // Properties
    private val _users = MutableStateFlow<List<User>>(emptyList())
    val users: StateFlow<List<User>> = _users.asStateFlow()
    
    // Initialization block
    init {
        fetchUsers()
    }
    
    // Public methods
    fun fetchUsers() {
        // Implementation
    }
    
    fun refreshUsers() {
        // Implementation
    }
    
    // Private methods
    private fun handleError(error: Throwable) {
        // Implementation
    }
    
    // Lifecycle methods
    override fun onCleared() {
        super.onCleared()
        // Cleanup
    }
}
```

**Property Organization:**
- Companion object first
- Public properties
- Private properties
- Initialization blocks
- Public methods
- Private methods
- Overridden methods
- Inner classes

### Formatting Rules

**Use ktlint or Android Studio Formatter:**
- Enforce consistent code style across team
- Configure in .editorconfig
- Integrate with Gradle for automated checks
- Run in CI/CD pipeline

**Code Style:**
- 4-space indentation (Android Studio default)
- Max line length: 100-120 characters
- Use trailing commas in multi-line declarations
- One statement per line
- Braces on same line (K&R style)
- Space after control flow keywords

**Kotlin Style Guidelines:**
- Follow Kotlin Coding Conventions
- Use expression bodies for simple functions
- Prefer `val` over `var` when possible
- Use type inference when type is obvious
- Leverage Kotlin's null safety
- Use data classes for data holders
- Prefer sealed classes for restricted hierarchies

**Example:**
```kotlin
// Good
class UserRepository(
    private val apiService: ApiService,
    private val userDao: UserDao,
) {
    suspend fun getUsers(): List<User> = withContext(Dispatchers.IO) {
        apiService.getUsers().map { it.toDomain() }
    }
}

// Avoid
class UserRepository(private val apiService: ApiService, private val userDao: UserDao) {
    suspend fun getUsers(): List<User> {
        return withContext(Dispatchers.IO) {
            return@withContext apiService.getUsers().map { it.toDomain() }
        }
    }
}
```

### Documentation Standards

**KDoc Comments:**
```kotlin
/**
 * Fetches user data from the remote server.
 *
 * @param userId The unique identifier for the user
 * @return Result containing user data or error
 * @throws NetworkException if request fails
 */
suspend fun fetchUser(userId: String): Result<User> {
    // Implementation
}
```

**Comments:**
- Use /** */ for KDoc documentation
- Use // for implementation comments
- Explain "why" not "what"
- Keep comments up to date with code
- Use TODO: and FIXME: for temporary notes
- Remove commented-out code before committing

**XML Documentation:**
```xml
<!-- User profile screen layout -->
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    
    <!-- User avatar image -->
    <ImageView
        android:id="@+id/avatarImage"
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:contentDescription="@string/user_avatar" />
        
</LinearLayout>
```

## Testing Approaches

### Testing Strategy

Implement a comprehensive testing pyramid:
- Many unit tests for business logic, ViewModels, and repositories
- Moderate integration tests for database and API interactions
- Focused UI tests for critical user flows
- Screenshot tests for visual regression testing

Focus on testing behavior and outcomes rather than implementation details.

### Unit Testing

**JUnit Framework:**
- Standard testing framework for Android
- Test business logic, ViewModels, and utilities
- Use assertions for validation
- Organize tests with @Before and @After
- Use test doubles (mocks, fakes) for dependencies

**Example with Coroutines:**
```kotlin
@ExperimentalCoroutinesApi
class UserViewModelTest {
    
    @get:Rule
    val instantExecutorRule = InstantTaskExecutorRule()
    
    @get:Rule
    val mainDispatcherRule = MainDispatcherRule()
    
    private lateinit var viewModel: UserViewModel
    private lateinit var mockRepository: UserRepository
    
    @Before
    fun setup() {
        mockRepository = mockk()
        viewModel = UserViewModel(mockRepository)
    }
    
    @Test
    fun `fetchUsers success updates users state`() = runTest {
        // Given
        val expectedUsers = listOf(User("1", "Test User"))
        coEvery { mockRepository.getUsers() } returns expectedUsers
        
        // When
        viewModel.fetchUsers()
        advanceUntilIdle()
        
        // Then
        assertEquals(expectedUsers, viewModel.users.value)
        assertFalse(viewModel.isLoading.value)
        assertNull(viewModel.error.value)
    }
    
    @Test
    fun `fetchUsers failure sets error message`() = runTest {
        // Given
        val exception = Exception("Network error")
        coEvery { mockRepository.getUsers() } throws exception
        
        // When
        viewModel.fetchUsers()
        advanceUntilIdle()
        
        // Then
        assertTrue(viewModel.users.value.isEmpty())
        assertEquals("Network error", viewModel.error.value)
    }
}
```

**Testing Best Practices:**
- Test one thing per test method
- Use descriptive test names with backticks: `test name with spaces`
- Follow Given-When-Then (Arrange-Act-Assert) pattern
- Keep tests independent and isolated
- Use MockK for mocking in Kotlin
- Test edge cases and error conditions
- Use Turbine for testing Flows

**Testing Coroutines:**
```kotlin
// Use TestDispatcher for coroutine tests
@get:Rule
val mainDispatcherRule = MainDispatcherRule()

@Test
fun testCoroutine() = runTest {
    // Test code with coroutines
    advanceUntilIdle() // Wait for all coroutines to complete
}
```

### UI Testing

**Espresso Framework:**
- Test user interactions and flows
- Run on emulator or real devices
- Test critical paths and happy flows
- Keep UI tests focused and maintainable

**Example:**
```kotlin
@RunWith(AndroidJUnit4::class)
class LoginActivityTest {
    
    @get:Rule
    val activityRule = ActivityScenarioRule(LoginActivity::class.java)
    
    @Test
    fun loginWithValidCredentials_showsHomeScreen() {
        // Given
        onView(withId(R.id.emailEditText))
            .perform(typeText("test@example.com"), closeSoftKeyboard())
        
        onView(withId(R.id.passwordEditText))
            .perform(typeText("password123"), closeSoftKeyboard())
        
        // When
        onView(withId(R.id.loginButton))
            .perform(click())
        
        // Then
        onView(withId(R.id.homeScreen))
            .check(matches(isDisplayed()))
    }
    
    @Test
    fun loginWithInvalidCredentials_showsError() {
        // Given
        onView(withId(R.id.emailEditText))
            .perform(typeText("invalid"), closeSoftKeyboard())
        
        // When
        onView(withId(R.id.loginButton))
            .perform(click())
        
        // Then
        onView(withText(R.string.invalid_email))
            .check(matches(isDisplayed()))
    }
}
```

**Compose UI Testing:**
```kotlin
@get:Rule
val composeTestRule = createComposeRule()

@Test
fun userList_displaysUsers() {
    // Given
    val users = listOf(
        User("1", "John Doe"),
        User("2", "Jane Smith")
    )
    
    // When
    composeTestRule.setContent {
        UserListScreen(users = users)
    }
    
    // Then
    composeTestRule.onNodeWithText("John Doe").assertIsDisplayed()
    composeTestRule.onNodeWithText("Jane Smith").assertIsDisplayed()
}

@Test
fun clickUser_navigatesToDetail() {
    // Given
    val onUserClick = mockk<(User) -> Unit>(relaxed = true)
    
    composeTestRule.setContent {
        UserListItem(
            user = User("1", "John Doe"),
            onClick = onUserClick
        )
    }
    
    // When
    composeTestRule.onNodeWithText("John Doe").performClick()
    
    // Then
    verify { onUserClick(any()) }
}
```

**UI Testing Best Practices:**
- Use resource IDs or test tags for reliable element selection
- Use idling resources for asynchronous operations
- Test on multiple screen sizes and orientations
- Keep tests independent (reset state between tests)
- Use Page Object pattern for complex screens
- Limit UI tests to critical flows (they're slow)
- Use semantics for Compose testing

### Integration Testing

**Testing Room Database:**
```kotlin
@RunWith(AndroidJUnit4::class)
class UserDaoTest {
    
    private lateinit var database: AppDatabase
    private lateinit var userDao: UserDao
    
    @Before
    fun setup() {
        val context = ApplicationProvider.getApplicationContext<Context>()
        database = Room.inMemoryDatabaseBuilder(context, AppDatabase::class.java)
            .allowMainThreadQueries()
            .build()
        userDao = database.userDao()
    }
    
    @After
    fun teardown() {
        database.close()
    }
    
    @Test
    fun insertAndRetrieveUser() = runTest {
        // Given
        val user = UserEntity("1", "John Doe", "john@example.com")
        
        // When
        userDao.insert(user)
        val retrieved = userDao.getUserById("1")
        
        // Then
        assertEquals(user, retrieved)
    }
}
```

**Testing API with MockWebServer:**
```kotlin
class UserApiTest {
    
    private lateinit var mockWebServer: MockWebServer
    private lateinit var apiService: ApiService
    
    @Before
    fun setup() {
        mockWebServer = MockWebServer()
        mockWebServer.start()
        
        val retrofit = Retrofit.Builder()
            .baseUrl(mockWebServer.url("/"))
            .addConverterFactory(GsonConverterFactory.create())
            .build()
        
        apiService = retrofit.create(ApiService::class.java)
    }
    
    @After
    fun teardown() {
        mockWebServer.shutdown()
    }
    
    @Test
    fun getUsers_returnsUserList() = runTest {
        // Given
        val responseBody = """
            [{"id": "1", "name": "John Doe"}]
        """.trimIndent()
        
        mockWebServer.enqueue(
            MockResponse()
                .setResponseCode(200)
                .setBody(responseBody)
        )
        
        // When
        val users = apiService.getUsers()
        
        // Then
        assertEquals(1, users.size)
        assertEquals("John Doe", users[0].name)
    }
}
```

### Screenshot Testing

**Visual Regression Testing:**
- Capture reference screenshots of UI components
- Compare against reference on subsequent runs
- Detect unintended visual changes
- Test different configurations (themes, text sizes)

**Tools:**
- Shot - Facebook's screenshot testing library
- Paparazzi - Render Compose/Views without emulator
- Roborazzi - Screenshot testing with Robolectric

**Example with Paparazzi:**
```kotlin
class UserProfileScreenshotTest {
    
    @get:Rule
    val paparazzi = Paparazzi(
        deviceConfig = DeviceConfig.PIXEL_5,
        theme = "android:Theme.Material3.DayNight"
    )
    
    @Test
    fun userProfile_lightTheme() {
        paparazzi.snapshot {
            UserProfileScreen(
                user = User("1", "John Doe", "john@example.com")
            )
        }
    }
    
    @Test
    fun userProfile_darkTheme() {
        paparazzi.snapshot {
            MaterialTheme(colorScheme = darkColorScheme()) {
                UserProfileScreen(
                    user = User("1", "John Doe", "john@example.com")
                )
            }
        }
    }
}
```

### Test Coverage Expectations

- Aim for 70-80% code coverage overall
- 90%+ coverage for business logic, ViewModels, and repositories
- Focus on meaningful tests over coverage metrics
- Test critical paths thoroughly
- Don't test framework code or simple getters/setters
- Test error handling and edge cases
- Test different Android versions and screen sizes

### Testing Tools

**JUnit 4/5** - Standard testing framework
**MockK** - Mocking library for Kotlin
**Turbine** - Testing library for Flows
**Espresso** - UI testing framework
**Compose Test** - Testing for Jetpack Compose
**Robolectric** - Unit tests that run on JVM
**MockWebServer** - Mock HTTP server for testing
**Truth** - Fluent assertion library by Google



## Deployment Considerations

### Build Configuration

**Build Types:**
- Debug: Development with debugging enabled, no optimization
- Release: Production with R8 optimization, code shrinking
- Staging: Testing environment with production-like settings
- Configure in build.gradle.kts

**Build Configuration Example:**
```kotlin
android {
    buildTypes {
        debug {
            applicationIdSuffix = ".debug"
            versionNameSuffix = "-debug"
            isDebuggable = true
            isMinifyEnabled = false
        }
        
        release {
            isMinifyEnabled = true
            isShrinkResources = true
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
            signingConfig = signingConfigs.getByName("release")
        }
        
        create("staging") {
            initWith(getByName("release"))
            applicationIdSuffix = ".staging"
            versionNameSuffix = "-staging"
            isDebuggable = true
        }
    }
}
```

**Product Flavors:**
```kotlin
android {
    flavorDimensions += "environment"
    
    productFlavors {
        create("dev") {
            dimension = "environment"
            applicationIdSuffix = ".dev"
            buildConfigField("String", "API_BASE_URL", "\"https://api-dev.example.com\"")
        }
        
        create("prod") {
            dimension = "environment"
            buildConfigField("String", "API_BASE_URL", "\"https://api.example.com\"")
        }
    }
}
```

**Environment Variables:**
```kotlin
// Use BuildConfig for environment-specific values
object ApiConfig {
    val baseUrl: String = BuildConfig.API_BASE_URL
    val isDebug: Boolean = BuildConfig.DEBUG
}

// Or use gradle.properties
android {
    defaultConfig {
        buildConfigField("String", "API_KEY", "\"${project.findProperty("API_KEY")}\"")
    }
}
```

### App Signing

**Debug Signing:**
- Automatic debug keystore created by Android Studio
- Located at ~/.android/debug.keystore
- Used for development and testing
- Not suitable for production

**Release Signing:**
- Create release keystore with keytool
- Store keystore securely (never commit to repository)
- Use different keys for different apps
- Keep keystore backup in secure location

**Generate Keystore:**
```bash
keytool -genkey -v -keystore my-release-key.jks \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -alias my-key-alias
```

**Configure Signing in Gradle:**
```kotlin
android {
    signingConfigs {
        create("release") {
            storeFile = file("../keystore/my-release-key.jks")
            storePassword = System.getenv("KEYSTORE_PASSWORD")
            keyAlias = System.getenv("KEY_ALIAS")
            keyPassword = System.getenv("KEY_PASSWORD")
        }
    }
    
    buildTypes {
        release {
            signingConfig = signingConfigs.getByName("release")
        }
    }
}
```

**Play App Signing:**
- Let Google manage your app signing key
- Upload signing key to Play Console
- Google signs your APK/AAB with app signing key
- Recommended for new apps
- Provides key security and recovery

### Internal Testing

**Internal Testing Track:**
- Up to 100 internal testers
- No review required
- Instant distribution
- Test new builds quickly
- Access via Play Console

**Closed Testing (Alpha/Beta):**
- Distribute to specific testers
- Create email lists or Google Groups
- Collect feedback before public release
- Monitor crash reports and ANRs

**Open Testing:**
- Public testing track
- Anyone can join via Play Store link
- Larger testing audience
- Still pre-release status

### Play Store Submission

**Preparation:**
- Increment versionCode and versionName
- Create Play Console listing
- Prepare screenshots for phone, tablet, and TV (if applicable)
- Write compelling app description
- Set pricing and availability
- Configure app content rating
- Add privacy policy URL

**App Bundle (AAB):**
- Recommended format for Play Store
- Smaller download sizes
- Dynamic delivery support
- Automatic APK generation for different devices
- Required for new apps since August 2021

**Build AAB:**
```bash
./gradlew bundleRelease
```

**Review Process:**
- Submit for review through Play Console
- Typical review time: Few hours to few days
- Address any rejection reasons promptly
- Follow Play Store policies strictly

**Release Options:**
- Staged rollout (percentage-based)
- Full rollout to all users
- Scheduled release at specific date/time
- Release to specific countries first

### CI/CD Patterns

**Continuous Integration:**
- Run tests on every commit
- Lint code with Android Lint and ktlint
- Build verification for all variants
- Automated dependency updates
- Static code analysis

**Continuous Deployment:**
- Automated Play Store uploads
- Deploy to internal track on merge to develop
- Deploy to production on release tags
- Automated screenshot generation
- Automated changelog generation

**Recommended Tools:**
- GitHub Actions - CI/CD integrated with GitHub
- GitLab CI - CI/CD for GitLab
- Bitrise - mobile-focused CI/CD
- CircleCI - flexible CI/CD platform
- Jenkins - self-hosted CI/CD

**GitHub Actions Example:**
```yaml
name: Android CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up JDK 17
      uses: actions/setup-java@v3
      with:
        java-version: '17'
        distribution: 'temurin'
    
    - name: Grant execute permission for gradlew
      run: chmod +x gradlew
    
    - name: Run tests
      run: ./gradlew test
    
    - name: Run lint
      run: ./gradlew lint
    
    - name: Build release APK
      run: ./gradlew assembleRelease
    
    - name: Upload APK
      uses: actions/upload-artifact@v3
      with:
        name: app-release
        path: app/build/outputs/apk/release/app-release.apk
```

**Fastlane for Android:**
```ruby
lane :beta do
  gradle(task: "clean assembleRelease")
  upload_to_play_store(
    track: 'internal',
    aab: 'app/build/outputs/bundle/release/app-release.aab'
  )
end

lane :production do
  gradle(task: "clean bundleRelease")
  upload_to_play_store(
    track: 'production',
    aab: 'app/build/outputs/bundle/release/app-release.aab',
    rollout: '0.1' # 10% rollout
  )
end
```

### Monitoring and Crash Reporting

**Crash Reporting:**
- Firebase Crashlytics - comprehensive crash reporting
- Play Console crash reports - built-in crash tracking
- Sentry - error tracking and monitoring
- Bugsnag - error monitoring

**Analytics:**
- Firebase Analytics - free, comprehensive analytics
- Google Analytics - web and mobile analytics
- Mixpanel - advanced product analytics
- Amplitude - behavioral analytics

**Performance Monitoring:**
- Firebase Performance Monitoring
- Android Vitals (Play Console)
- New Relic Mobile - APM for mobile
- Custom performance tracking

**What to Monitor:**
- Crash-free rate (target: 99%+)
- ANR (Application Not Responding) rate
- App startup time
- Network request performance
- Screen load times
- Memory usage
- Battery impact
- User engagement metrics
- Retention rates

**Firebase Crashlytics Setup:**
```kotlin
// In Application class
class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        
        // Enable Crashlytics
        FirebaseCrashlytics.getInstance().setCrashlyticsCollectionEnabled(true)
        
        // Set user identifier
        FirebaseCrashlytics.getInstance().setUserId(userId)
        
        // Log custom events
        FirebaseCrashlytics.getInstance().log("App started")
    }
}

// Log non-fatal exceptions
try {
    // Code that might throw
} catch (e: Exception) {
    FirebaseCrashlytics.getInstance().recordException(e)
}
```

### Versioning Strategy

**Semantic Versioning:**
- versionName: MAJOR.MINOR.PATCH (e.g., "1.2.3")
- MAJOR: Breaking changes or major features
- MINOR: New features, backward compatible
- PATCH: Bug fixes and minor improvements

**Version Code:**
- versionCode: Integer that increases with each release
- Required to be unique and incrementing
- Used by Play Store to determine newest version
- Can use date-based: YYYYMMDDNN (e.g., 2024012001)

**Version Management:**
```kotlin
android {
    defaultConfig {
        versionCode = 1
        versionName = "1.0.0"
    }
}
```

**Automated Versioning:**
```kotlin
// Read version from version.properties
val versionPropsFile = file("version.properties")
val versionProps = Properties()
if (versionPropsFile.exists()) {
    versionProps.load(FileInputStream(versionPropsFile))
}

android {
    defaultConfig {
        versionCode = versionProps["VERSION_CODE"].toString().toInt()
        versionName = versionProps["VERSION_NAME"].toString()
    }
}
```

## UI Development

### Views vs Jetpack Compose

**When to Use Jetpack Compose:**
- New projects targeting Android 5.0+ (API 21)
- Rapid prototyping and iteration
- Modern declarative UI approach
- Team comfortable with Kotlin and reactive programming
- Apps that benefit from live preview
- Simpler state management needs

**When to Use Views:**
- Need to support Android 4.x or earlier
- Complex custom views with specific rendering needs
- Existing large View-based codebase
- Third-party libraries require Views
- Team expertise in View system
- Fine-grained control over view lifecycle

**Hybrid Approach:**
- Use Compose for new features
- Wrap Views with AndroidView in Compose
- Embed Compose in Views with ComposeView
- Gradual migration strategy

### Jetpack Compose Patterns

**State Management:**
```kotlin
@Composable
fun UserScreen(viewModel: UserViewModel = hiltViewModel()) {
    val users by viewModel.users.collectAsStateWithLifecycle()
    val isLoading by viewModel.isLoading.collectAsStateWithLifecycle()
    val error by viewModel.error.collectAsStateWithLifecycle()
    
    UserContent(
        users = users,
        isLoading = isLoading,
        error = error,
        onRefresh = { viewModel.fetchUsers() }
    )
}

@Composable
fun UserContent(
    users: List<User>,
    isLoading: Boolean,
    error: String?,
    onRefresh: () -> Unit
) {
    // UI implementation
}
```

**State Hoisting:**
```kotlin
@Composable
fun SearchBar(
    query: String,
    onQueryChange: (String) -> Unit,
    modifier: Modifier = Modifier
) {
    TextField(
        value = query,
        onValueChange = onQueryChange,
        modifier = modifier
    )
}

// Usage
@Composable
fun SearchScreen() {
    var query by remember { mutableStateOf("") }
    
    SearchBar(
        query = query,
        onQueryChange = { query = it }
    )
}
```

**Side Effects:**
```kotlin
@Composable
fun UserScreen(userId: String) {
    // LaunchedEffect - run suspend functions
    LaunchedEffect(userId) {
        viewModel.loadUser(userId)
    }
    
    // DisposableEffect - cleanup when composable leaves composition
    DisposableEffect(Unit) {
        val listener = setupListener()
        onDispose {
            listener.remove()
        }
    }
    
    // SideEffect - sync Compose state to non-Compose code
    SideEffect {
        analytics.setCurrentScreen("user_screen")
    }
}
```

**Navigation:**
```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()
    
    NavHost(navController = navController, startDestination = "home") {
        composable("home") {
            HomeScreen(
                onNavigateToUser = { userId ->
                    navController.navigate("user/$userId")
                }
            )
        }
        
        composable(
            route = "user/{userId}",
            arguments = listOf(navArgument("userId") { type = NavType.StringType })
        ) { backStackEntry ->
            val userId = backStackEntry.arguments?.getString("userId")
            UserScreen(userId = userId ?: "")
        }
    }
}
```

**Lists:**
```kotlin
@Composable
fun UserList(users: List<User>, onUserClick: (User) -> Unit) {
    LazyColumn {
        items(users, key = { it.id }) { user ->
            UserListItem(
                user = user,
                onClick = { onUserClick(user) }
            )
        }
    }
}

@Composable
fun UserListItem(user: User, onClick: () -> Unit) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .padding(8.dp)
            .clickable(onClick = onClick)
    ) {
        Row(modifier = Modifier.padding(16.dp)) {
            Text(text = user.name, style = MaterialTheme.typography.titleMedium)
        }
    }
}
```

### View System Patterns

**Activity and Fragment Lifecycle:**
```kotlin
class UserActivity : AppCompatActivity() {
    
    private lateinit var binding: ActivityUserBinding
    private val viewModel: UserViewModel by viewModels()
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityUserBinding.inflate(layoutInflater)
        setContentView(binding.root)
        
        setupUI()
        observeViewModel()
    }
    
    private fun setupUI() {
        binding.refreshButton.setOnClickListener {
            viewModel.fetchUsers()
        }
    }
    
    private fun observeViewModel() {
        lifecycleScope.launch {
            repeatOnLifecycle(Lifecycle.State.STARTED) {
                viewModel.users.collect { users ->
                    updateUI(users)
                }
            }
        }
    }
}
```

**View Binding:**
```kotlin
// Enable in build.gradle.kts
android {
    buildFeatures {
        viewBinding = true
    }
}

// Usage in Activity
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        
        binding.button.setOnClickListener {
            // Handle click
        }
    }
}

// Usage in Fragment
class UserFragment : Fragment() {
    private var _binding: FragmentUserBinding? = null
    private val binding get() = _binding!!
    
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        _binding = FragmentUserBinding.inflate(inflater, container, false)
        return binding.root
    }
    
    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

**RecyclerView:**
```kotlin
class UserAdapter(
    private val onUserClick: (User) -> Unit
) : ListAdapter<User, UserAdapter.UserViewHolder>(UserDiffCallback()) {
    
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder {
        val binding = ItemUserBinding.inflate(
            LayoutInflater.from(parent.context),
            parent,
            false
        )
        return UserViewHolder(binding, onUserClick)
    }
    
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
    
    class UserViewHolder(
        private val binding: ItemUserBinding,
        private val onUserClick: (User) -> Unit
    ) : RecyclerView.ViewHolder(binding.root) {
        
        fun bind(user: User) {
            binding.nameText.text = user.name
            binding.emailText.text = user.email
            binding.root.setOnClickListener { onUserClick(user) }
        }
    }
}

class UserDiffCallback : DiffUtil.ItemCallback<User>() {
    override fun areItemsTheSame(oldItem: User, newItem: User): Boolean {
        return oldItem.id == newItem.id
    }
    
    override fun areContentsTheSame(oldItem: User, newItem: User): Boolean {
        return oldItem == newItem
    }
}
```

### Material Design

**Material Design 3 (Material You):**
```kotlin
// Compose
@Composable
fun MyApp() {
    MaterialTheme(
        colorScheme = dynamicColorScheme(),
        typography = Typography,
        shapes = Shapes
    ) {
        // App content
    }
}

@Composable
fun dynamicColorScheme(): ColorScheme {
    return if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
        val context = LocalContext.current
        if (isSystemInDarkTheme()) {
            dynamicDarkColorScheme(context)
        } else {
            dynamicLightColorScheme(context)
        }
    } else {
        if (isSystemInDarkTheme()) {
            darkColorScheme()
        } else {
            lightColorScheme()
        }
    }
}
```

**Material Components:**
- Button, OutlinedButton, TextButton
- Card, ElevatedCard, OutlinedCard
- TextField, OutlinedTextField
- TopAppBar, BottomAppBar
- NavigationBar, NavigationRail
- Dialog, AlertDialog
- Snackbar
- Chip, FilterChip, AssistChip

### Responsive Design

**Adaptive Layouts:**
```kotlin
@Composable
fun AdaptiveLayout() {
    val windowSizeClass = calculateWindowSizeClass(activity = LocalContext.current as Activity)
    
    when (windowSizeClass.widthSizeClass) {
        WindowWidthSizeClass.Compact -> {
            // Phone in portrait
            CompactLayout()
        }
        WindowWidthSizeClass.Medium -> {
            // Phone in landscape or small tablet
            MediumLayout()
        }
        WindowWidthSizeClass.Expanded -> {
            // Large tablet or desktop
            ExpandedLayout()
        }
    }
}
```

**Screen Size Qualifiers:**
```plaintext
res/
├── layout/              # Default (phones)
├── layout-sw600dp/      # 7" tablets
├── layout-sw720dp/      # 10" tablets
├── layout-land/         # Landscape
└── layout-xlarge/       # Extra large screens
```

**Density-Independent Pixels:**
- Use dp for dimensions
- Use sp for text sizes
- Provide multiple drawable densities (mdpi, hdpi, xhdpi, xxhdpi, xxxhdpi)
- Use vector drawables when possible



## Data Persistence

### Room Database

**Android's Recommended SQLite Abstraction:**
- Compile-time verification of SQL queries
- Convenient annotations for database operations
- Observable queries with Flow or LiveData
- Migration support
- Type converters for custom types
- Full-text search support

**When to Use:**
- Complex data models with relationships
- Need for advanced querying
- Large datasets
- Offline-first applications
- Structured relational data

**Setup:**
```kotlin
// Entity
@Entity(tableName = "users")
data class UserEntity(
    @PrimaryKey val id: String,
    @ColumnInfo(name = "name") val name: String,
    @ColumnInfo(name = "email") val email: String,
    @ColumnInfo(name = "created_at") val createdAt: Long
)

// DAO
@Dao
interface UserDao {
    @Query("SELECT * FROM users")
    fun getAllUsers(): Flow<List<UserEntity>>
    
    @Query("SELECT * FROM users WHERE id = :userId")
    suspend fun getUserById(userId: String): UserEntity?
    
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(user: UserEntity)
    
    @Update
    suspend fun update(user: UserEntity)
    
    @Delete
    suspend fun delete(user: UserEntity)
    
    @Query("DELETE FROM users")
    suspend fun deleteAll()
}

// Database
@Database(
    entities = [UserEntity::class],
    version = 1,
    exportSchema = true
)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
    
    companion object {
        @Volatile
        private var INSTANCE: AppDatabase? = null
        
        fun getDatabase(context: Context): AppDatabase {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    AppDatabase::class.java,
                    "app_database"
                )
                .addMigrations(MIGRATION_1_2)
                .build()
                INSTANCE = instance
                instance
            }
        }
        
        val MIGRATION_1_2 = object : Migration(1, 2) {
            override fun migrate(database: SupportSQLiteDatabase) {
                database.execSQL("ALTER TABLE users ADD COLUMN age INTEGER NOT NULL DEFAULT 0")
            }
        }
    }
}
```

**Best Practices:**
- Use Flow for observable queries
- Perform database operations on background thread
- Use transactions for multiple operations
- Implement proper migrations
- Export schema for version control
- Use type converters for complex types

**Relationships:**
```kotlin
// One-to-Many
data class UserWithPosts(
    @Embedded val user: UserEntity,
    @Relation(
        parentColumn = "id",
        entityColumn = "userId"
    )
    val posts: List<PostEntity>
)

@Transaction
@Query("SELECT * FROM users WHERE id = :userId")
suspend fun getUserWithPosts(userId: String): UserWithPosts
```

### DataStore

**Modern Data Storage Solution:**
- Replacement for SharedPreferences
- Type-safe with Protocol Buffers
- Asynchronous API with Flow
- Handles data migration
- Transactional updates

**Preferences DataStore:**
```kotlin
// Create DataStore
val Context.dataStore: DataStore<Preferences> by preferencesDataStore(name = "settings")

// Keys
object PreferencesKeys {
    val THEME_MODE = stringPreferencesKey("theme_mode")
    val NOTIFICATIONS_ENABLED = booleanPreferencesKey("notifications_enabled")
}

// Repository
class SettingsRepository(private val dataStore: DataStore<Preferences>) {
    
    val themeMode: Flow<String> = dataStore.data
        .map { preferences ->
            preferences[PreferencesKeys.THEME_MODE] ?: "system"
        }
    
    suspend fun setThemeMode(mode: String) {
        dataStore.edit { preferences ->
            preferences[PreferencesKeys.THEME_MODE] = mode
        }
    }
    
    val notificationsEnabled: Flow<Boolean> = dataStore.data
        .map { preferences ->
            preferences[PreferencesKeys.NOTIFICATIONS_ENABLED] ?: true
        }
    
    suspend fun setNotificationsEnabled(enabled: Boolean) {
        dataStore.edit { preferences ->
            preferences[PreferencesKeys.NOTIFICATIONS_ENABLED] = enabled
        }
    }
}
```

**Proto DataStore (Type-Safe):**
```protobuf
// settings.proto
syntax = "proto3";

option java_package = "com.example.app";
option java_multiple_files = true;

message Settings {
  string theme_mode = 1;
  bool notifications_enabled = 2;
  int32 font_size = 3;
}
```

```kotlin
// Serializer
object SettingsSerializer : Serializer<Settings> {
    override val defaultValue: Settings = Settings.getDefaultInstance()
    
    override suspend fun readFrom(input: InputStream): Settings {
        return Settings.parseFrom(input)
    }
    
    override suspend fun writeTo(t: Settings, output: OutputStream) {
        t.writeTo(output)
    }
}

// DataStore
val Context.settingsDataStore: DataStore<Settings> by dataStore(
    fileName = "settings.pb",
    serializer = SettingsSerializer
)

// Usage
class SettingsRepository(private val dataStore: DataStore<Settings>) {
    val settings: Flow<Settings> = dataStore.data
    
    suspend fun updateThemeMode(mode: String) {
        dataStore.updateData { currentSettings ->
            currentSettings.toBuilder()
                .setThemeMode(mode)
                .build()
        }
    }
}
```

### SharedPreferences

**Simple Key-Value Storage:**
- Store small amounts of primitive data
- User preferences and settings
- App configuration
- Simple data types (String, Int, Boolean, Float, Long)

**Usage:**
```kotlin
class PreferencesManager(context: Context) {
    private val prefs = context.getSharedPreferences("app_prefs", Context.MODE_PRIVATE)
    
    var username: String?
        get() = prefs.getString("username", null)
        set(value) = prefs.edit().putString("username", value).apply()
    
    var isFirstLaunch: Boolean
        get() = prefs.getBoolean("is_first_launch", true)
        set(value) = prefs.edit().putBoolean("is_first_launch", value).apply()
    
    fun clear() {
        prefs.edit().clear().apply()
    }
}
```

**What NOT to Store:**
- Sensitive data (use EncryptedSharedPreferences or Keystore)
- Large amounts of data (use Room or files)
- Complex objects (use DataStore or Room)

### EncryptedSharedPreferences

**Secure Storage for Sensitive Data:**
```kotlin
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()

val encryptedPrefs = EncryptedSharedPreferences.create(
    context,
    "secure_prefs",
    masterKey,
    EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
    EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
)

// Usage
encryptedPrefs.edit()
    .putString("auth_token", token)
    .apply()

val token = encryptedPrefs.getString("auth_token", null)
```

### Android Keystore

**Secure Credential Storage:**
- Store cryptographic keys
- Hardware-backed security (when available)
- Keys never leave secure hardware
- Biometric authentication integration

**Usage:**
```kotlin
class KeystoreManager {
    private val keyStore = KeyStore.getInstance("AndroidKeyStore").apply {
        load(null)
    }
    
    fun generateKey(alias: String) {
        val keyGenerator = KeyGenerator.getInstance(
            KeyProperties.KEY_ALGORITHM_AES,
            "AndroidKeyStore"
        )
        
        val keyGenParameterSpec = KeyGenParameterSpec.Builder(
            alias,
            KeyProperties.PURPOSE_ENCRYPT or KeyProperties.PURPOSE_DECRYPT
        )
            .setBlockModes(KeyProperties.BLOCK_MODE_GCM)
            .setEncryptionPaddings(KeyProperties.ENCRYPTION_PADDING_NONE)
            .setUserAuthenticationRequired(true)
            .setUserAuthenticationParameters(30, KeyProperties.AUTH_BIOMETRIC_STRONG)
            .build()
        
        keyGenerator.init(keyGenParameterSpec)
        keyGenerator.generateKey()
    }
    
    fun getKey(alias: String): SecretKey? {
        return keyStore.getKey(alias, null) as? SecretKey
    }
}
```

### File Storage

**Internal Storage:**
- Private to your app
- Deleted when app is uninstalled
- Use for app-specific data

```kotlin
// Write file
val file = File(context.filesDir, "myfile.txt")
file.writeText("Hello, World!")

// Read file
val content = file.readText()

// Cache directory
val cacheFile = File(context.cacheDir, "temp.txt")
```

**External Storage:**
- Shared storage accessible by other apps
- Requires permissions (Android 10+: scoped storage)
- Use for user-visible files

```kotlin
// Scoped storage (Android 10+)
val contentValues = ContentValues().apply {
    put(MediaStore.Images.Media.DISPLAY_NAME, "image.jpg")
    put(MediaStore.Images.Media.MIME_TYPE, "image/jpeg")
    put(MediaStore.Images.Media.RELATIVE_PATH, Environment.DIRECTORY_PICTURES)
}

val uri = contentResolver.insert(
    MediaStore.Images.Media.EXTERNAL_CONTENT_URI,
    contentValues
)

uri?.let {
    contentResolver.openOutputStream(it)?.use { outputStream ->
        // Write to outputStream
    }
}
```

## Networking

### Retrofit

**Type-Safe HTTP Client:**
- Declarative API definitions
- Automatic JSON serialization/deserialization
- Support for coroutines and Flow
- Interceptor support
- Error handling

**Setup:**
```kotlin
// API Service
interface ApiService {
    @GET("users")
    suspend fun getUsers(): List<UserDto>
    
    @GET("users/{id}")
    suspend fun getUser(@Path("id") userId: String): UserDto
    
    @POST("users")
    suspend fun createUser(@Body user: CreateUserRequest): UserDto
    
    @PUT("users/{id}")
    suspend fun updateUser(
        @Path("id") userId: String,
        @Body user: UpdateUserRequest
    ): UserDto
    
    @DELETE("users/{id}")
    suspend fun deleteUser(@Path("id") userId: String): Response<Unit>
    
    @GET("users")
    suspend fun searchUsers(@Query("q") query: String): List<UserDto>
}

// Retrofit instance
object RetrofitClient {
    private const val BASE_URL = "https://api.example.com/"
    
    private val okHttpClient = OkHttpClient.Builder()
        .addInterceptor(AuthInterceptor())
        .addInterceptor(LoggingInterceptor())
        .connectTimeout(30, TimeUnit.SECONDS)
        .readTimeout(30, TimeUnit.SECONDS)
        .build()
    
    val apiService: ApiService by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .client(okHttpClient)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(ApiService::class.java)
    }
}
```

**Interceptors:**
```kotlin
// Authentication interceptor
class AuthInterceptor(private val tokenProvider: TokenProvider) : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val originalRequest = chain.request()
        val token = tokenProvider.getToken()
        
        val newRequest = if (token != null) {
            originalRequest.newBuilder()
                .header("Authorization", "Bearer $token")
                .build()
        } else {
            originalRequest
        }
        
        return chain.proceed(newRequest)
    }
}

// Logging interceptor
class LoggingInterceptor : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val request = chain.request()
        
        Log.d("HTTP", "Request: ${request.method} ${request.url}")
        
        val response = chain.proceed(request)
        
        Log.d("HTTP", "Response: ${response.code} ${request.url}")
        
        return response
    }
}
```

### OkHttp

**HTTP Client:**
- Connection pooling
- GZIP compression
- Response caching
- Interceptors for request/response modification

**Caching:**
```kotlin
val cacheSize = 10 * 1024 * 1024 // 10 MB
val cache = Cache(context.cacheDir, cacheSize.toLong())

val okHttpClient = OkHttpClient.Builder()
    .cache(cache)
    .addNetworkInterceptor(CacheInterceptor())
    .build()

class CacheInterceptor : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val response = chain.proceed(chain.request())
        
        val cacheControl = CacheControl.Builder()
            .maxAge(5, TimeUnit.MINUTES)
            .build()
        
        return response.newBuilder()
            .header("Cache-Control", cacheControl.toString())
            .build()
    }
}
```

### Kotlin Coroutines for Networking

**Structured Concurrency:**
```kotlin
class UserRepository(private val apiService: ApiService) {
    
    suspend fun getUsers(): Result<List<User>> = withContext(Dispatchers.IO) {
        try {
            val users = apiService.getUsers()
            Result.success(users.map { it.toDomain() })
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
    
    fun getUsersFlow(): Flow<List<User>> = flow {
        val users = apiService.getUsers()
        emit(users.map { it.toDomain() })
    }.flowOn(Dispatchers.IO)
    
    suspend fun getUserWithRetry(userId: String): User {
        return retry(times = 3, initialDelay = 1000) {
            apiService.getUser(userId).toDomain()
        }
    }
}

// Retry utility
suspend fun <T> retry(
    times: Int,
    initialDelay: Long = 100,
    maxDelay: Long = 1000,
    factor: Double = 2.0,
    block: suspend () -> T
): T {
    var currentDelay = initialDelay
    repeat(times - 1) {
        try {
            return block()
        } catch (e: Exception) {
            // Log error
        }
        delay(currentDelay)
        currentDelay = (currentDelay * factor).toLong().coerceAtMost(maxDelay)
    }
    return block() // Last attempt
}
```

### Error Handling

**Network Error Types:**
```kotlin
sealed class NetworkError : Exception() {
    data class HttpError(val code: Int, val message: String) : NetworkError()
    data class NetworkException(val throwable: Throwable) : NetworkError()
    object NoInternetConnection : NetworkError()
    object Timeout : NetworkError()
    object Unknown : NetworkError()
}

// Error handling
suspend fun <T> safeApiCall(apiCall: suspend () -> T): Result<T> {
    return try {
        Result.success(apiCall())
    } catch (e: HttpException) {
        Result.failure(NetworkError.HttpError(e.code(), e.message()))
    } catch (e: IOException) {
        Result.failure(NetworkError.NetworkException(e))
    } catch (e: Exception) {
        Result.failure(NetworkError.Unknown)
    }
}
```

### Network Monitoring

**ConnectivityManager:**
```kotlin
class NetworkMonitor(context: Context) {
    private val connectivityManager = context.getSystemService(Context.CONNECTIVITY_SERVICE) as ConnectivityManager
    
    private val _isConnected = MutableStateFlow(false)
    val isConnected: StateFlow<Boolean> = _isConnected.asStateFlow()
    
    private val networkCallback = object : ConnectivityManager.NetworkCallback() {
        override fun onAvailable(network: Network) {
            _isConnected.value = true
        }
        
        override fun onLost(network: Network) {
            _isConnected.value = false
        }
    }
    
    fun startMonitoring() {
        val request = NetworkRequest.Builder()
            .addCapability(NetworkCapabilities.NET_CAPABILITY_INTERNET)
            .build()
        
        connectivityManager.registerNetworkCallback(request, networkCallback)
    }
    
    fun stopMonitoring() {
        connectivityManager.unregisterNetworkCallback(networkCallback)
    }
}
```

## App Lifecycle

### Activity Lifecycle

**Lifecycle States:**
- Created: Activity is being created
- Started: Activity is visible but not interactive
- Resumed: Activity is visible and interactive
- Paused: Activity is partially visible
- Stopped: Activity is not visible
- Destroyed: Activity is being destroyed

**Lifecycle Methods:**
```kotlin
class MainActivity : AppCompatActivity() {
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Initialize activity, set content view
        // Restore saved state
    }
    
    override fun onStart() {
        super.onStart()
        // Activity is visible
        // Start animations, register listeners
    }
    
    override fun onResume() {
        super.onResume()
        // Activity is interactive
        // Resume paused operations
    }
    
    override fun onPause() {
        super.onPause()
        // Activity is losing focus
        // Pause operations, save draft data
    }
    
    override fun onStop() {
        super.onStop()
        // Activity is not visible
        // Release resources, stop animations
    }
    
    override fun onDestroy() {
        super.onDestroy()
        // Activity is being destroyed
        // Final cleanup
    }
    
    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        // Save UI state
        outState.putString("key", value)
    }
    
    override fun onRestoreInstanceState(savedInstanceState: Bundle) {
        super.onRestoreInstanceState(savedInstanceState)
        // Restore UI state
        val value = savedInstanceState.getString("key")
    }
}
```

### Fragment Lifecycle

**Fragment Lifecycle Methods:**
```kotlin
class UserFragment : Fragment() {
    
    override fun onAttach(context: Context) {
        super.onAttach(context)
        // Fragment attached to activity
    }
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Initialize fragment
    }
    
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // Create and return view hierarchy
        return binding.root
    }
    
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // View is created, setup UI
    }
    
    override fun onStart() {
        super.onStart()
        // Fragment is visible
    }
    
    override fun onResume() {
        super.onResume()
        // Fragment is interactive
    }
    
    override fun onPause() {
        super.onPause()
        // Fragment is losing focus
    }
    
    override fun onStop() {
        super.onStop()
        // Fragment is not visible
    }
    
    override fun onDestroyView() {
        super.onDestroyView()
        // View is being destroyed
        _binding = null
    }
    
    override fun onDestroy() {
        super.onDestroy()
        // Fragment is being destroyed
    }
    
    override fun onDetach() {
        super.onDetach()
        // Fragment detached from activity
    }
}
```

### Services

**Background Services:**
```kotlin
class MyService : Service() {
    
    override fun onBind(intent: Intent?): IBinder? {
        return null
    }
    
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        // Perform background work
        
        // Return START_STICKY to restart service if killed
        return START_STICKY
    }
    
    override fun onDestroy() {
        super.onDestroy()
        // Cleanup
    }
}

// Foreground Service
class ForegroundService : Service() {
    
    override fun onCreate() {
        super.onCreate()
        startForeground(NOTIFICATION_ID, createNotification())
    }
    
    private fun createNotification(): Notification {
        return NotificationCompat.Builder(this, CHANNEL_ID)
            .setContentTitle("Service Running")
            .setContentText("Performing background task")
            .setSmallIcon(R.drawable.ic_notification)
            .build()
    }
}
```

### WorkManager

**Deferrable Background Work:**
```kotlin
// Define Worker
class SyncWorker(
    context: Context,
    params: WorkerParameters
) : CoroutineWorker(context, params) {
    
    override suspend fun doWork(): Result {
        return try {
            // Perform sync operation
            syncData()
            Result.success()
        } catch (e: Exception) {
            if (runAttemptCount < 3) {
                Result.retry()
            } else {
                Result.failure()
            }
        }
    }
    
    private suspend fun syncData() {
        // Sync implementation
    }
}

// Schedule work
class WorkScheduler(private val context: Context) {
    
    fun scheduleSync() {
        val constraints = Constraints.Builder()
            .setRequiredNetworkType(NetworkType.CONNECTED)
            .setRequiresBatteryNotLow(true)
            .build()
        
        val syncRequest = PeriodicWorkRequestBuilder<SyncWorker>(
            repeatInterval = 15,
            repeatIntervalTimeUnit = TimeUnit.MINUTES
        )
            .setConstraints(constraints)
            .setBackoffCriteria(
                BackoffPolicy.EXPONENTIAL,
                WorkRequest.MIN_BACKOFF_MILLIS,
                TimeUnit.MILLISECONDS
            )
            .build()
        
        WorkManager.getInstance(context).enqueueUniquePeriodicWork(
            "sync_work",
            ExistingPeriodicWorkPolicy.KEEP,
            syncRequest
        )
    }
    
    fun cancelSync() {
        WorkManager.getInstance(context).cancelUniqueWork("sync_work")
    }
}
```

### Broadcast Receivers

**System Events:**
```kotlin
class MyBroadcastReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        when (intent.action) {
            Intent.ACTION_BOOT_COMPLETED -> {
                // Device booted
            }
            Intent.ACTION_BATTERY_LOW -> {
                // Battery low
            }
        }
    }
}

// Register in manifest
<receiver android:name=".MyBroadcastReceiver" android:exported="false">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
    </intent-filter>
</receiver>

// Or register dynamically
val receiver = MyBroadcastReceiver()
val filter = IntentFilter(Intent.ACTION_BATTERY_LOW)
registerReceiver(receiver, filter)
```



## Performance Optimization

### Memory Management

**Understanding Memory:**
- Heap memory for objects
- Stack memory for method calls
- Native memory for JNI and bitmaps
- Graphics memory for UI rendering

**Avoiding Memory Leaks:**
```kotlin
// Memory leak - BAD
class MainActivity : AppCompatActivity() {
    companion object {
        var instance: MainActivity? = null // Static reference to Activity
    }
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        instance = this // Leak!
    }
}

// Fixed - GOOD
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Use Application context for long-lived objects
        // Use lifecycle-aware components
    }
}

// Proper cleanup
class MyFragment : Fragment() {
    private var callback: (() -> Unit)? = null
    
    override fun onDestroyView() {
        super.onDestroyView()
        callback = null // Clear references
        _binding = null
    }
}
```

**Memory Profiling:**
- Use Android Studio Memory Profiler
- Monitor heap allocations
- Detect memory leaks with LeakCanary
- Analyze memory dumps
- Track object allocations

**Best Practices:**
- Use Application context for singletons
- Clear references in onDestroy/onDestroyView
- Use weak references for callbacks when appropriate
- Avoid static references to Activities/Fragments/Views
- Use lifecycle-aware components
- Release resources when not needed
- Use object pools for frequently created objects

### Startup Time Optimization

**App Startup Phases:**
1. Cold start: App not in memory
2. Warm start: App in background
3. Hot start: App in foreground

**Optimization Strategies:**
- Lazy initialize components
- Defer non-critical initialization
- Use App Startup library for initialization
- Avoid blocking main thread
- Optimize Application.onCreate()
- Use baseline profiles
- Enable R8 optimization

**App Startup Library:**
```kotlin
class NetworkInitializer : Initializer<NetworkManager> {
    override fun create(context: Context): NetworkManager {
        return NetworkManager.getInstance(context)
    }
    
    override fun dependencies(): List<Class<out Initializer<*>>> {
        return emptyList()
    }
}

// In AndroidManifest.xml
<provider
    android:name="androidx.startup.InitializationProvider"
    android:authorities="${applicationId}.androidx-startup"
    android:exported="false"
    tools:node="merge">
    <meta-data
        android:name="com.example.NetworkInitializer"
        android:value="androidx.startup" />
</provider>
```

**Baseline Profiles:**
```kotlin
// In build.gradle.kts
plugins {
    id("androidx.baselineprofile")
}

dependencies {
    baselineProfile(project(":baselineprofile"))
}
```

**Target Times:**
- Cold start: < 5 seconds (ideal < 2 seconds)
- Warm start: < 2 seconds
- Hot start: < 1.5 seconds

### UI Performance

**Smooth Scrolling:**
- Use RecyclerView with DiffUtil
- Implement efficient ViewHolder pattern
- Avoid complex layouts in list items
- Use ConstraintLayout for flat hierarchies
- Load images asynchronously
- Implement prefetching

**RecyclerView Optimization:**
```kotlin
class UserAdapter : ListAdapter<User, UserAdapter.ViewHolder>(UserDiffCallback()) {
    
    // Set fixed size if RecyclerView size doesn't change
    init {
        setHasStableIds(true)
    }
    
    override fun getItemId(position: Int): Long {
        return getItem(position).id.hashCode().toLong()
    }
    
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        // Reuse layout inflater
        val binding = ItemUserBinding.inflate(
            LayoutInflater.from(parent.context),
            parent,
            false
        )
        return ViewHolder(binding)
    }
    
    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
    
    // ViewHolder with minimal work in bind
    class ViewHolder(private val binding: ItemUserBinding) : RecyclerView.ViewHolder(binding.root) {
        fun bind(user: User) {
            binding.nameText.text = user.name
            // Load image asynchronously
            Coil.load(binding.avatarImage) {
                data(user.avatarUrl)
                crossfade(true)
                placeholder(R.drawable.placeholder)
            }
        }
    }
}
```

**Layout Optimization:**
- Use ConstraintLayout to flatten view hierarchy
- Avoid nested LinearLayouts
- Use merge tags to reduce hierarchy
- Use ViewStub for rarely shown views
- Avoid overdraw

**Compose Performance:**
```kotlin
// Stable parameters for skippable composables
@Immutable
data class User(val id: String, val name: String)

@Composable
fun UserList(users: List<User>) {
    LazyColumn {
        items(
            items = users,
            key = { it.id } // Stable keys for recomposition
        ) { user ->
            UserItem(user = user)
        }
    }
}

// Avoid unnecessary recomposition
@Composable
fun UserItem(user: User) {
    // Use remember for expensive calculations
    val formattedName = remember(user.name) {
        user.name.uppercase()
    }
    
    Text(text = formattedName)
}

// Use derivedStateOf for computed state
@Composable
fun FilteredList(users: List<User>, query: String) {
    val filteredUsers by remember {
        derivedStateOf {
            users.filter { it.name.contains(query, ignoreCase = true) }
        }
    }
    
    LazyColumn {
        items(filteredUsers) { user ->
            UserItem(user = user)
        }
    }
}
```

### Image Optimization

**Image Loading:**
- Use Coil, Glide, or Picasso
- Implement caching (memory and disk)
- Downscale images to display size
- Use appropriate image formats (WebP, AVIF)
- Implement progressive loading
- Use placeholders

**Coil Example:**
```kotlin
imageView.load(imageUrl) {
    crossfade(true)
    placeholder(R.drawable.placeholder)
    error(R.drawable.error)
    transformations(CircleCropTransformation())
    size(width, height) // Resize to view size
    memoryCachePolicy(CachePolicy.ENABLED)
    diskCachePolicy(CachePolicy.ENABLED)
}
```

**Bitmap Optimization:**
```kotlin
fun decodeSampledBitmap(
    resource: Int,
    reqWidth: Int,
    reqHeight: Int
): Bitmap {
    return BitmapFactory.Options().run {
        inJustDecodeBounds = true
        BitmapFactory.decodeResource(resources, resource, this)
        
        inSampleSize = calculateInSampleSize(this, reqWidth, reqHeight)
        
        inJustDecodeBounds = false
        BitmapFactory.decodeResource(resources, resource, this)
    }
}

fun calculateInSampleSize(
    options: BitmapFactory.Options,
    reqWidth: Int,
    reqHeight: Int
): Int {
    val (height: Int, width: Int) = options.run { outHeight to outWidth }
    var inSampleSize = 1
    
    if (height > reqHeight || width > reqWidth) {
        val halfHeight: Int = height / 2
        val halfWidth: Int = width / 2
        
        while (halfHeight / inSampleSize >= reqHeight && halfWidth / inSampleSize >= reqWidth) {
            inSampleSize *= 2
        }
    }
    
    return inSampleSize
}
```

### Battery Optimization

**Power-Hungry Operations:**
- Network requests
- Location services
- Background processing
- Wake locks
- Sensors

**Optimization Strategies:**
- Batch network requests
- Use WorkManager for background tasks
- Implement Doze mode compatibility
- Use JobScheduler for deferred work
- Reduce location accuracy when possible
- Release wake locks promptly

**Battery-Efficient Location:**
```kotlin
val locationRequest = LocationRequest.Builder(
    Priority.PRIORITY_BALANCED_POWER_ACCURACY,
    10000 // 10 seconds
).apply {
    setMinUpdateIntervalMillis(5000)
    setMaxUpdateDelayMillis(20000)
}.build()

// Use fused location provider
fusedLocationClient.requestLocationUpdates(
    locationRequest,
    locationCallback,
    Looper.getMainLooper()
)
```

**Doze Mode Compatibility:**
```kotlin
// Check if app is whitelisted
val powerManager = getSystemService(Context.POWER_SERVICE) as PowerManager
val isIgnoringBatteryOptimizations = powerManager.isIgnoringBatteryOptimizations(packageName)

// Request whitelist (only for specific use cases)
if (!isIgnoringBatteryOptimizations) {
    val intent = Intent(Settings.ACTION_REQUEST_IGNORE_BATTERY_OPTIMIZATIONS).apply {
        data = Uri.parse("package:$packageName")
    }
    startActivity(intent)
}
```

### Network Performance

**Optimization Techniques:**
- Implement caching
- Use HTTP/2 or HTTP/3
- Compress request/response data
- Batch API requests
- Implement pagination
- Use CDN for static assets
- Prefetch data when appropriate

**Caching Strategy:**
```kotlin
val cacheSize = 10 * 1024 * 1024 // 10 MB
val cache = Cache(context.cacheDir, cacheSize.toLong())

val okHttpClient = OkHttpClient.Builder()
    .cache(cache)
    .addNetworkInterceptor { chain ->
        val response = chain.proceed(chain.request())
        response.newBuilder()
            .header("Cache-Control", "public, max-age=300") // 5 minutes
            .build()
    }
    .build()
```

### Build Optimization

**R8 Optimization:**
```kotlin
// build.gradle.kts
android {
    buildTypes {
        release {
            isMinifyEnabled = true
            isShrinkResources = true
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
}
```

**Build Performance:**
- Enable Gradle build cache
- Use configuration cache
- Increase Gradle heap size
- Use parallel execution
- Avoid unnecessary dependencies

```kotlin
// gradle.properties
org.gradle.jvmargs=-Xmx4096m -XX:MaxMetaspaceSize=512m
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.configuration-cache=true
android.enableJetifier=false
android.useAndroidX=true
```

## Play Store Guidelines

### Review Guidelines Overview

**Key Areas:**
- Content Policy: Appropriate content, no harmful behavior
- Privacy and Security: Data handling, permissions
- Monetization: In-app purchases, ads, subscriptions
- Device and Network Abuse: Battery, data usage
- User Experience: Functionality, stability, performance

### Common Rejection Reasons

**Incomplete Information:**
- Missing privacy policy
- Incomplete app description
- Missing content rating
- Insufficient app functionality
- Placeholder content or broken features

**Privacy Violations:**
- Accessing data without permission
- Missing data safety section
- Not explaining data usage
- Sharing data without consent
- Accessing sensitive permissions unnecessarily

**Crashes and Bugs:**
- App crashes on launch
- Broken core functionality
- ANRs (Application Not Responding)
- Poor performance
- Memory leaks

**Policy Violations:**
- Inappropriate content
- Misleading functionality
- Spam or deceptive behavior
- Intellectual property violations
- Malware or harmful behavior

### Privacy Requirements

**Data Safety Section:**
- Declare all data collected
- Explain how data is used
- Specify if data is shared with third parties
- Indicate if data is encrypted
- Provide data deletion option

**Permissions:**
```xml
<!-- AndroidManifest.xml -->
<!-- Declare permissions with clear purpose -->
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"
    android:maxSdkVersion="32" />

<!-- Runtime permission request -->
```

```kotlin
// Request permissions
val permissionLauncher = registerForActivityResult(
    ActivityResultContracts.RequestPermission()
) { isGranted ->
    if (isGranted) {
        // Permission granted
    } else {
        // Permission denied
    }
}

// Check and request
when {
    ContextCompat.checkSelfPermission(
        this,
        Manifest.permission.CAMERA
    ) == PackageManager.PERMISSION_GRANTED -> {
        // Permission already granted
    }
    shouldShowRequestPermissionRationale(Manifest.permission.CAMERA) -> {
        // Show rationale
        showPermissionRationale()
    }
    else -> {
        // Request permission
        permissionLauncher.launch(Manifest.permission.CAMERA)
    }
}
```

**Privacy Policy:**
- Required for apps that access sensitive data
- Must be accessible from Play Store listing
- Should explain data collection and usage
- Include contact information
- Update when practices change

### In-App Purchases

**Google Play Billing:**
```kotlin
// Setup billing client
val billingClient = BillingClient.newBuilder(context)
    .setListener { billingResult, purchases ->
        if (billingResult.responseCode == BillingClient.BillingResponseCode.OK && purchases != null) {
            for (purchase in purchases) {
                handlePurchase(purchase)
            }
        }
    }
    .enablePendingPurchases()
    .build()

// Connect to billing service
billingClient.startConnection(object : BillingClientStateListener {
    override fun onBillingSetupFinished(billingResult: BillingResult) {
        if (billingResult.responseCode == BillingClient.BillingResponseCode.OK) {
            // Ready to query purchases
            queryProducts()
        }
    }
    
    override fun onBillingServiceDisconnected() {
        // Retry connection
    }
})

// Query products
fun queryProducts() {
    val productList = listOf(
        QueryProductDetailsParams.Product.newBuilder()
            .setProductId("premium_upgrade")
            .setProductType(BillingClient.ProductType.INAPP)
            .build()
    )
    
    val params = QueryProductDetailsParams.newBuilder()
        .setProductList(productList)
        .build()
    
    billingClient.queryProductDetailsAsync(params) { billingResult, productDetailsList ->
        // Handle product details
    }
}

// Launch purchase flow
fun launchPurchase(productDetails: ProductDetails) {
    val productDetailsParamsList = listOf(
        BillingFlowParams.ProductDetailsParams.newBuilder()
            .setProductDetails(productDetails)
            .build()
    )
    
    val billingFlowParams = BillingFlowParams.newBuilder()
        .setProductDetailsParamsList(productDetailsParamsList)
        .build()
    
    billingClient.launchBillingFlow(activity, billingFlowParams)
}

// Handle purchase
fun handlePurchase(purchase: Purchase) {
    if (purchase.purchaseState == Purchase.PurchaseState.PURCHASED) {
        if (!purchase.isAcknowledged) {
            val acknowledgePurchaseParams = AcknowledgePurchaseParams.newBuilder()
                .setPurchaseToken(purchase.purchaseToken)
                .build()
            
            billingClient.acknowledgePurchase(acknowledgePurchaseParams) { billingResult ->
                // Purchase acknowledged
            }
        }
    }
}
```

**Subscription Guidelines:**
- Clearly explain subscription terms
- Provide easy cancellation
- Show pricing and renewal info
- Handle subscription status changes
- Implement grace periods

### App Metadata

**App Name:**
- Maximum 30 characters
- Unique and descriptive
- No generic terms
- No promotional text

**Description:**
- Short description: 80 characters
- Full description: 4000 characters
- Clear and accurate
- Highlight key features
- Proper grammar and spelling
- Update for new features

**Screenshots:**
- Minimum 2 screenshots
- Phone: 16:9 or 9:16 aspect ratio
- Tablet: 16:9 or 9:16 aspect ratio (optional)
- Show actual app functionality
- High quality images
- No placeholder content

**Feature Graphic:**
- 1024 x 500 pixels
- Showcases app branding
- No text or promotional content
- High quality image

**App Icon:**
- 512 x 512 pixels
- Follows Material Design guidelines
- Distinctive and recognizable
- No transparency (for adaptive icons, use background layer)

### Content Rating

**IARC Rating:**
- Complete questionnaire honestly
- Consider all content types
- Include user-generated content
- Update if content changes
- Different ratings per region

**Age Restrictions:**
- Set appropriate age rating
- Implement age gates if needed
- Restrict mature content
- Follow COPPA for children's apps

### Submission Checklist

**Before Submission:**
- [ ] Test on multiple devices and Android versions
- [ ] Fix all crashes and ANRs
- [ ] Complete all app metadata
- [ ] Add privacy policy URL
- [ ] Complete data safety section
- [ ] Add all required screenshots and graphics
- [ ] Test in-app purchases in sandbox
- [ ] Verify all permissions have clear purpose
- [ ] Check for placeholder content
- [ ] Review Play Store policies
- [ ] Test accessibility features
- [ ] Verify deep links work
- [ ] Test push notifications
- [ ] Check app size and performance
- [ ] Complete content rating questionnaire

**Release Notes:**
- Describe new features
- Mention bug fixes
- Keep concise and clear
- Update for each release
- Localize if possible

### Post-Release

**Monitoring:**
- Watch crash reports in Play Console
- Monitor ANR rate
- Track Android Vitals metrics
- Read user reviews
- Check app ratings
- Monitor install/uninstall rates

**Updates:**
- Fix critical bugs quickly
- Add requested features
- Improve based on feedback
- Keep app current with Android versions
- Regular maintenance releases

**User Engagement:**
- Respond to reviews
- Address user concerns
- Promote new features
- Track app store optimization
- Monitor competitor apps

**Android Vitals:**
- Crash rate: < 1.09%
- ANR rate: < 0.47%
- Excessive wakeups: < 10 per hour
- Stuck wake locks: < 1%

## Conclusion

This template provides a comprehensive foundation for Android mobile application projects. Adapt these recommendations to your specific project needs, team size, and requirements. The key is to establish consistent patterns early and evolve them as your project grows.

Remember:
- Follow Material Design guidelines and Android best practices
- Prioritize user experience and performance from the start
- Write tests for critical functionality and business logic
- Implement proper memory management to avoid leaks
- Keep up with Android updates and Jetpack libraries
- Monitor crash reports and user feedback
- Plan for Play Store submission from day one
- Profile and optimize before assuming performance issues
- Support accessibility features for all users
- Iterate based on user feedback and analytics

**Key Takeaways:**
- Choose Kotlin and Jetpack Compose for new projects
- Implement MVVM or Clean Architecture for maintainability
- Use Room or DataStore for data persistence
- Leverage Kotlin Coroutines for asynchronous programming
- Test thoroughly with JUnit, Espresso, and Compose tests
- Optimize for memory, battery, and startup time
- Follow Play Store guidelines to avoid rejection
- Monitor performance with Android Profiler and Firebase

For questions or suggestions about this template, consult with your team and adapt it to your organization's specific needs and constraints. Stay current with Android's latest technologies and best practices through Android Developer documentation, Google I/O sessions, and the developer community.

