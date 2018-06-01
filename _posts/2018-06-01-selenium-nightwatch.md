---
layout: post
title: Selenium + Nightwatch
headline: "Automated E2E Testing"
categories: open-source
tags: 
  - testing
  - selenium
  - e2e
  - nightwatch
comments: true
mathjax: null
featured: true
published: true
---

[Selenium](https://www.seleniumhq.org/) is a very useful tool for automating end-to-end tests.  It uses the [W3C WebDriver API](https://www.w3.org/TR/webdriver/) to launch an instance of a browser (such as Chrome, Firefox, or Safari), navigate to a web page, and then run assertions that verify the page is appearing/functioning as you expect.  This blog post will guide you through the setup of [Nightwatch](http://nightwatchjs.org/) and Selenium.  It will also explain how to write a simple test which navigates to the [DuckDuckGo search engine](https://duckduckgo.com/), makes a search, and asserts the results are as expected.

[Full code for this demo can be found on Github](https://github.com/ruddell/selenium-nightwatch-demo)

### Project Dependencies

This demo expects you to have the following tools installed:

- NodeJS
- Java 7+

As mentioned in the intro, we will be using [Nightwatch](http://nightwatchjs.org/) to run our tests.  Nightwatch is very similar to [Protractor](https://www.protractortest.org/), another test runner that has features to specifically help with Angular.  Here's [a blog post](http://www.webdriverjs.com/protractor-vs-webdriverio-vs-nightwatch/) that compares and contrasts Nightwatch, Protractor, and WebDriverIO.  You can also find more comparisons of test runners by searching StackOverflow and Google.

The main reason we are using Nightwatch is so that we can get started quickly writing tests and have access to powerful tools out-of-the-box, such as reporting and screenshots.

A requirement of Nighwatch is to download Selenium, one of the most popular implementations of the WebDriver API.  Note that running the Selenium server requires Java 7+.  We will be using the `selenium-server` node module to prevent any manual download or setup.

### Project Setup

Create a new folder and initialize a node project with:
```bash
npm init
```

Install Nightwatch, Selenium Server, and Chromedriver:
```bash
npm install --save-dev nightwatch selenium-server chromedriver
```

Set the `test` script in `package.json` to launch Nighwatch with: `./node_modules/nightwatch/bin/nightwatch`

### Configuring Nightwatch

For out tests, we need to create a JS file named `nightwatch.conf.js`.  This file contains information such as where tests are located, where to save reports and screenshots, and where to find Selenium.

```javascript
const seleniumServer = require("selenium-server");
const chromedriver = require("chromedriver");

module.exports = {
  "src_folders": [
    "tests"// Where the tests are located
  ],
  "output_folder": "./output/", // reports from nightwatch
  "selenium": { // selenium configuration settings
    "start_process": true, // tells nightwatch to manage the selenium process
    "server_path": seleniumServer.path, // path to selenium
    "log_path" : "./output/",
    "host": "127.0.0.1", // host for selenium
    "port": 4444, // port for selenium
    "cli_args": {
      "webdriver.chrome.driver" : chromedriver.path // pass chromedriver path
    }
  },
  "test_settings": {
    "default": { // default settings (you can override with custom settings)
      "screenshots": {
        "enabled": true, // enables screenshots
        "path": "output/" // output folder for screenshots
      },
      "globals": {
        "waitForConditionTimeout": 5000 // sometimes internet is slow so wait.
      },
      "desiredCapabilities": {
        "browserName": "chrome", // use Chrome as the default browser
        "chromeOptions" : {
         "args" : [ ] // pass custom CLI args to Chrome
       }
      }
    }
  }
}
```

You can find the full list of configuration options [in the Nightwatch documentation](http://nightwatchjs.org/gettingstarted/#settings-file).

### Writing The First Test

Now that we have both Nightwatch installed and configured, we can get started writing tests.  The test we write will do several things:

- Open Chrome
- Navigate to DuckDuckGo.com
- Input a search query in the text box
- Click the search button
- Verify the results returned match expectations
- Take a screenshot at the end of the test

Create a test called `duck-duck-go.js` inside the `tests` folder (configured in the above `nightwatch.conf.js` under `src_folders`).

#### Open a Browser

Start it out basic with just opening the browser and navigating to the website:

```javascript
module.exports = {
    'Demo test DuckDuckGo search' : function (browser) {
      browser
        .url('https://duckduckgo.com/')
        .waitForElementVisible('body', 1000)
        .end();
    }
};
```

- `url` navigates to the specified URL
- `waitForElementVisible` waits for the specified element to be visible
- `end` finishes the test, make sure your other actions are before this line

#### Accessing Input Fields

To access the input fields, first wait for them to be visible.  The `#` in `search_form_input_homepage` means we are looking for an element with the ID matching `search_form_input_homepage`.  Once the element is found, `click` on the field and send some `keys` like in the lines below:

```javascript
    .waitForElementVisible('#search_form_input_homepage', 1000)
    .click('#search_form_input_homepage')
    .keys(`nightwatch js`)
```

#### Clicking Buttons

To click the search button, wait for it to be visible then send it a `click`:

```javascript
    .waitForElementVisible('#search_button_homepage', 1000)
    .click('#search_button_homepage')
```

#### Asserting Page Content

Now that we searched DuckDuckGo for "nightwatch js", we can verify that the result text contains the name of the library.

```javascript
    .assert.containsText('#web_content_wrapper', 'Nightwatch.js')
```

#### Take a Screenshot
Taking a screenshot during the test can help track any visual issues or changes.

```javascript
    .saveScreenshot(`./output/search.png`)
```

#### All Together Now
When we put all of the code from the previous sections together, the test should look like the following:

```javascript
module.exports = {
    'Demo test DuckDuckGo search' : function (browser) {
      browser
        .url('https://duckduckgo.com/')
        .waitForElementVisible('body', 1000)
        .waitForElementVisible('#search_form_input_homepage', 1000)
        .click('#search_form_input_homepage')
        .keys(`nightwatch js`)
        .waitForElementVisible('#search_button_homepage', 1000)
        .click('#search_button_homepage')
        .assert.containsText('#web_content_wrapper', 'Nightwatch.js')
        .saveScreenshot(`./output/search.png`)
        .end();
    }
};
```

You can see what it looks like when running in this YouTube video:

<iframe width="560" height="315" src="https://www.youtube.com/embed/BAdu4OnCCsg" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### Summary

In conclusion, Selenium and Nighwatch offer a simple yet powerful set of tools for end-to-end testing, enabling you to catch issues before your users do.  [Full code for this demo can be found on Github](https://github.com/ruddell/selenium-nightwatch-demo)

For a more advanced tutorial on Nighwatch, including how to run tests on remote browsers though [SauceLabs](https://saucelabs.com/), see [https://github.com/dwyl/learn-nightwatch](https://github.com/dwyl/learn-nightwatch)