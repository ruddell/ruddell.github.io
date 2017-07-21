---
layout: post
title: Ignite JHipster
headline: "Ignite JHipster"
categories: open-source
tags: 
  - jhipster
  - ignite
  - react-native
  - open-source
comments: true
mathjax: null
featured: true
published: true
---

[Ignite JHipster](https://github.com/ruddell/ignite-jhipster/) is a new boilerplate for Infinite Red's [Ignite CLI](https://infinite.red/ignite), a React Native app generator.  It enhances the [JHipster](https://jhipster.github.io/) developer experience by generating a full React Native application compatible with their JHipster backend.  The React Native app comes complete with user authentication and an entity generator, jumpstarting the mobile app process.

<div style="text-align:center"><img src="{{ site.url }}/images/ignite-jhipster/logo.png" /></div>

For those unfamiliar, JHipster is an application generator that creates a complete and modern Web app, unifying Spring Boot and Spring Cloud Netflix on the Java side, and Angular and Webpack on the front-end. JHipster also handles all of the database creation, cache creation, comes Docker-ready, and more.

Ignite CLI is a generator for React Native with boilerplates, plugins, and more. While in the past, you had one choice -- Infinite Red's boilerplate -- now you can choose from many boilerplates and also add standalone plugins as you need them.  Ignite JHipster is one of those boilerplates, along with several other options.

### Demo Overview

**Note: This boilerplate is still in beta until the entity generator is complete - please use with caution and provide feedback.**

This demo will cover the process of using **Ignite JHipster** from top-to-bottom.  This includes the following sections:

 - [Environment Setup](#env)
 - [Backend Generation (JHipster)](#backend)
 - [Frontend Generation (Ignite JHipster)](#frontend)
 - [App Config](#config)
 - [Running the App](#running)
 - [Adding an Entity](#entity)
 - [Summary](#summary)

### <a name="env"></a>Environment Setup

#### Installing JHipster

Full instructions for installing JHipster can be found in [the JHipster docs](https://jhipster.github.io/installation/).

#### Installing Ignite

Ignite requires NodeJS v7+

`npm install -g ignite-cli`

#### React Native Setup

React Native requires Watchman to be installed.  Follow their [installation guide](https://facebook.github.io/watchman/docs/install.html) for full instructions.  It also requires the react-native-cli, installed with:

`npm install -g react-native-cli`

Depending on which OS you are using, you will need to set up an emulator.  The iOS Simulator is included in Xcode, only available on macOS.  The Android simulator works alongside [Android Studio](https://developer.android.com/studio/run/managing-avds.html) or [Genymotion](https://www.genymotion.com/).

### <a name="backend"></a>Backend Generation (JHipster)

First of all, we need a JHipster application to serve as the backend for the React Native app.  We'll keep this simple by using one of the default apps.  The only important choice when generating the JHipster app is the authentication.  Ignite JHipster supports all auth types *except session auth*, as that uses Cookies.  This list includes JWT, UAA, and OAuth2.

To keep this demo simple, we will be using one of the sample generated apps, [jhipster-sample-app-ng2](https://github.com/jhipster/jhipster-sample-app-ng2). This app uses JWT, the default authentication type for JHipster apps.

After cloning the project, run `yarn install` or `npm install`, then `./mvnw` to start the Spring Boot backend.  It will be available at [http://localhost:8080](http://localhost:8080).

<figure>
	<a href="{{ site.url }}/images/ignite-jhipster/jhipster-backend.jpg"><img style="max-width: 85%" src="{{ site.url }}/images/ignite-jhipster/jhipster-backend.jpg"></a>
	<figcaption>JHipster app running on port 8080</figcaption>
</figure>

### <a name="frontend"></a>React Native App Generation (Ignite JHipster)

Once the JHipster app is running, we can start work on the React Native frontend.

To generate a new Ignite app using the Ignite JHipster boilerplate, run the following command:

`ignite new IgniteJHipsterSampleApp --boilerplate ignite-jhipster`

This will fetch the latest version of the boilerplate from NPM and generate the application from that code.

To start the emulator and run the app on the emulator, run either of the following commands.

**iOS:**

`react-native run-ios`

**Android:**

`react-native run-android`

<figure>
	<a href="{{ site.url }}/images/ignite-jhipster/app-home.png"><img src="{{ site.url }}/images/ignite-jhipster/app-home.png"></a>
	<figcaption>JHipster Ignite app running on emulator</figcaption>
</figure>

### <a name="config"></a>Configuring Ignite JHipster

The important configuration variables are located in the AppConfig.js file.

For all applications, customize:
 - `apiUrl` (For this demo, use [http://localhost:8080/](http://localhost:8080/))

For Oauth2/UAA applications, customize:
 - `oauthClientId`
 - `oauthClientSecret`

For UAA applications, customize:
 - `uaaBaseUrl`

<figure>
	<a href="{{ site.url }}/images/ignite-jhipster/app-config.png"><img src="{{ site.url }}/images/ignite-jhipster/app-config.png"></a>
	<figcaption>JHipster Ignite AppConfig.js</figcaption>
</figure>

### <a name="running"></a>Running the App

Once configured, the application should connect to the JHipster backend and all screens should work as expected.  The generated screens include register, login, logout, settings, forgot password, and reset password.

<figure>
	<a href="{{ site.url }}/images/ignite-jhipster/screens.png"><img src="{{ site.url }}/images/ignite-jhipster/screens.png"></a>
	<figcaption>Logged in menu</figcaption>
</figure>

### <a name="entity"></a>Adding an Entity

**Note**: The entity generator currently only generates the redux, saga, and API.  Ignite JHipster v1 will generate the full entity UI

Similar to the regular JHipster generator, Ignite JHipster offers an entity generator to quickly scaffold the required React Native code.  The entity must exist in the JHipster app before it can be generated in the React Native app.  For this demo, we will use the pre-existing `BankAccount` entity from the sample app.

`ignite generate entity BankAccount`

Running that command will prompt the developer to enter the path to their JHipster directory.  Once entered, Ignite JHipster copies the entity config file and generates the redux, saga, API, and UI code for retreiving and viewing entities with the app.  It also wires these files together so the requests work out-of-the-box.

### <a name="summary"></a>Summary

In summary, developers can now have a full JHipster website, backend, and mobile app - within minutes rather than weeks.  The time saved can then be spent on adding the important business code rather than setting up an environment and configuring dependencies.

Any questions, comments, improvements, issues, and pull requests are welcome over at the [Ignite JHipster Github page](https://github.com/ruddell/ignite-jhipster/).