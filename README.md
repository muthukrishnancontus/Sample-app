# Mirrorfly-android-sdk

### Chat SDKs for Android

With CONTUS MirrorFly <a href="https://www.mirrorfly.com/tutorials/build-chat-app-for-android.php" target="_self">Chat SDK for Android</a>, you can efficiently integrate the desired real-time chat features into a client app.

When it comes to the client-side implementation, you can initialize and configure the chat with minimal efforts. With the server-side, MirrorFly ensures reliable infra-management services for the chat within the app. This page will let you know how to install the chat SDK in your app.

> **Note :** If you're looking for the fastest way in action with CONTUS MirrorFly <a href="https://www.mirrorfly.com/chat-api-solution.php" target="_self">Chat SDKs</a>, then you need to build your app on top of our sample version. Simply download the sample app and commence your app development. To download sample app <a href="https://github.com/MirrorFly/-MirrorFly-Android-Sample-V2" target="_blank">click here</a>


### Requirements
The requirements for chat SDK for Android are:
- Android Lollipop 5.0 (API Level 21) or above
- Java 7 or higher
- Gradle 4.1.0 or higher

### Integrate the Chat SDK

**Step 1:** Create a new project or Open an existing project in Android Studio

**Step 2:** If using Gradle 6.8 or higher, add the following code to your settings.gradle file. If using Gradle 6.7 or lower, add the following code to your root build.gradle file. See <a href="https://docs.gradle.org/6.8/release-notes.html#dm-features" target="_self">this release note</a> to learn more about updates to Gradle.

```groovy
    dependencyResolutionManagement {
        repositories {
           jcenter()
               maven {
                 url "https://repo.mirrorfly.com/snapshot"
       }
    }}
   ```

**Step 3:** Add the following dependencies in the app/build.gradle file.
   ```groovy
   dependencies {
       implementation 'com.mirrorfly.sdk:MirrorFlySDK:7.3.0'
    }
   ```

**Step 4:** Add the below line in the gradle.properties file, to avoid imported library conflicts.
   ```
   android.enableJetifier=true
   ```

**Step 5:** Open the AndroidManifest.xml and add below permissions.
   ```xml
   <uses-permission android:name="android.permission.INTERNET" />
   ```


## Registration

The below method to register a user in sandbox Live mode based on `setIsTrialLicenceKey` provided.

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

## Initialize Chat SDK

To start using the sdk, there is a need for some basic requirements before proceeding with the initialization process. Thus, the ChatSDK builder class is used to provide the necessary data to the SDK. In your `Application` class, inside the onCreate() method use the below ChatSDK Builder to provide the necessary data.

```kotlin
     ChatSDK.Builder()
     .setDomainBaseUrl("https://api-preprod-sandbox.mirrorfly.com/api/v1/")
     .setLicenseKey(LICENSE_KEY) //Please enter your license key
     .setIsTrialLicenceKey(true)
    .build()
```


## Connect to the Chat Server

In order to send messages using the Chat SDK, at first you need to establish the connection to the server. SDK provides methods for initialize the chat connection.

>Note:
The ChatManager.connect() method should be called only once in an application. SDK will handle the chat server connection and disconnection automatically.

```kotlin
     ChatManager.connect(object : ChatConnectionListener {
        override fun onConnected() {
          // Write your success logic here to navigate Profile Page or
          // To Start your one-one chat with your friends
          }

          override fun onDisconnected() {
              // Connection disconnected
              //No need implementations
         }

        override fun onConnectionNotAuthorized() {
             // Connection Not authorized
             //No need implementations
       }
})
```

## Send a One-to-One Message

Use the below method to send a text message to other user,

>Note: To generate a unique user id, you must call the other user's `username` method `String userJID = FlyUtils.getJid(USERNAME)`

```kotlin
     FlyMessenger.sendTextMessage(TO_JID, TEXT, listener = object : SendMessageListener {
       override fun onResponse(isSuccess: Boolean, chatMessage: ChatMessage?) {
        // you will get the message sent success response
        }
 })
 ```

## Receive a One-to-One Message

You can extend the `FlyBaseActivity` from SDK into your app `BaseActivity`, and observe all the incoming messages and other feature listeners.

```kotlin
   class MainActivity :  FlyBaseActivity()
   ```

Moreover, here the listeners would be called only when a new message is received from other user. To get more details please visit this [callback listeners](callback-listeners#observing-the-message-events)

```kotlin
    override fun onMessageReceived(message: ChatMessage) {
      super.onMessageReceived(message)
       // received message object
  }
  ```