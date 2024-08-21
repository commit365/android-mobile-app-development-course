# Lesson 2: Setting Up the Development Environment

In this lesson, we will guide you through the process of setting up your development environment for Android app development. This includes installing Android Studio, configuring the necessary SDKs, understanding the Gradle build system, and creating your first Android project.

## 1. Installing Android Studio

Android Studio is the official Integrated Development Environment (IDE) for Android development. It provides tools for coding, debugging, and testing your applications. Follow these steps to install Android Studio:

### Step 1: Download Android Studio
- Visit the [Android Studio download page](https://developer.android.com/studio).
- Click on the download button for your operating system (Windows, macOS, or Linux).

### Step 2: Install Android Studio
- **Windows**: Run the downloaded `.exe` file and follow the installation wizard. Make sure to select the option to install the Android SDK and the Android Virtual Device (AVD).
- **macOS**: Open the downloaded `.dmg` file and drag Android Studio to your Applications folder. Launch Android Studio from your Applications.
- **Linux**: Extract the downloaded `.zip` file to a suitable location. Open a terminal and navigate to the extracted folder, then run `studio.sh` to start Android Studio.

### Step 3: Initial Setup
- When you first launch Android Studio, it may prompt you to import settings from a previous version. If this is your first installation, select "Do not import settings."
- Follow the setup wizard to install the necessary components, including the Android SDK and emulator. This process may take some time, depending on your internet connection.

## 2. Configuring SDKs

Once Android Studio is installed, you need to configure the Android SDK:

### Step 1: Open SDK Manager
- In Android Studio, click on `File` > `Settings` (or `Android Studio` > `Preferences` on macOS).
- In the left sidebar, select `Appearance & Behavior` > `System Settings` > `Android SDK`.

### Step 2: Install SDK Platforms
- In the SDK Platforms tab, check the box for the latest Android version (e.g., Android 13.0) and any other versions you may want to support.
- Click `Apply` to download and install the selected SDK platforms.

### Step 3: Install SDK Tools
- Switch to the SDK Tools tab and ensure that the following tools are checked:
  - Android SDK Build-Tools
  - Android Emulator
  - Android SDK Platform-Tools
  - Android SDK Tools
- Click `Apply` to install the selected tools.

## 3. Understanding the Gradle Build System

Gradle is the build system used by Android Studio to compile and package your applications. It manages dependencies, builds variants, and handles other tasks related to the build process.

### Key Concepts:
- **Build Scripts**: Gradle uses build scripts written in Groovy or Kotlin DSL to define how your project is built. The main script is usually located in the `build.gradle` file.
- **Modules**: A module is a discrete unit of functionality in your app. Each module has its own build script and can be an Android app, a library, or a test module.
- **Dependencies**: Gradle allows you to specify libraries and other modules that your app depends on. These dependencies are automatically downloaded and included in your project.

## 4. Creating Your First Android Project

Now that your development environment is set up, let’s create your first Android project.

### Step 1: Start a New Project
- Open Android Studio and select `New Project`.
- Choose a project template. For beginners, the `Empty Activity` template is a good starting point. Click `Next`.

### Step 2: Configure Your Project
- **Name**: Enter a name for your project (e.g., "MyFirstApp").
- **Package Name**: This is a unique identifier for your app (e.g., "com.example.myfirstapp").
- **Save Location**: Choose a directory to save your project.
- **Language**: Select `Kotlin` as the programming language.
- **Minimum API Level**: Choose the minimum Android version you want to support. A common choice is API 21 (Android 5.0).
- Click `Finish` to create your project.

### Step 3: Exploring Project Structure
Once your project is created, take a moment to explore the project structure:

- **app/**: This directory contains your application code and resources.
  - **src/**: Contains the source code for your app.
    - **main/**: The main source set for your app.
      - **java/**: Contains your Kotlin code files.
      - **res/**: Contains resources such as layouts, strings, and images.
      - **AndroidManifest.xml**: The manifest file that describes your app’s components and permissions.
- **build.gradle (Project Level)**: This file manages project-wide settings and dependencies.
- **build.gradle (Module Level)**: This file manages settings and dependencies specific to the app module.

### Step 4: Running Your Project
- To run your project, you can use the Android Emulator or a physical Android device.
- Click on the green play button in the toolbar, select your device, and click `OK`. Android Studio will build your project and launch it on the selected device.

In this lesson, you have successfully set up your development environment, installed Android Studio, configured the SDKs, understood the Gradle build system, and created your first Android project. In the next lesson, we will dive into the Android app structure and key components.