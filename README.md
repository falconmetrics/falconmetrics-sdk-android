# FalconMetrics SDK for Android

FalconMetrics is a lightweight analytics and attribution tracking SDK for Android applications. This SDK allows you to track various user events and conversions in your Android app to measure marketing effectiveness and user engagement.

## Features

- Easy integration with minimal setup
- Event tracking for user actions (sign-ups, purchases, etc.)
- Privacy-compliant with opt-out capabilities
- Efficient background processing with minimal battery impact
- Automatic install referrer tracking

## Installation

### Gradle

Add the FalconMetrics SDK to your app's build.gradle file:

```gradle
dependencies {
    implementation 'io.falconmetrics:falconmetrics:0.3.0'
}
```

## Usage

### Initialization

Initialize the SDK in your Application class or main activity:

```kotlin
// In your Application class
class MyApp : Application() {
    
    lateinit var falconMetrics: FalconMetrics
    
    override fun onCreate() {
        super.onCreate()
        
        // Create the FalconMetrics instance
        falconMetrics = FalconMetricsSdk.create(applicationContext)
        
        // Initialize with your API key
        falconMetrics.init("YOUR_API_KEY")
    }
}
```

### Tracking Events

The SDK supports tracking various event types:

#### User Sign-Up or Login

```kotlin
// Track when a completes registration or onboarding
falconMetrics.trackEvent(CompleteRegistrationEvent())
```

#### Add to Cart Event

```kotlin
// Track when a user adds an item to cart
falconMetrics.trackEvent(
    AddedToCartEvent(
        itemId = "product-123",
        quantity = 2,
        productPriceInCents = 1099, // $10.99
        currency = "USD",
        productCategory = "Electronics",
        cartId = "cart-456"
    )
)
```

#### SubscribeEvent Event

```kotlin
// Track when a user applies a coupon
falconMetrics.trackEvent(
    SubscribeEvent(
        currency = "USD",
        predictedLtvValueInCents = "10000"
    )
)
```

#### Purchase Event

```kotlin
// Track when a user completes a purchase
falconMetrics.trackEvent(
    PurchaseEvent(
        itemIds = listOf("product-123", "product-456"),
        quantity = 2,
        transactionId = "order-789",
        productPriceInCents = 1099, // $10.99
        currency = "USD",
        revenueInCents = 2198, // $21.98
        productCategory = "Electronics",
        cartId = "cart-456",
        paymentMethod = "credit_card",
        taxInCents = 175, // $1.75
        shippingCostInCents = 499, // $4.99
        discountInCents = 220 // $2.20
    )
)
```

#### Custom Event

Custom events are powerful and provide a lot of flexibility. Yoy need to make sure that the eventName
matches the event in the ad network. 

You can add optionally revenue and a currency to the event to write revenue to the ad network.

```kotlin
// Track a custom event
falconMetrics.trackEvent(
    CustomEvent(
        eventName = "MyEvent",
        revenueInCents = 1000,
        currency = "USD",
        attributes= mapOf<String, Any>("key1" to "value1", "key2" to 2)
    )
)
```

### Google advertising ID

For a more accurate attribution it is recommended to enable Google advertising ID tracking. To do this, add the following permission to your AndroidManifest.xml file: 

```xml
  <uses-permission android:name="com.google.android.gms.permission.AD_ID"/>
```

The FalconMetrics sdk automatically tracks the Google advertising ID for you and takes into account the users consent to use the ID.

### Meta Referrer

The meta refferer is more elaborate thant the Google Play Install referrer. Google Play Install referrer only provides info for same session click through installs, while the Meta Referrer provides info for view through installs, click through installs and multiple session click through installs. The FalconMetrics sdk automatically retrieves and processes the meta referrer in order to provide the most accurate attribution possible.

Add the following `<queries>` element inside the root `<manifest>` tag to enable Meta Referrer support:

```xml
<queries>
  <package android:name="com.facebook.katana" />
</queries>

<queries>
  <package android:name="com.instagram.android" />
</queries>

<queries>
  <package android:name="com.facebook.lite" />
</queries>
```

### Privacy Control

The SDK provides methods to enable or disable tracking based on user consent:

```kotlin
// Disable tracking
falconMetrics.setTracking(context, false)

// Check if tracking is enabled
val isEnabled = falconMetrics.isTrackingEnabled(context)

// Re-enable tracking
falconMetrics.setTracking(context, true)
```

By default, tracking is enabled when the SDK is initialized.

## ProGuard Configuration

The SDK includes consumer ProGuard rules, so you don't need to add any additional ProGuard configuration to your project.

## Requirements

- Android API level 21 (Android 5.0) or higher
- Kotlin 1.5 or higher

## License

© 2025 FalconMetrics LLC. All rights reserved.

This SDK is licensed under the FalconMetrics SDK License Addendum. Use of this SDK is subject to the FalconMetrics Terms of Use and SDK License, available at: https://www.falconmetrics.io/terms

## Support

For questions or support, please contact support@falconmetrics.io or visit our documentation at https://docs.falconmetrics.io
