# Lesson 6: Working with Activities and Intents

In this lesson, we will explore the fundamental concepts of Activities and Intents in Android development. Understanding the Activity lifecycle, task management, and how to navigate between Activities and Fragments is crucial for building responsive applications. We will also cover how to pass data between components using Intents and Bundles, and how to implement the Parcelable interface for custom data types.

## 1. Understanding the Activity Lifecycle

An Activity is a single screen in an Android app that allows users to interact with the app. Each Activity has a lifecycle managed by the Android system, which determines how the Activity behaves in response to user interactions and system events.

### Activity Lifecycle States
The Activity lifecycle consists of several states, each represented by specific callback methods:

- **onCreate()**: Called when the Activity is first created. This is where you initialize your Activity, set the content view, and bind UI components.
  
  ```kotlin
  override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      setContentView(R.layout.activity_main)
  }
  ```

- **onStart()**: Called when the Activity becomes visible to the user. This is a good place to start animations or refresh UI elements.
  
  ```kotlin
  override fun onStart() {
      super.onStart()
      // Code to run when the Activity is visible
  }
  ```

- **onResume()**: Called when the Activity starts interacting with the user. This is where you should start any processes that require user interaction.
  
  ```kotlin
  override fun onResume() {
      super.onResume()
      // Code to run when the Activity is in the foreground
  }
  ```

- **onPause()**: Called when the Activity is partially obscured or not in focus. Use this method to pause ongoing tasks or save UI state.
  
  ```kotlin
  override fun onPause() {
      super.onPause()
      // Code to run when the Activity is not in the foreground
  }
  ```

- **onStop()**: Called when the Activity is no longer visible. This is a good place to stop animations or release resources.
  
  ```kotlin
  override fun onStop() {
      super.onStop()
      // Code to run when the Activity is no longer visible
  }
  ```

- **onDestroy()**: Called before the Activity is destroyed. Use this method to clean up resources.
  
  ```kotlin
  override fun onDestroy() {
      super.onDestroy()
      // Code to run when the Activity is being destroyed
  }
  ```

### Task Management and Back Stack Behavior
- **Task**: A task is a collection of Activities that users interact with when performing a certain job. The Android system manages tasks and their back stack.
- **Back Stack**: When a new Activity is started, it is pushed onto the back stack. Pressing the back button pops the current Activity off the stack and returns to the previous one.

### Managing the Back Stack
You can control how Activities are launched and how they behave in the back stack using flags in Intents. Common flags include:
- `FLAG_ACTIVITY_NEW_TASK`: Starts a new task for the Activity.
- `FLAG_ACTIVITY_CLEAR_TOP`: If the Activity is already running in the task, it will be brought to the front and all other Activities above it will be closed.

## 2. Navigating Between Activities and Fragments

### Starting a New Activity
To start a new Activity, you use an Intent. An Intent is a messaging object that you can use to request an action from another app component.

```kotlin
val intent = Intent(this, SecondActivity::class.java)
startActivity(intent)
```

### Passing Data Using Intents
You can pass data between Activities using Intents. Use the `putExtra()` method to attach data to the Intent.

```kotlin
val intent = Intent(this, SecondActivity::class.java)
intent.putExtra("EXTRA_MESSAGE", "Hello, Second Activity!")
startActivity(intent)
```

In the receiving Activity, retrieve the data using `getStringExtra()`:

```kotlin
val message = intent.getStringExtra("EXTRA_MESSAGE")
```

### Using Bundles
Bundles are used to pass data between Activities or Fragments. They are key-value pairs that can hold various data types.

```kotlin
val bundle = Bundle()
bundle.putString("KEY", "Value")
val intent = Intent(this, SecondActivity::class.java)
intent.putExtras(bundle)
startActivity(intent)
```

In the receiving Activity, you can retrieve the data from the Bundle:

```kotlin
val bundle = intent.extras
val value = bundle?.getString("KEY")
```

## 3. Using Parcelable for Custom Data Types

When passing complex data types (like custom objects) between Activities, you should implement the Parcelable interface. Parcelable is a more efficient way to serialize objects compared to Serializable.

### Implementing Parcelable
To make a custom class Parcelable, implement the Parcelable interface and override the required methods.

```kotlin
import android.os.Parcel
import android.os.Parcelable

data class User(val name: String, val age: Int) : Parcelable {
    constructor(parcel: Parcel) : this(
        parcel.readString() ?: "",
        parcel.readInt()
    )

    override fun writeToParcel(parcel: Parcel, flags: Int) {
        parcel.writeString(name)
        parcel.writeInt(age)
    }

    override fun describeContents(): Int {
        return 0
    }

    companion object CREATOR : Parcelable.Creator<User> {
        override fun createFromParcel(parcel: Parcel): User {
            return User(parcel)
        }

        override fun newArray(size: Int): Array<User?> {
            return arrayOfNulls(size)
        }
    }
}
```

### Passing Parcelable Objects
You can pass Parcelable objects using Intents just like you would with primitive types.

```kotlin
val user = User("Alice", 30)
val intent = Intent(this, SecondActivity::class.java)
intent.putExtra("USER_DATA", user)
startActivity(intent)
```

In the receiving Activity, retrieve the Parcelable object:

```kotlin
val user = intent.getParcelableExtra<User>("USER_DATA")
```

## Conclusion

In this lesson, we have explored the Activity lifecycle, task management, and back stack behavior in Android. We learned how to navigate between Activities and Fragments, pass data using Intents and Bundles, and implement the Parcelable interface for custom data types. Understanding these concepts is essential for managing user interactions and data flow in your Android applications. In the next lesson, we will dive into handling user input and validation in your app.