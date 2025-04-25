
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

> 🛠️ **Tip:** Make sure you're using the correct environment configuration (staging or production) when setting the `client_id` and `cookies_version`.

