# Firebase App Distribution via fastlane

Purpose of the project
----------------------

:boom: This project will help you distribute your app with the power of Firebase App Distribution and fastlane. This sample also guides you how to organize a multi-build types and a multi-flavor Android application. 

Table of contents
=================

<!--ts-->
   * [Firebase App Distribution via fastlane](#purpose-of-the-project)
   * [Tools and components](#tools-and-components)
      * [Firebase App Distribution](#firebase-app-distribution)
      * [fastlane](#fastlane)
   * [To-do list](#to-do-list)
      * [Firebase side](#firebase-side)
        * [Create a Firebase project](#create-a-firebase-project)
        * [Register your app with Firebase](#register-your-app-with-firebase)
        * [Add a Firebase configuration file](#add-a-firebase-configuration-file)
        * [Add Firebase SDKs to your app](#add-firebase-sdks-to-your-app)
      * [Android side](#android-side)
        * [Setup fastlane]()
<!--te-->

Tools and components
====================

Firebase App Distribution
-------------------------

:fire: **Firebase App Distribution** makes distributing your apps to trusted testers painless. By getting your apps onto testers' devices quickly, you can get feedback early and often. And, if you use Crashlytics in your apps, you’ll automatically get stability metrics for all your builds, so you know when you’re ready to ship. Please visit [Firebase App Distribution page](https://firebase.google.com/docs/app-distribution) fore more detail.

fastlane
--------

:rocket: **fastlane** is the easiest way to automate beta deployments and releases for your iOS and Android apps. 🚀 It handles all tedious tasks, like generating screenshots, dealing with code signing, and releasing your application. Please visit [fastlane page](https://docs.fastlane.tools/) fore more detail.

To-do list
==========

Firebase side
-------------

In this part, we will learn how to register our app via [Firebase Console](https://console.firebase.google.com/u/0/).

#### Create a Firebase project

Before you can add Firebase to your Android app, you need to create a Firebase project to connect to your Android app. [Visit Understand Firebase Projects](https://firebase.google.com/docs/projects/learn-more) to learn more about Firebase projects.

1. In the [Firebase console](https://console.firebase.google.com/u/0/), click **Add project**, then select or enter a **Project name**.
<p align="left" style="padding-left: 15px">
    <img src="images/firebase_console.png" width="250" />
</p>

2. Click **Continue**.
3. Click **Create project**.

#### Register your app with Firebase

1. In the center of the [Firebase console's project overview page](https://console.firebase.google.com/u/0/), click the **Android** icon to launch the setup workflow.
2. Enter your app's [application ID](https://developer.android.com/studio/build/application-id) in the **Android package name** field. An *application ID* is sometimes referred to as a **package name** (**e.g.** com.yourcompany.yourproject)
3. Enter other app information as prompted by the setup workflow. (SHA-1)
   * To get the debug certificate fingerprint, copy and paste below code block into your Terminal:
   `keytool -list -v \ -alias androiddebugkey -keystore ~/.android/debug.keystore`
   * The default password for the debug keystore is android. The keytool then prints the fingerprint to the terminal. For example:
   `Certificate fingerprint: SHA1: DA:39:A3:EE:5E:6B:4B:0D:32:55:BF:EF:95:60:18:90:AF:D8:07:09`
4. Click **Register app**.

#### Add a Firebase configuration file

1. Add the Firebase Android configuration file to your app:
    * Click **Download google-services.json** to obtain your Firebase Android config file (google-services.json).
    * Move your config file into the module (app-level) directory of your app. **This is not required anymore, hereafter you can move all required info into strings resources. This is explained in the [Android side](#android-side)**
    
      ```
      dependencies {
      // Add the following line:
      classpath 'com.google.gms:google-services:4.3.2'  // Google Services plugin
      }
      ```
    * In your module (app-level) Gradle file (usually app/build.gradle), add a line to the bottom of the file.
    
        ```
        android {
    
        }

        // Add the following line to the bottom of the file:
            apply plugin: 'com.google.gms.google-services'  // Google Play services Gradle plugin
        ```
#### Add Firebase SDKs to your app

1. To your module (app-level) Gradle file (usually app/build.gradle), add the dependencies for the Firebase products that you want to use in your app.
    ```
    dependencies {
    // ...

    // Add the Firebase SDK for Google Analytics
    implementation 'com.google.firebase:firebase-analytics:17.2.1'

    // Add the SDKs for any other Firebase products you want to use in your app
    // For example, to use Firebase Authentication and Cloud Firestore
    implementation 'com.google.firebase:firebase-auth:19.1.0'
    implementation 'com.google.firebase:firebase-firestore:21.2.1'

    // Getting a "Could not find" error? Make sure that you've added
    // Google's Maven repository to your root-level build.gradle file
    }
    ```
2. Sync your app to ensure that all dependencies have the necessary versions.
3. If you added Analytics, run your app to send verification to Firebase that you've successfully integrated Firebase. Otherwise, you can skip the verification step. **Firebase usually needs a verification for distribution so that you need add Analytics and run the app.**

Android side
------------

#### Setup fastlane

1. Navigate to your app folder or on Terminal inside Android Studio, type the command below:

    `sudo gem install fastlane -NV`
2. Verify that the installation was successful by typing this command in the CLI:
`
    `fastlane -version`
3. Run `fastlane init`
4. Provide the package name for your application when asked (**e.g.** com.thedancercodes.fastlanedroid)
5. Press enter when asked for the path to your json secret file
6. Answer ’n’ when asked if you plan on uploading info to Google Play via Fastlane (we can set this up later)

That’s it! ***Fastlane*** will automatically generate a configuration for you based on the information provided.
This process generates our App file and Fastfile. Below is a screenshot of the Fastlane directory in your project containing these files.
<p align="left">
    <img src="images/fastlane_folder.png" width="250" />
</p>

**Appfile:** Defines configuration information that is global to your app.

**Fastfile:** Defines different lanes that drive the behavior of fastlane to do different tasks.

*The Fastfile defines what steps run & in what order Fastlane runs them.
A group of steps is called a lane*

Below are code snippets show the contents of the **Fastfile** & **Appfile**:

| App file | Fast File
|:----------------:|:---------------:|
| <img src="images/app_file.png" width="350" />  | <img src="images/fast_file.png" width="350"/>