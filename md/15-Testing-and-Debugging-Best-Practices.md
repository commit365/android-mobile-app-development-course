# Lesson 15: Testing and Debugging Best Practices

In this lesson, we will explore best practices for testing and debugging Android applications. We will cover writing unit tests using JUnit, UI tests with Espresso, and integration tests with MockK or Mockito. Additionally, we will discuss debugging techniques, logging, and using Android Profiler for performance analysis and memory management.

## 1. Writing Unit Tests with JUnit

Unit tests are used to verify the functionality of individual components in isolation. JUnit is a popular testing framework for writing unit tests in Java and Kotlin.

### Setting Up JUnit

To use JUnit in your Android project, add the following dependency to your `build.gradle` file:

```groovy
dependencies {
    testImplementation 'junit:junit:4.13.2'
}
```

### Writing a Simple Unit Test

1. **Create a Test Class**: Create a new Kotlin class in the `src/test/java` directory.

```kotlin
import org.junit.Assert.assertEquals
import org.junit.Test

class CalculatorTest {

    @Test
    fun addition_isCorrect() {
        val result = 2 + 2
        assertEquals(4, result)
    }
}
```

2. **Run the Test**: You can run the test from Android Studio by right-clicking on the test class and selecting "Run."

### Best Practices for Unit Testing

- **Keep Tests Isolated**: Ensure that tests do not depend on each other.
- **Use Descriptive Names**: Name your test methods clearly to indicate what they are testing.
- **Test Edge Cases**: Consider testing edge cases and invalid inputs.

## 2. Writing UI Tests with Espresso

Espresso is a testing framework for writing UI tests in Android. It allows you to simulate user interactions and verify UI behavior.

### Setting Up Espresso

Add the following dependencies to your `build.gradle` file:

```groovy
dependencies {
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.0'
    androidTestImplementation 'androidx.test.ext:junit:1.1.5'
}
```

### Writing a Simple UI Test

1. **Create a UI Test Class**: Create a new Kotlin class in the `src/androidTest/java` directory.

```kotlin
import androidx.test.ext.junit.runners.AndroidJUnit4
import androidx.test.rule.ActivityTestRule
import org.junit.Rule
import org.junit.Test
import org.junit.runner.RunWith
import androidx.test.espresso.Espresso.onView
import androidx.test.espresso.action.ViewActions.click
import androidx.test.espresso.assertion.ViewAssertions.matches
import androidx.test.espresso.matcher.ViewMatchers.withId
import androidx.test.espresso.matcher.ViewMatchers.withText

@RunWith(AndroidJUnit4::class)
class MainActivityTest {

    @get:Rule
    var activityRule = ActivityTestRule(MainActivity::class.java)

    @Test
    fun buttonClick_changesText() {
        // Perform a click on the button
        onView(withId(R.id.button)).perform(click())

        // Check if the text is changed
        onView(withId(R.id.textView)).check(matches(withText("Button Clicked!")))
    }
}
```

2. **Run the UI Test**: You can run the UI test from Android Studio by right-clicking on the test class and selecting "Run."

### Best Practices for UI Testing

- **Use Idling Resources**: Use Idling Resources to handle asynchronous operations in your UI tests.
- **Keep Tests Independent**: Ensure that UI tests can run independently of each other.
- **Test User Flows**: Focus on testing critical user flows rather than every single UI element.

## 3. Writing Integration Tests with MockK or Mockito

Integration tests verify the interaction between different components of your application. MockK and Mockito are popular libraries for creating mock objects in tests.

### Setting Up MockK or Mockito

Add the following dependencies to your `build.gradle` file:

For **MockK**:
```groovy
dependencies {
    testImplementation 'io.mockk:mockk:1.13.3'
}
```

For **Mockito**:
```groovy
dependencies {
    testImplementation 'org.mockito:mockito-core:4.6.1'
}
```

### Writing an Integration Test with MockK

```kotlin
import io.mockk.every
import io.mockk.mockk
import org.junit.Assert.assertEquals
import org.junit.Test

class UserServiceTest {

    private val userRepository: UserRepository = mockk()
    private val userService = UserService(userRepository)

    @Test
    fun getUser_returnsUser() {
        val user = User(1, "John Doe")
        every { userRepository.getUser(1) } returns user

        val result = userService.getUser(1)
        assertEquals(user, result)
    }
}
```

### Writing an Integration Test with Mockito

```kotlin
import org.junit.Assert.assertEquals
import org.junit.Test
import org.mockito.Mockito.*

class UserServiceTest {

    private val userRepository: UserRepository = mock(UserRepository::class.java)
    private val userService = UserService(userRepository)

    @Test
    fun getUser_returnsUser() {
        val user = User(1, "John Doe")
        `when`(userRepository.getUser(1)).thenReturn(user)

        val result = userService.getUser(1)
        assertEquals(user, result)
    }
}
```

## 4. Debugging Techniques

Debugging is an essential part of the development process. Here are some common debugging techniques:

### Using Logcat

Use Logcat to log messages and track the flow of your application. You can log messages using:

```kotlin
Log.d("TAG", "Debug message")
Log.e("TAG", "Error message")
Log.i("TAG", "Info message")
```

### Setting Breakpoints

Use breakpoints in Android Studio to pause execution and inspect variables. You can set breakpoints by clicking on the left margin of the code editor.

### Inspecting Variables

While debugging, you can inspect variable values in the Variables pane. This helps you understand the state of your application at any point in time.

## 5. Using Android Profiler

Android Profiler is a powerful tool for analyzing the performance of your application. It provides insights into CPU, memory, network, and energy usage.

### Accessing Android Profiler

1. Open Android Studio and run your app.
2. Click on the "Profiler" tab at the bottom of the Android Studio window.

### Analyzing Performance

- **CPU Profiler**: Monitor CPU usage, track method calls, and identify performance bottlenecks.
- **Memory Profiler**: Analyze memory usage, detect memory leaks, and view object allocations.
- **Network Profiler**: Monitor network requests, view response times, and analyze data usage.

### Memory Management

Use the Memory Profiler to identify memory leaks and optimize memory usage. You can take heap dumps and analyze object references to find memory leaks.

## Conclusion

In this lesson, we covered best practices for testing and debugging Android applications. We learned how to write unit tests with JUnit, UI tests with Espresso, and integration tests with MockK or Mockito. We also discussed debugging techniques, logging, and using Android Profiler for performance analysis and memory management. Mastering these testing and debugging practices will help you ensure the quality and reliability of your Android applications. In the next lesson, we will explore advanced topics in Android development, including Kotlin Multiplatform and Jetpack Compose.