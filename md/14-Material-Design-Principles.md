# Lesson 14: Material Design Principles

In this lesson, we will explore Material Design, a design system developed by Google that provides guidelines for creating visually appealing and user-friendly interfaces. We will cover the key principles of Material Design, how to implement various components such as Buttons, Cards, Snackbars, and Bottom Sheets, and how to customize themes and styles for responsive layouts.

## 1. Understanding Material Design Guidelines

Material Design emphasizes the following principles:

- **Material as a Metaphor**: Use visual cues that mimic real-world materials, such as shadows and depth, to create a tactile interface.
- **Bold, Graphic, and Intentional**: Use bold colors, typography, and imagery to create a visually striking interface.
- **Motion Provides Meaning**: Use animations and transitions to provide feedback and guide users through interactions.

### Key Components of Material Design

Material Design provides a wide range of UI components that can be used to build applications. These components are designed to be consistent across different platforms and devices.

## 2. Implementing Material Design Components

### Buttons

Buttons are essential for user interaction. Material Design provides several types of buttons:

- **Text Button**: A flat button without elevation.
  
  ```xml
  <com.google.android.material.button.MaterialButton
      android:id="@+id/text_button"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Text Button"
      style="@style/Widget.MaterialComponents.Button.TextButton" />
  ```

- **Contained Button**: A button with a filled background.
  
  ```xml
  <com.google.android.material.button.MaterialButton
      android:id="@+id/contained_button"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Contained Button"
      style="@style/Widget.MaterialComponents.Button.Contained" />
  ```

- **Outlined Button**: A button with an outlined border.
  
  ```xml
  <com.google.android.material.button.MaterialButton
      android:id="@+id/outlined_button"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Outlined Button"
      style="@style/Widget.MaterialComponents.Button.Outlined" />
  ```

### Cards

Cards are used to display content and actions related to a single subject. They provide a clean and organized way to present information.

```xml
<androidx.cardview.widget.CardView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:cardElevation="4dp"
    app:cardCornerRadius="8dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:id="@+id/card_title"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Card Title"
            android:textSize="18sp"
            android:textStyle="bold" />

        <TextView
            android:id="@+id/card_content"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="This is some content inside the card." />
    </LinearLayout>
</androidx.cardview.widget.CardView>
```

### Snackbars

Snackbars provide brief messages at the bottom of the screen to inform users about an action. They can include an optional action button.

```kotlin
Snackbar.make(findViewById(R.id.root_layout), "This is a Snackbar", Snackbar.LENGTH_LONG)
    .setAction("Undo") {
        // Handle action
    }
    .show()
```

### Bottom Sheets

Bottom Sheets are used to display additional content or actions without leaving the current screen. They can be persistent or modal.

#### Modal Bottom Sheet

1. **Add Dependency**:

```groovy
dependencies {
    implementation "com.google.android.material:material:1.4.0"
}
```

2. **Create Bottom Sheet Layout**:

```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="This is a Bottom Sheet" />
</LinearLayout>
```

3. **Show the Bottom Sheet**:

```kotlin
val bottomSheetDialog = BottomSheetDialog(this)
bottomSheetDialog.setContentView(R.layout.bottom_sheet_layout)
bottomSheetDialog.show()
```

## 3. Customizing Themes and Styles

Material Design encourages the use of themes and styles to ensure a consistent look and feel across your app.

### Defining a Theme

In your `res/values/styles.xml`, define a theme that extends from Material Components:

```xml
<resources>
    <style name="AppTheme" parent="Theme.MaterialComponents.Light.NoActionBar">
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryVariant">@color/colorPrimaryDark</item>
        <item name="colorOnPrimary">@android:color/white</item>
        <item name="colorSecondary">@color/colorAccent</item>
        <item name="colorOnSecondary">@android:color/white</item>
    </style>
</resources>
```

### Applying the Theme

Apply the theme in your `AndroidManifest.xml`:

```xml
<application
    android:theme="@style/AppTheme">
    ...
</application>
```

### Customizing Styles

You can create custom styles for specific components. For example, to customize a button style:

```xml
<resources>
    <style name="CustomButton" parent="Widget.MaterialComponents.Button">
        <item name="android:backgroundTint">@color/colorAccent</item>
        <item name="android:textColor">@android:color/white</item>
    </style>
</resources>
```

### Applying Custom Styles

Apply the custom style to your button:

```xml
<com.google.android.material.button.MaterialButton
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    style="@style/CustomButton"
    android:text="Custom Button" />
```

## 4. Creating Responsive Layouts

Responsive layouts ensure that your app looks good on different screen sizes and orientations. Use the following techniques to create responsive designs:

### Use ConstraintLayout

`ConstraintLayout` allows you to create complex layouts while maintaining performance. It enables you to position elements relative to each other and the parent layout.

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/title"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Title"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me"
        app:layout_constraintTop_toBottomOf="@id/title"
        app:layout_constraintStart_toStartOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

### Use Different Layouts for Different Screen Sizes

You can create different layout resources for different screen sizes and orientations by creating resource folders such as `layout-sw600dp` for tablets or `layout-land` for landscape orientation.

### Example of Layout Resource Folders

- `res/layout/main_activity.xml` (default layout)
- `res/layout-sw600dp/main_activity.xml` (for devices with a minimum width of 600dp)
- `res/layout-land/main_activity.xml` (for landscape orientation)

## Conclusion

In this lesson, we explored Material Design principles and learned how to implement various Material Design components such as Buttons, Cards, Snackbars, and Bottom Sheets. We also covered customizing themes and styles and creating responsive layouts for different screen sizes and orientations. Understanding these concepts will help you create visually appealing and user-friendly applications that adhere to Material Design guidelines. In the next lesson, we will dive into testing and debugging best practices to ensure the quality of your applications.