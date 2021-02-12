---
layout: post
title: JHipster React Native Demo
headline: "Generate, Build, and Deploy React Native with JHipster"
categories: open-source
tags:
  - jhipster
  - react-native
  - open-source
  - expo
comments: true
mathjax: null
featured: true
published: true
---

[JHipster React Native](https://github.com/ruddell/generator-jhipster-react-native) is now available as a [JHipster Blueprint](https://www.jhipster.tech/modules/creating-a-blueprint/).  Previously known as `ignite-jhipster`, the latest version brings many changes and improvements to the developer experience.  With full Expo support (including Web and PWAs), building and deploying the application is easier than ever. For a full changelog, [see the Github release](https://github.com/ruddell/generator-jhipster-react-native/releases/tag/v4.0.0).

In this demo, we will generate a React Native application. Next, we will launch the React Native app locally on iOS, Android, and Web. To finish up, we will build and deploy the applications, and demonstrate over-the-air updates.



Generate the React Native Client


1. [Environment Setup](#environment-setup)
2. [Generating the React Native Client](#generating-the-react-native-client)
3. [App Config](#app-config)
4. [Running the App](#running-the-app)
5. [Building and Deploying](#building-and-deploying)
6. [Updating Over the Air](#updating-over-the-air)


## Environment Setup <a name="environment-setup"></a>

Make sure to have an LTS version of NodeJS, then install the JHipster React Native Blueprint and Expo CLI:
```
npm i -g generator-jhipster-react-native expo-cli
```

Also make sure to register an Expo account at https://expo.io/signup if you do not already have one.

In this demo, we will be using the `rnhipster` command, which functions similarly to `jhipster --blueprints react-native`. The key difference is `rnhipster` uses its own version of JHipster rather than the globally installed version, preventing version mismatches. This allows developers with older JHipster versions such as v6 to use the latest blueprint improvements in JHipster v7.

## Generate the React Native Client <a name="generating-the-react-native-client"></a>

Because the React Native blueprint is designed to communicate with a JHipster backend, we'll use an existing application.  We want to use the entities and config from that application to generate our React Native app.

The repository for the demo backend is located on Github at [ruddell/jhipster-sample-app-react-native-backend](https://github.com/ruddell/jhipster-sample-app-react-native-backend.git) and on Heroku at https://jh-sample-app-react-native-api.herokuapp.com/.  To get the config and entities, we will be using [JDL]((https://www.jhipster.tech/jdl/)), a JHipster-specific domain language to describe applications and entities.  The backend JDL can be [viewed here](https://github.com/ruddell/jhipster-sample-app-react-native-backend/blob/main/jhipster.jdl).

Make a new directory for generating the client and import the JDL.

```bash
mkdir ../client && cd ../client
rnhipster jdl https://raw.githubusercontent.com/ruddell/jhipster-sample-app-react-native-backend/main/jhipster.jdl
```
Enter a name for the application, enable E2E Detox Tests if desired, and wait for `npm install` to finish.

**Tip:** If you prefer not to use JDL, run `rnjhipster` to use the prompts.  Specify the local path to the backend folder and pass the `--with-entities` flag to also generate the backend entities.

![JHipster React Native Banner](https://dev-to-uploads.s3.amazonaws.com/i/qi624ofse2uzq3irnt3d.png)

## App Config <a name="app-config"></a>

The API URL is configured in `app/config/app-config.js`.  When deploying the app, whether to an App Store or a CDN, it is required to set the `apiUrl` to a deployed URL of the backend.

For this demo, set the `apiUrl` to `https://jh-sample-app-react-native-api.herokuapp.com/`.

**Tip**: If you are using OAuth 2.0 for the authentication type, [see the docs for a guide to Okta and Keycloak configuration.](https://github.com/ruddell/generator-jhipster-react-native/blob/main/docs/oauth2-oidc.md)

## Running the App <a name="running-the-app"></a>

To start the app, run `npm start` in the `client` directory.

There are multiple ways to launch the application directly on a specific platform:

{:class="table table-bordered"}
| **Running On**       | **Command**                       |
|:---------------------|:----------------------------------|
| On Device            | Scan the QR Code with your device |
| Web Browser          | `npm start -- --web`              |
| iOS Simulator        | `npm start -- --ios`              |
| Android Simulator    | `npm start -- --android`          |

Once the packager is started, you can also launch other platforms.  Press `w` for web, `i` for iOS, or `a` for Android.

For iOS and Android Simulator setup, follow the platform-specific instructions for [Android](https://docs.expo.io/workflow/android-studio-emulator/) and [iOS](https://docs.expo.io/workflow/ios-simulator/).

![Logged In to the Apps](https://dev-to-uploads.s3.amazonaws.com/i/935edefrpnz8ki0h4cte.png)

## Building and Deploying <a name="building-and-deploying"></a>

### Building Web

Run `npm run build:web` to build a production deployment of the web client in `web-build`.

**Tip:** If you want to enable the PWA, set `offline: true` in `webpack.config.js`.

### Deploying Web

Once the web client is built, you can preview it with `npx serve web-build`.  If everything looks good, upload the `web-build` folder to a static site host or CDN of your choice.

### Building iOS and Android

You will only need to submit a new build of the app to the App Store for review when you update the Expo SDK in your project.  Otherwise no native code changes, so the Javascript bundle can be updated over-the-air without going through the review processes again.

iOS and Android apps are built through the `expo build` command.  The actual compilation of the app happens in the cloud on Expo's build servers, then the packaged application is available for download.  The build process does take several minutes, but you rarely need to update the Expo SDK.

For more information on `expo build`, see the Expo docs on [Building Standalone Apps](https://docs.expo.io/distribution/building-standalone-apps/).

#### iOS

To build your iOS app, run `npm run build:ios`. Choose an iOS bundle identifier, then choose either `archive` or `simulator`.

An Apple App Store developer account is required for the `archive` option, which is later uploaded to your iOS App Store builds.  You can start a build for the Simulator without a developer account.

#### Android

To build your iOS app, run `npm run build:ios`. Choose an Android package name, then choose either `apk` or `aab`.

An `apk` builds an application that you can directly install on any device.  An `aab` builds an optimized build of your app for deployment through the Google Play Store.

### Deploying iOS and Android

You can deploy your apps built in the previous step to the App Stores with `expo upload:ios` and `expo upload:android`. You will need a developer account for both App Stores to submit an app for listing.

For information on `expo upload`, see the Expo docs on [Uploading Apps to the Apple App Store and Google Play](https://docs.expo.io/distribution/uploading-apps/).

## Updating Over the Air <a name="updating-over-the-air"></a>

Once your apps have been deployed to the App Stores, you can release new updates with the `expo publish` command.  The next time a user loads the application, the latest client will download and prepare itself.  On the second launch, the new client is displayed with any updates.

Web updates are not currently supported, so you will still need to deploy those changes manually.

For information on OTA Updates, see the Expo docs on [Configuring OTA Updates](https://docs.expo.io/guides/configuring-ota-updates/).

## Summary

Any questions, comments, improvements, issues, and pull requests are welcome over at the [JHipster React Native Github page](https://github.com/ruddell/generator-jhipster-react-native/).