## WebDriver - the end of monopoly

* Slow evolution of WebDriver
* Alternatve testing solutions
  * Cypress.io
  * TestCafe
  * Puppeteer
* Death of Internet Explorer

---

## Puppeteer

* started in 2017 by Google Chrome 
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
* APIs for:
  * Codecoverage
  * Browser emulation
  * Networking

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

## Puppeteer Issues & Solutions

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

# Advanced Features

---

## CodeCoverage

---

## Network

---

### Cypress.io vs Puppeteer

---

## Cloud Browsers

* Browserless.io
* Aerokube Browsers
* Google Cloud Functions
* ~~SauceLabs~~

---

