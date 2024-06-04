# Mirrorfly UIKit for Android

### Chat SDKs for Android

With CONTUS MirrorFly <a href="https://www.mirrorfly.com/docs/chat/android/v3/quick-start/" target="_self">Chat SDK for Android</a>, you can efficiently integrate the desired real-time chat features into a client app.

When it comes to the client-side implementation, you can initialize and configure the chat with minimal efforts. With the server-side, MirrorFly ensures reliable infra-management services for the chat within the app. This page will let you know how to install the chat SDK in your app.

> **Note :** If you're looking for the fastest way in action with CONTUS MirrorFly <a href="https://www.mirrorfly.com/chat-api-solution.php" target="_self">Chat SDKs</a>, then you need to build your app on top of our sample version. Simply download the sample app and commence your app development. To download sample app <a href="https://github.com/MirrorFly/-MirrorFly-Android-Sample-V2" target="_blank">click here</a>


### Requirements
The requirements for chat SDK for Android are:
```groovy
- Android Lollipop 5.0 (API Level 21) or above
- Java 7 or higher
- Gradle 4.1.0 or higher
```
> **Note :** If you're utilizing Chat SDK version 7.11.4 or higher, it's necessary to adjust the target SDK version to 34. This is due to the migration of Chat SDK to Android 14.

### Integrate the Chat SDK

**Step 1:** Create a new project or Open an existing project in Android Studio

**Step 2:** If using `Gradle 6.8` or higher, add the following code to your settings.gradle file. If using `Gradle 6.7` or lower, add the following code to your root build.gradle file. See <a href="https://docs.gradle.org/6.8/release-notes.html#dm-features" target="_self">this release note</a> to learn more about updates to Gradle.

```gradle
dependencyResolutionManagement {
        repositories {
            mavenCentral()
               maven {
                 url "https://repo.mirrorfly.com/release"
                 }
       }
}
   ```

**Step 3:** Add the following dependencies in the app/build.gradle file.
   ```gradle
 apply plugin: 'com.android.application'

android {
    buildFeatures {
        viewBinding true
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
      implementation 'com.mirrorfly.sdk:MirrorFlySDK:7.13.0'
}
   ```

**Step 4:** Add the below line in the gradle.properties file, to avoid imported library conflicts.
   ```gradle
   android.enableJetifier=true
   ```

**Step 5:** Open the AndroidManifest.xml and add below permissions.
   ```xml
   <uses-permission android:name="android.permission.INTERNET" />
   ```
After saving your build.gradle file, click the Sync button to apply all the changes.


## Initialize Chat SDK

To begin utilizing the SDK, it is necessary to provide the **LicenceKey** using the **ChatManager.initializeSDK()** method. In your Application class, within the **onCreate()** method, use the following ChatManager.initializeSDK() to supply the required data.
```kotlin
ChatManager.initializeSDK(LICENSE_KEY) { isSuccess, _, data -> 
    if (isSuccess) {
        //success
    } else {
        //failure
    }
}
```

## Registration

The below method to register a user in sandbox Live mode based on **LicenceKey** provided.

>Note: While registration, the below `registerUser` method will accept the `FCM_TOKEN`,`USER_TYPE` and `FORCE_REGISTER` as an optional param and pass it across.

```kotlin
  FlyCore.registerUser(USER_IDENTIFIER) { isSuccess, throwable, data ->
        if(isSuccess) {
            val isNewUser = data["is_new_user"] as Boolean
            val responseObject = data.get("data") as JSONObject
            // Get Username and password from the object
        } else {
            // Register user failed print throwable to find the exception details.
        }       
     }
```


## Connect to the Chat Server

After registration, the chat server will be `connected` and managed within the SDK. The SDK will provide a callback once it is connected. You can extend the `FlyBaseActivity` from SDK into your app's `BaseActivity`, and you will be notified through the following override method:

```kotlin
class MainActivity :  FlyBaseActivity()
   ```
Here is the callback,
```kotlin
override fun onConnected() {
super.onConnected()
// Chat server connected.
}
```


## Send a One-to-One Message

Use the below method to send a text message to other user,

>Note: To generate a unique user id, you must call the other user's `username` method `String jid  = FlyUtils.getJid(USERNAME)`

```kotlin

val sendMessageParams = TextMessage().apply {
    toId = TO_JID
    messageText = TEXT_MESSAGE
}
FlyMessenger.sendTextMessage(sendMessageParams, object : SendMessageCallback {
    override fun onResponse(
        isSuccess: Boolean,
        error: Throwable?,
        chatMessage: ChatMessage?
    ) {
    
    }
})
 ```

## Receive a One-to-One Message

You can extend the `FlyBaseActivity` from SDK into your app `BaseActivity`, and observe all the incoming messages and other feature listeners.

```kotlin
class MainActivity :  FlyBaseActivity()
   ```

Moreover, here the listeners would be called only when a new message is received from other user. To get more details please visit this <a href="https://www.mirrorfly.com/docs/chat/android/v3/callback-listeners/#observing-the-message-events" target="_self"> [callback listeners] </a>

```kotlin
override fun onMessageReceived(message: ChatMessage) {
      super.onMessageReceived(message)
       // received message object
  }
  ```
