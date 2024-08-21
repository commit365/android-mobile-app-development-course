# Lesson 13: Implementing Navigation Components

In this lesson, we will explore how to implement the Navigation component in Android applications. The Navigation component simplifies the implementation of navigation between different parts of your app, including managing fragment transitions, deep linking, and bottom navigation. We will also cover creating a navigation graph and using Safe Args for type-safe argument passing.

## 1. Overview of the Navigation Component

The Navigation component is part of Android Jetpack and provides a framework for managing app navigation. It helps you implement navigation patterns such as:

- **Fragment Navigation**: Navigating between different fragments in your app.
- **Deep Linking**: Allowing users to navigate directly to specific content within your app.
- **Bottom Navigation**: Managing navigation between top-level destinations in your app.

### Adding Navigation Dependencies

To get started, add the following dependencies to your `build.gradle` file:

```groovy
dependencies {
    implementation "androidx.navigation:navigation-fragment-ktx:2.5.0"
    implementation "androidx.navigation:navigation-ui-ktx:2.5.0"
}
```

## 2. Creating a Navigation Graph

A navigation graph is an XML resource that defines all navigation paths within your app. It contains destinations (fragments, activities) and actions (navigation paths).

### Creating a Navigation Graph

1. **Add Navigation Graph Resource**:
   - Right-click on the `res` folder, select `New` > `Android Resource File`.
   - Name it `nav_graph.xml` and set the Resource Type to `Navigation`.

2. **Define Destinations**:
   - Open the `nav_graph.xml` file in the Navigation Editor.
   - Drag and drop fragments from the palette into the graph.
   - Define actions between fragments by connecting them.

### Example Navigation Graph

Here’s an example of a simple navigation graph with two fragments:

```xml
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    app:startDestination="@id/firstFragment">

    <fragment
        android:id="@+id/firstFragment"
        android:name="com.example.app.FirstFragment"
        android:label="First Fragment"
        tools:layout="@layout/fragment_first" />

    <fragment
        android:id="@+id/secondFragment"
        android:name="com.example.app.SecondFragment"
        android:label="Second Fragment"
        tools:layout="@layout/fragment_second" />

    <action
        android:id="@+id/action_firstFragment_to_secondFragment"
        app:destination="@id/secondFragment" />
</navigation>
```

## 3. Handling Transitions Between Fragments

To navigate between fragments, you can use the `NavController` class, which manages app navigation within a `NavHost`.

### Setting Up NavHost

1. **Add NavHostFragment to Your Activity Layout**:

```xml
<androidx.navigation.fragment.NavHostFragment
    android:id="@+id/nav_host_fragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:navGraph="@navigation/nav_graph" />
```

2. **Get NavController in Your Activity**:

```kotlin
val navController = findNavController(R.id.nav_host_fragment)
```

### Navigating to Another Fragment

You can navigate to another fragment using the `navigate()` method:

```kotlin
navController.navigate(R.id.action_firstFragment_to_secondFragment)
```

## 4. Deep Linking

Deep linking allows users to navigate directly to specific content in your app using a URL. You can define deep links in your navigation graph.

### Adding Deep Links

1. **Define Deep Link in Navigation Graph**:

```xml
<fragment
    android:id="@+id/secondFragment"
    android:name="com.example.app.SecondFragment"
    android:label="Second Fragment"
    tools:layout="@layout/fragment_second">
    <deepLink
        app:uri="https://www.example.com/secondFragment" />
</fragment>
```

2. **Handle Deep Links in Your Activity**:

Make sure your activity can handle the incoming deep link by checking the intent:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    val navController = findNavController(R.id.nav_host_fragment)
    navController.handleDeepLink(intent)
}
```

## 5. Bottom Navigation

Bottom navigation allows users to switch between top-level destinations in your app. You can implement it using the Navigation component.

### Setting Up Bottom Navigation

1. **Add BottomNavigationView to Your Layout**:

```xml
<com.google.android.material.bottomnavigation.BottomNavigationView
    android:id="@+id/bottom_navigation"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:menu="@menu/bottom_navigation_menu" />
```

2. **Create a Menu Resource**:

Create a menu resource file (e.g., `bottom_navigation_menu.xml`) in the `res/menu` folder:

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/firstFragment"
        android:title="First"
        android:icon="@drawable/ic_first" />
    <item
        android:id="@+id/secondFragment"
        android:title="Second"
        android:icon="@drawable/ic_second" />
</menu>
```

3. **Set Up Navigation with Bottom Navigation**:

In your activity, set up the `BottomNavigationView` with the `NavController`:

```kotlin
val navController = findNavController(R.id.nav_host_fragment)
val bottomNavigationView = findViewById<BottomNavigationView>(R.id.bottom_navigation)

bottomNavigationView.setupWithNavController(navController)
```

## 6. Using Safe Args for Type-Safe Argument Passing

Safe Args is a Gradle plugin that generates code for type-safe argument passing between destinations in your navigation graph.

### Setting Up Safe Args

1. **Add Safe Args Dependency**:

Add the following dependency to your `build.gradle` file:

```groovy
dependencies {
    classpath "androidx.navigation:navigation-safe-args-gradle-plugin:2.5.0"
}
```

2. **Apply the Plugin**:

In your app-level `build.gradle`, apply the Safe Args plugin:

```groovy
apply plugin: "androidx.navigation.safeargs.kotlin"
```

### Passing Arguments with Safe Args

1. **Define Arguments in Navigation Graph**:

```xml
<fragment
    android:id="@+id/secondFragment"
    android:name="com.example.app.SecondFragment"
    android:label="Second Fragment">
    <argument
        android:name="userId"
        app:argType="integer" />
</fragment>
```

2. **Navigating with Arguments**:

Use the generated directions class to navigate with arguments:

```kotlin
val action = FirstFragmentDirections.actionFirstFragmentToSecondFragment(userId)
navController.navigate(action)
```

3. **Receiving Arguments in the Destination Fragment**:

In the destination fragment, retrieve the arguments:

```kotlin
val args: SecondFragmentArgs by navArgs()
val userId = args.userId
```

## Conclusion

In this lesson, we covered how to implement the Navigation component in Android applications. We learned to create a navigation graph, handle transitions between fragments, and implement deep linking and bottom navigation. We also explored using Safe Args for type-safe argument passing. Mastering these concepts will help you create a smooth and user-friendly navigation experience in your Android applications. In the next lesson, we will discuss testing and debugging best practices to ensure the quality of your applications.