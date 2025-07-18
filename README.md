# Upscope Android SDK

The Upscope Android SDK enables cobrowsing functionality in Android applications, allowing support agents to view and interact with your users' screens in real-time.

## Installation

### Gradle (Recommended)

Add the dependency to your app's `build.gradle` file:

```gradle
dependencies {
    implementation 'io.upscope:upscope-android-sdk:2025.7.0'
}
```

### Maven

Add the dependency to your `pom.xml`:

```xml
<dependency>
    <groupId>io.upscope</groupId>
    <artifactId>upscope-android-sdk</artifactId>
    <version>2025.7.0</version>
</dependency>
```

## Quick Start

### 1. Initialize the SDK

In your `Application` class:

```kotlin
import io.upscope.sdk.UpscopeManager

class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        
        // Initialize Upscope SDK
        UpscopeManager.create(
            apiKey = "your_api_key_here",
            context = this
        )
    }
}
```

### 2. Set up Screen Capture

In your main `Activity`:

```kotlin
import io.upscope.sdk.UpscopeManager

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        // Set up app-level screen capture
        UpscopeManager.shared?.setupWithActivity(this)
    }
}
```

### 3. Connect to Upscope

```kotlin
// Connect to Upscope servers
UpscopeManager.shared?.connect()

// Get a lookup code for easy session connection
val lookupCode = UpscopeManager.shared?.getLookupCode()
// Share this code with your support agent
```

### 4. Handle Connection Status

```kotlin
UpscopeManager.shared?.addConnectionStatusObserver { status ->
    when (status) {
        ConnectionStatus.Connected -> {
            // SDK is connected and ready
        }
        ConnectionStatus.Disconnected -> {
            // SDK is disconnected
        }
        ConnectionStatus.Connecting -> {
            // SDK is attempting to connect
        }
    }
}
```

## Features

### App-Only Screen Capture

The SDK uses **app-only capture** which:
- Captures only your app's content, not the entire device screen
- Requires no special permissions from users
- Provides better privacy and security
- Works seamlessly with all Activities that extend ComponentActivity

### Redaction of Sensitive Data

Protect sensitive information during screen sharing:

#### View-Based UI
```kotlin
// Mark a view as redacted
sensitiveView.markAsRedacted()
```

#### Jetpack Compose
```kotlin
// Use the redaction modifier
TextField(
    value = password,
    onValueChange = { password = it },
    modifier = Modifier.upscopeRedacted()
)
```

Enable redaction globally:
```kotlin
UpscopeManager.shared?.redactionEnabled = true
```

## Requirements

- **Minimum SDK**: Android API 21 (Android 5.0)
- **Target SDK**: Android API 34+
- **Kotlin**: 1.8.0 or higher
- **Gradle**: 7.0 or higher

## Permissions

The SDK requires minimal permissions:
- `INTERNET` - For connecting to Upscope servers
- `ACCESS_NETWORK_STATE` - For checking network connectivity

No camera, microphone, or system-level screen capture permissions are required.

## Sample Application

Check out our [sample application](https://github.com/upscopeio/cobrowsing-android/tree/main/sample-app) for a complete implementation example.

## Documentation

For detailed documentation, visit: [https://docs.upscope.io/docs/android-sdk](https://docs.upscope.io/docs/android-sdk)

## Support

- **Email**: support@upscope.io
- **Documentation**: https://docs.upscope.io
- **GitHub Issues**: https://github.com/upscopeio/cobrowsing-android/issues

## License

This SDK is proprietary software. See [LICENSE](LICENSE) for more information.

## Version

Current version: 2025.7.0