## WebDriver - the end of monopoly

* Slow evolution of WebDriver <!-- .element: class="fragment" data-fragment-index="1" -->
* Death of Internet Explorer <!-- .element: class="fragment" data-fragment-index="2" -->
* Alternatve testing solutions <!-- .element: class="fragment" data-fragment-index="3" -->
  * Cypress.io <!-- .element: class="fragment" data-fragment-index="3" -->
  * TestCafe <!-- .element: class="fragment" data-fragment-index="3" -->
  * Puppeteer <!-- .element: class="fragment" data-fragment-index="3" -->

---

## Puppeteer

* started in 2017 
* by Google Chrome Team
* based on CDP protocol

---

## CDP Protocol

* CDP = Chrome DevTools Protocol
* Bi-directional protocol
* Works over websockets

---

![](img/protocol.png)

---

## CDP Implementations

* JavaScript: [Taiko](https://github.com/getgauge/taiko/) by ThoughtWorks
* JavaScript: [Chromeless](https://github.com/prisma-archive/chromeless) (deprecated)
* JavaScript [Chrominator](https://github.com/jesg/chrominator)
* Java: [chrome-devtools-java-client](https://github.com/kklisura/chrome-devtools-java-client)
* Ruby: [Cuprite for Capybara](https://github.com/machinio/cuprite)

...

---

## Puppeteer is a standard implementation of CDP

---

## Puppeteer Features

* is ~3 times faster than WebDriver
* direct connection to browser (no need for ChromeDriver)
* installs Chrome browser for you
* works with Firefox 
* APIs for window/frames/tabs control
* network API
* workers API
* ...

---

## How to start with Puppeteer

* Install Puppeteer & Jest-Puppeteer

```js
const browser = await puppeteer.launch({ headless: true })
const page = await browser.newPage();
await page.setViewport({ width: 1280, height: 800 })
await page.goto('/Painting-Supplies/cat_CL140420/bww15', { waitUntil: 'networkidle2' })
await page.click('button.add-to-cart-btn.addToCart')
await page.waitForSelector('h4.cart-items-header')
let text = await page.evaluate(() => document.body.textContent)
expect(text).toContain('1 item in cart');
await browser.close()
```

---

![](img/puppeteer_api.png)

---

## API Concepts

* Browser - represents main browser window
* Page - actions around webpage
* ElementHandle - represents an element on page

---

# Puppeteer Issues & Solutions

---

## Execution Context was destroyed

Not waiting for page load on click

```js
await page.click('a.navigate-to-page');
const body = await page.$('body'); // error!
```

**Solution #1** wait for page load...

```js
await Promise.all([
  page.click('a.navigate-to-page'),
  page.waitForNavigation(),
]);
```

---

🤔 What if we don't know if navigation is going to happen?

---

## Execution Context was destroyed

**Solution #2** Create a retry-wrapper

```js
const retry = require('promise-retry');

const retryAction = async (action) => {
  return retry(function (retry, number) {       
    return action().catch(function (err) {
      if (err.message.indexOf('context') > -1) retry(err);
    });
  }, {retries: 5});
}

await page.click('a.navigate-to-page');
const body = await retryAction(() => page.$('body'));
```

---

## Element is not ready yet

* **Solution #1**. `waitForSelector` + click/type/...
* **Solution #2**. Use retry wrapper from previous slide 👈
* **Solution #3** Use `jest-puppeteer`

```js
// Will try while 500ms to click on "button"
await page.toClick('button')
```

---

## Assertions

Use `expect-puppeteer` from jest-puppeteer

```js
// make an assertion
await expect(page).toMatchElement('div', { text: 'Success' })
```

---

## Test is executed too fast

**Solution** Add extra timeout after each action

```js
const sleep = ms => new Promise((done) => setTimeout(done, ms));

const waitAction = async (action, sleepOpts) => {
  await sleep(sleepOpts.before || 0);
  const response = await action();
  await sleep(sleepOpts.after || 100); // default action time
  return response;
}

waitAction(() => page.click('a.nav'), { before: 100, after: 500 });
```

---

## Window vs Viewport

**Problem:** viewport does not resize actual window

```js
await page.setViewport({
  width: 1900,
  height: 1000,
});
```

**Solution** resize window on start

```js
const width = 1900;
const height = 1000;

const browser = await puppeteer.launch({args: 
  [`--window-size=${width},${height}`]
});
const page = await browser.newPage();
await page.setViewport({ width, height });

```

---

## Mock Requests

**Solution** Use PollyJS with Puppeteer Adapter

```js
const { server } = polly;

server.host('http://localhost:3000', () => {
  server.get('/favicon.ico').passthrough();
  server.get('/sockjs-node/*').intercept((_, res) => res.sendStatus(200));
});

await page.goto('http://localhost:3000', { waitUntil: 'networkidle0' });

```

---

## Selenoid

**Solution** Use 'webdriverio' to start and stop browser session

```js
const puppeteer = require('puppeteer');
const { remote } = require('webdriverio');

const wd = await remote({
  capabilities: {
    browserName: 'chrome',
    browserVersion: 'latest',
    'selenoid:options': { enableVideo: true, videoName: `video.mp4` },
  }
});
const browser = await puppeteer.connect({ 
  browserWSEndpoint: `ws://localhost:4444/devtools/${wd.sessionId}/` });
// run test & finish
await wd.quit();
```

---


## Cooking Puppeteer

<img src="img/witch_cooking.png" style="float: right; width: 300px;">

* Use assertion library
* Install Jest-Puppeteer (optionally)
* Use retry-wrappers
* Write more wrappers! <!-- .element: class="fragment" data-fragment-index="1" -->


---

# Advanced Features

* code coverage (CSS, JS, source maps)
* performance testing
* device emulation (network emulation)

---

## Cloud Browsers

* Browserless.io
* Aerokube Browsers
* Google Cloud Functions
* ~~SauceLabs~~

---

### Cypress.io vs Puppeteer

| Type | Cypress.io | Puppeteer |   |
| --- | --- | --- | --- |
| Fast | ✔  | ✔ | | 
| Auto-retries | ✔ | ❌ | *see these slides |
| Cross-browser support | ✔⚠  | ✔⚠ | *chromium firefox |
| Events | Emulated | Native | * over/focus/click events may be skipped |
| Mocking | fetch | network control | * full network control over fetch mocking |

---

### Cypress.io vs Puppeteer

| Type | Cypress.io | Puppeteer |
| --- | --- | --- | 
| iFrame | ❌ | ✔ |
| Tabs | ❌ | ✔ |
| Incognito Window | ❌ | ✔ |
| XPath | ❌ | ✔ |
| async/await | ❌ | ✔ |
| Uploads | ❌ | ✔ |
| Downloads | ❌ | ✔ |
| Simple <!-- .element: class="fragment" data-fragment-index="1" --> | ✔ <!-- .element: class="fragment" data-fragment-index="1" --> | ❌ <!-- .element: class="fragment" data-fragment-index="1" --> |
| UI <!-- .element: class="fragment" data-fragment-index="2" --> | ✔ <!-- .element: class="fragment" data-fragment-index="2" --> | ❌ <!-- .element: class="fragment" data-fragment-index="2" --> |

---

![](img/different.jpg)

---

## **Playwright**

* Made by the same team as Puppeteer <!-- .element: class="fragment" data-fragment-index="1" -->
* Same API as Puppeteer <!-- .element: class="fragment" data-fragment-index="2" -->
* Works for Firefox and Chromium... <!-- .element: class="fragment" data-fragment-index="3" -->

... and Safari <!-- .element: class="fragment" data-fragment-index="4" -->

---

## Cross-Browser Testing

|          | ver | Linux | macOS | Win |
|   ---:   | :---: | :---: | :---:  | :---: |
| Chromium| 81.0.4044 | ✔ | ✔ | ✔ |
| WebKit | 13.0.4 | ✔ | ✔ | ✔ |
| Firefox |73.0b3 | ✔ | ✔ | ✔ |

* Headless is supported for all the browsers on all platforms.
* Chromium == Microsoft Edge (new)
* Webkit == Safari

---

## Differences with Puppeteer

* Focuses on test automation
  * auto wait for element appeared
  * custom strategies from core
* Includes patched Firefox & WebKit
* Has XPath & custom selector support

---

## Migrate to Playwright Today

⚠ Not recommended until Playwright hits 1.0

[Migrating from Puppeteer to Playwright](https://medium.com/@davert/puppeteer-to-playwright-migration-guide-6c86ea66e85e?source=friends_link&sk=e3ec4d78e3f51114dadcb7aabde82451)

---


## Not sure what to choose? 

* Is Puppeteer stll a thing? 🤔 <!-- .element: class="fragment" data-fragment-index="1" -->
* Is this a good time to migrate to Playwright? ⏲ <!-- .element: class="fragment" data-fragment-index="2" -->
* Selenium works. Don't touch it! 👷 <!-- .element: class="fragment" data-fragment-index="3" -->
* Only Cypress!!!!111111 We 💖 it!1111 <!-- .element: class="fragment" data-fragment-index="4" -->

