# SEONIdVerification Android SDK Repository

## Description

SEONIdVerification Android SDK is a framework designed to streamline the identity verification process in your applications. This SDK facilitates integrating SEON's identity verification capabilities into your Android app seamlessly.

**Owner Team:** SEON Technologies Ltd.

## Installation

SEONIdVerification is available through Maven Central Repository. To install it, follow these steps:

##### If you are using kotlin version catalogs

- Add this into your project level (root) `settings.gradle.kts`:

```
dependencyResolutionManagement {
    repositories {
        ...
        mavenCentral()
    }
}
```
- Add these lines under the proper section in the `libs.versions.toml`:

```
[versions]
...
seonIdVerification = "1.2.0"
...
[libraries]
...
seon-id-verification = { group = "io.seon.idvandroid", name = "id-verification-sdk-android", version.ref = "seonIdVerification" }
```

- And add this into your module level (eg. app) `build.gradle.kts`:

```
dependencies {
    ...
    implementation(libs.seon.id.verification)
}
```

- You can now run gradle sync.

------------

##### If you are using older gradle dependency management
*(syntax may be vary for gradle or gradle.kts files)*

- Add this line to the project level (root) `build.gradle(.kts)`:

```
allprojects {
    repositories {
        ...
        mavenCentral()
    }
}
```

- And add this line to the module level (eg. app) `build.gradle(.kts)`:

```
dependencies {
    ...
    implementation "io.seon.idvandroid:id-verification-sdk-android:1.2.0"
}
```

- You can now run gradle sync.

------------

- To integrate the SDK first thing you have to do is init the activity result launcher (before onResume() is called), so you could do it initially in your activity:

```
private val activityResultLauncher =
    registerForActivityResult(StartActivityForResult()) { result: ActivityResult ->
        handleResult(result)
    }
```

- Next thing is to initialize the SDK through IDVService class (singleton) initialize(...) method:

```
IDVService.instance.initialize(
    baseUrl = "https://example.url/", // use the baseUrl you received from SEON
    customerData = IDVCustomerData(licenseKey = "0a1b3c4d5e"), // use the licenseKey you received from SEON,
    templateId = null, // Optional. Use the templateId if you have defined a template in the admin panel
    languageCode = "en" // use the language iso code you prefer to use in the SDK. If not provided, we fall back on the default Locale language of the app, and finally on our default language, which is English (for the available languages please contact SEON)
)
```

- And now you can start its verification flow by calling startVerificationFlow(...) method of the IDVService class:

```
IDVService.instance.startVerificationFlow(
    activityResultLauncher,
    applicationContext
)
```

- Also you have to add these permissions and features to your app's AndroidManifest.xml file:

```
<uses-permission android:name="android.permission.INTERNET" />

<uses-permission android:name="android.permission.CAMERA" />

<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"
    android:maxSdkVersion="28" />

<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"
    android:maxSdkVersion="32" />

<uses-feature
    android:name="android.hardware.camera"
    android:required="true" />
```

- Lastly you have to implement the handler method called by the activityResultLauncher:

```
private fun handleResult(result: ActivityResult) {
    when (result.resultCode) {
        IDVFlowResult.InterruptedByUser.code -> {
            // user navigated back
        }

        IDVFlowResult.Error.code -> {
            // handle error, its code is given in the bundle under the following key
            val error = result.data?.getStringExtra(VERIFICATION_ERROR_KEY)
        }

        IDVFlowResult.Completed.code -> {
            // handle completed status
        }
        
        IDVFlowResult.CompletedSuccess.code -> {
            // handle completed success status
        }
        
        IDVFlowResult.CompletedPending.code -> {
            // handle completed pending status
        }
        
        IDVFlowResult.CompletedFailed.code -> {
            // handle completed failed status
        }
    }
}
```

- You could also customize the SDK main colors through the IDVService class setThemeColors(...) method (the pair's first parameter is for light color, second is for dark color theme), please call it before you start the flow, but the best would be to call this even before the initialize(...) method is called

```
IDVService.instance.setThemeColors(
    textOnLight = Pair(getColor(R.color.black), getColor(R.color.white)),
    textOnDark = Pair(getColor(R.color.white), getColor(R.color.black)),
    accent = Pair(getColor(R.color.purple), getColor(R.color.purple)),
    onAccent = Pair(getColor(R.color.black), getColor(R.color.white)),
)
```

------------

# Changelog

## 1.2.0
-   Made referenceId required in the CustomerData

## 1.0.3
-   Liveness as a Standalone Project
-   Minor improvements and bugfixes
## 0.7.4
-   Some minor improvements on Liveness Check
-	Initial version on Maven
