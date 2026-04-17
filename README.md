# Upscope Android SDK

The Upscope Android SDK enables cobrowsing functionality in Android applications, allowing support agents to view and interact with your users' screens in real-time.

## Installation

Add the dependency to your app's `build.gradle` file:

```gradle
dependencies {
    implementation 'io.github.upscopeio:upscope-android-sdk:2026.4.6'
}
```

### Maven

```xml
<dependency>
    <groupId>io.github.upscopeio</groupId>
    <artifactId>upscope-android-sdk</artifactId>
    <version>2026.4.6</version>
</dependency>
```

## Quick Start

### 1. Initialize the SDK

In your `Application` class:

```kotlin
import io.upscope.sdk.Upscope
import io.upscope.sdk.UpscopeConfiguration

class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()

        val config = UpscopeConfiguration.Builder("your-api-key")
            .build()

        Upscope.initialize(applicationContext, config)
    }
}
```

### 2. Set Visitor Identity (Optional)

```kotlin
Upscope.uniqueId = "user-123"
Upscope.callName = "John Doe"
Upscope.identities = listOf("john@example.com")
Upscope.tags = listOf("premium")
```

### 3. Connect to Upscope

```kotlin
// Connect to Upscope servers (not needed if autoConnect is enabled)
Upscope.connect()

// Request a lookup code for easy session connection
Upscope.getLookupCode()

// Read the lookup code (available asynchronously)
val code = Upscope.lookupCode
```

### 4. Handle Connection State

Implement `UpscopeListener` to receive events:

```kotlin
import io.upscope.sdk.ConnectionState
import io.upscope.sdk.UpscopeListener

Upscope.listener = object : UpscopeListener {
    override fun onConnectionStateChanged(state: ConnectionState) {
        when (state) {
            is ConnectionState.Connected -> { /* Ready */ }
            is ConnectionState.Connecting -> { /* Establishing connection */ }
            is ConnectionState.Reconnecting -> { /* Reconnecting after drop */ }
            is ConnectionState.Inactive -> { /* Not connected */ }
            is ConnectionState.Error -> { /* Error: state.error */ }
        }
    }

    override fun onSessionStarted(agentName: String?) { /* Agent viewing */ }
    override fun onSessionEnded(reason: SessionEndReason) { /* Session over */ }
}
```

Or use Kotlin `StateFlow` for reactive observation:

```kotlin
// Collect connection state changes
Upscope.connectionStateFlow.collect { state -> /* ... */ }
Upscope.lookupCodeFlow.collect { code -> /* ... */ }
```

## Features

### App-Only Screen Capture

The SDK captures only your app's content — not the entire device screen. No special permissions are required from users. Activity tracking is handled automatically via lifecycle callbacks.

### Masking Sensitive Data

Protect sensitive information during screen sharing:

```kotlin
// Mask a specific view (content hidden from agent)
Upscope.addMaskedView(sensitiveView)

// Remove masking
Upscope.removeMaskedView(sensitiveView)

// Automatically mask all secure text inputs (enabled by default)
Upscope.maskSecureInputs = true
```

### Remote Control

Allow agents to interact with the user's screen:

```kotlin
val config = UpscopeConfiguration.Builder("your-api-key")
    .allowRemoteClick(true)
    .allowRemoteScroll(true)
    .requireControlRequest(true)  // Agent must request permission first
    .build()
```

### Session Authorization

Require user consent before screen sharing begins:

```kotlin
val config = UpscopeConfiguration.Builder("your-api-key")
    .requireAuthorizationForSession(true)
    .authorizationPromptTitle("Screen sharing request")
    .authorizationPromptMessage("An agent wants to view your screen")
    .build()
```

Or provide a custom authorization UI:

```kotlin
val config = UpscopeConfiguration.Builder("your-api-key")
    .requireAuthorizationForSession(true)
    .onSessionRequest { response, agentName ->
        // Show your own UI, then call response.accept() or response.reject()
        Cancellable { /* dismiss your UI */ }
    }
    .build()
```

## Requirements

- **Minimum SDK**: Android API 26 (Android 8.0)
- **Kotlin**: 2.0 or higher

## Permissions

The SDK requires minimal permissions:

- `INTERNET` — For connecting to Upscope servers
- `ACCESS_NETWORK_STATE` — For checking network connectivity

No camera, microphone, or system-level screen capture permissions are required.

## Documentation

- **Full documentation**: https://userview.com/docs/sdk/android
- **Email**: support@upscope.io

## License

This SDK is proprietary software. See [LICENSE](LICENSE) for more information.

