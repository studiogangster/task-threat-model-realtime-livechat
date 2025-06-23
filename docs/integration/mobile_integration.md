# Mobile Integration Guide

This section provides step-by-step instructions for integrating the Sprinklr Live Chat SDK into mobile applications (iOS and Android).

## Prerequisites

- Access to the Sprinklr Live Chat SDK for mobile (CocoaPods, Gradle, or direct download)
- Organization credentials (orgId, apiKey)
- A native iOS (Swift/Objective-C) or Android (Kotlin/Java) application

## Installation

### iOS (Swift)

Add the SDK to your Podfile:

```ruby
pod 'SprinklrLiveChatSDK'
```

Then run:

```bash
pod install
```

### Android (Kotlin/Java)

Add the SDK to your build.gradle dependencies:

```gradle
implementation 'com.sprinklr:live-chat-sdk:1.0.0'
```

## Initialization

### iOS

```swift
import SprinklrLiveChatSDK

let chat = SprinklrChatSDK(orgId: "YOUR_ORG_ID", apiKey: "YOUR_API_KEY", environment: .production)
```

### Android

```kotlin
val chat = SprinklrChatSDK(
    orgId = "YOUR_ORG_ID",
    apiKey = "YOUR_API_KEY",
    environment = "production"
)
```

## Authentication

- Obtain a session token for the end user from your backend.
- Pass the session token to the SDK when starting a chat.

## Starting a Chat

### iOS

```swift
chat.connect()
chat.startChat(sessionToken: sessionToken, userInfo: ["name": "User Name", "email": "user@example.com"])
```

### Android

```kotlin
chat.connect()
chat.startChat(sessionToken, mapOf("name" to "User Name", "email" to "user@example.com"))
```

## Handling Messages

Register callbacks or listeners to receive messages and update your chat UI.

## Disconnecting

Call the appropriate disconnect method to end the chat session.

## Security Best Practices

- Never expose API keys or sensitive credentials in client-side code.
- Use HTTPS for all communications.
- Validate and sanitize all user input.

See the troubleshooting section for common integration issues.
