
<h1>
  <img src="https://axeptio.imgix.net/2024/07/e444a7b2-ea3d-4471-a91c-6be23e0c3cbb.png" alt="Descrizione immagine" width="80" style="vertical-align: middle; margin-right: 10px;" />
  QA Testing Documentation for Axeptio
</h1>

Here you’ll find everything you need to test most of the features related to the Axeptio SDK, both in the Brand and TCF environments. This guide gathers useful steps, handy links, important tips, and even a few “QA ninja” tricks 🥷.

Whether you're working on a new feature, validating a critical fix, or testing a specific PR, this document is here to be your trusty sidekick — helping you figure out what to test, where to test it, and how to do it as smoothly as possible.

Ready? Let’s go! 🚀

## 📑 Table of Contents
1. [Testing BO Changes from a PR](#testing-bo-changes-from-a-pr)
2. [Testing the SDK PR with a specific hash](#testing-the-sdk-pr-with-a-specific-hash)
3. [Running the Widget SDK Locally](#running-the-widget-sdk-locally)
4. [Checking `axeptioSDK.settings` in the Browser Console](#checking-axeptiosdk.settings-in-the-browser-console)
5. [Android Sample App](#android-sample-app)
   - [Local Testing](#locaL-testing)
6. [iOS Sample App](#ios-sample-app)
7. [Flutter Sample App](#flutter-sample-app)

<br> 

# Testing BO Changes from a PR
When a PR includes changes to the Back Office (BO), it usually provides a temporary preview environment — a dedicated URL where the fix or new feature can be tested before the PR is merged.

If there are issues or modifications under review, you will find this preview link directly in the PR description. This environment reflects the staging setup, but includes the specific changes of that PR.

You can access it using your staging credentials.

Once the fix or feature has been validated in this preview environment:

- It will be deployed to the actual staging environment.
- From there, you can perform another round of tests if needed.
- Finally, the changes will be promoted to production.

<br> 

# Testing the SDK PR with a specific hash
To test the SDK version from a specific PR, use the hash-based version of the SDK instead of the default production one.

### ✅ Options to Use the SDK with the PR Hash
- Client-Side App
  Use the SDK by appending the PR hash to the SDK URL directly in your integration:
  ```arduino
  https://static.axept.io/sdk-pr-0e97ef9a-66e0-568e-b740-8cc7f62d8ed8.js
  ```

- Local HTML File (Static)
  You can test locally by referencing the desired SDK directly via the full PR URL:
  ```html
  <script src="https://static.axept.io/sdk-pr-0e97ef9a-66e0-568e-b740-8cc7f62d8ed8.js"></script>
  ```

Applies to:
- TCF
- Brands

### Note on Production URLs:
The following URLs point to the latest production version of the SDK:

- `https://static.axept.io/sdk.js`

- `https://static.axept.io/tcf/sdk.js`

When testing a PR, always use the hash-specific URL to ensure you are testing the correct version of the SDK.

<br> 

# Running the Widget SDK Locally
For deeper testing or debugging, it’s possible to run the widget SDK locally on your machine. This is especially useful when working directly with SDK code or when needing to test changes before they are published.

## 🛠️ Steps to run it locally:
1. Run `yarn` to install dependencies.
2. Run `yarn update:assets` (updates necessary assets like fonts, icons, etc.).
3. Open `index.html` and update the `clientId` and `cookiesVersion` accordingly.
4. Start the local server:
   ```bash
   yarn start:sdk
   ```
5. Open your browser and go to: [https://localhost:9100/](https://localhost:9100/)

> ⚠️ **Note:** Make sure your local environment allows HTTPS on `localhost`,  
> or your browser might block the widget from loading properly.

<br> 

# Checking `axeptioSDK.settings` in the Browser Console

You can access `axeptioSDK.settings` directly from your browser’s developer console to inspect the current SDK configuration.

This object returns detailed information about the project, including:

- The **project settings**
- The **steps** defined in the consent flow
- The list of **vendors** associated with each step

This is especially useful for debugging or verifying that the correct configuration and flow are being loaded in your environment.
Below are some of the parameters returned in the console output:  

```js
{
  allVendorsCookieName:
  apiUrl: 
  authorizedVendorsCookieName: 
  clientId: 
  cookiesConfig: 
  cookiesVersion: 
  dataLayerName: 
  googleConsentMode: 
  jsonCookieName: 
  mountClassName: 
  openCookiesWidgetIfVendorsMismatch: 
  platform: 
  postConsentUrl: 
  tokenInstance: {
    existing: 
    value: 
  }
  triggerGTMEvents:
  userCookiesDomain:
  userCookiesDuration: 
  userCookiesSameSite: 
  userCookiesSecure: 
  userCrossCookiesDomain: 
```

<br> 

# 📱 Android Sample App

To run the Android sample app for testing the SDK, follow these steps:

1. You will need a **GIT token with read permissions** to access the project.
2. In Android Studio, select the appropriate **Build Variant**:
   - `PublisherDebug` or `BrandsDebug`
3. Open the file `build.gradle.kts` and update the following fields:
   - `client_id`
   - `cookies_version`

## Local Testing

### To test a bug fix
1. Clone the `axeptio-android-sdk-sources` repository
2. Switch to the branch you want to test.
3. Configure the widget (see the configuration section below).

### To test the version in production
1. Clone the `axeptio-android-sdk-sources` repository
2. Configure the widget (see configuration section).
3. Update the SDK version in `build.gradle.kts`:
```kotin
implementation("io.axept.android:android-sdk:2.0.6")
```
### ⚙️ Widget Configuration in the Sample App
In `build.gradle.kts`, add the project ID and the version name:
```kotlin
productFlavors {
    create("publishers") {
        dimension = "service"
        buildConfigField("String", "AXEPTIO_CLIENT_ID", "\"67b63ac7d81d22bf09c09e52\"")
        buildConfigField("String", "AXEPTIO_COOKIES_VERSION", "\"tcf-consent-mode\"")
        buildConfigField("String", "AXEPTIO_TARGET_SERVICE", "\"publishers\"")
    }
    create("brands") {
        dimension = "service"
        buildConfigField("String", "AXEPTIO_CLIENT_ID", "\"67f3f816b336596c4a7c741c\"")
        buildConfigField("String", "AXEPTIO_COOKIES_VERSION", "\"demo-en-EU\"")
        buildConfigField("String", "AXEPTIO_TARGET_SERVICE", "\"brands\"")
    }
}
```
In Build Variants, select either `brands` or `publishers` depending on the service you want to test.

### GitHub Authentication for Maven
In `settings.gradle.kts`, add your GitHub username and a personal access token:
```kotlin
maven {
    url = uri("https://maven.pkg.github.com/axeptio/tcf-android-sdk")
    credentials {
        username = "USER" // TODO: GITHUB USERNAME
        password = "TOKEN" // TODO: GITHUB TOKEN
    }
}
```

> 🛠️ **Tip:** Make sure you're using the correct environment configuration (staging or production) when setting the `client_id` and `cookies_version`.

<br> 

# 📱iOS Sample App
These are the steps to test changes in the iOS SDK using cookie configurations created in the production Back Office (BO).

### To test a bug fix
1. Clone the `axeptio-ios-sdk-sources` repository.
2. Switch to the branch you want to test.
3. Configure the widget (see the configuration section).

### To test the version in production
1. Clone the `axeptio-ios-sdk-sources` repository.
2. Configure the widget (see the configuration section).
3. Point to the SDK version you want to test (see below).

#### Swift
Update the following file:
```bash
sampleSwift/sampleSwift.xcodeproj/project.xcworkspace/xcshareddata/swiftpm/Package.resolved
```
**Example**:
```json
{
  "identity" : "axeptio-ios-sdk",
  "kind" : "remoteSourceControl",
  "location" : "https://github.com/axeptio/axeptio-ios-sdk",
  "state" : {
    "revision" : "9d02ecded880cb373ba9629162e561935f8aa8a5",
    "version" : "2.0.2"
  }
}
```
#### Objective-C
Update the following file:
```csharp
sampleObjectiveC/Podfile.lock
```
```makefile
PODS:
  - AxeptioIOSSDK (2.0.2)
```

### Configure widget in sample app
To configure the widget in the sample app (public repo), add the Project ID and cookies version in `AppDelegate.swift`.
Also, select the appropriate flavor: `.brands` or `.publisherTcf`.
```swift
static let targetService: AxeptioService = .brands
    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {
        Axeptio.shared.initialize(targetService: AppDelegate.targetService, 
        	clientId: "5fbfa806a0787d3985c6ee5f", cookiesVersion: "demo-brands")
```
<br>

# 📱Flutter Sample App
### To test a bug fix
1. Clone the `flutter-sdk` repository.
2. Switch to the branch you want to test.
3. Configure the widget in the sample app for either **iOS** or **Android**.

### To test the version in production
- Checkout the master branch.

### Change native SDK version
#### Android 
In `android/build.gradle`, update the dependencies:
```gradle
dependencies {
    implementation("io.axept.android:android-sdk:2.0.4")
}
```
#### iOS
In `ios/axeptio_sdk.podspec`, update the version:
```ruby
Pod::Spec.new do |s|
  s.name             = 'axeptio_sdk'
  s.version          = '2.0.7'
  s.summary          = 'AxeptioSDK for presenting cookies consent to the user'
  s.homepage         = '<https://github.com/axeptio/flutter-sdk>'
  s.license          = { :type => 'MIT', :file => '../LICENSE' }
  s.author           = { 'Axeptio' => 'support@axeptio.eu' }
  s.source           = { :git => "<https://github.com/axeptio/flutter-sdk.git>" }
  s.source_files = 'Classes/**/*'
  s.dependency 'Flutter'
  s.dependency "AxeptioIOSSDK", "2.0.7"
  s.platform = :ios, '15.0'
```

### ⚙️Configure widget in sample app 
To configure the widget, add the project ID and version name in `example/lib/main.dart`:
```dart
  Future<void> initSDK() async {
    try {
      await _axeptioSdkPlugin.initialize(
        AxeptioService.publishers,
        '67b63ac7d81d22bf09c09e52',
        'tcf-consent-mode',
        null,
      );
```
#### Android
In the Android sample app, add your GitHub credentials in example/android/build.gradle:
```gradle
maven {
      url = uri("<https://maven.pkg.github.com/axeptio/axeptio-android-sdk>")
      credentials {
          username = "USER" // TODO: GITHUB USERNAME
          password = "TOKEN" // TODO: GITHUB TOKEN
      }
```
In build variants select `Brands` or `Publisher` depending on which service you want to use.

In `settings.gradle.kts` add your GitHub user and token.
```gradle
  maven {
      url = uri("<https://maven.pkg.github.com/axeptio/tcf-android-sdk>")
      credentials {
          username = "USER" // TODO: GITHUB USERNAME
          password = "TOKEN" // TODO: GITHUB TOKEN
      }
  }
```
