### So you want some browser test automation?

![](img/psst.jpg)

---

# JavaScript is the future
### For test automation <!-- .element: class="fragment" data-fragment-index="1" -->

---

## JavaScript!

* Simple language*
* Rich ecosystem <!-- .element: class="fragment" data-fragment-index="1" -->
* Not limited to Selenium <!-- .element: class="fragment" data-fragment-index="2" -->
* In browser execution <!-- .element: class="fragment" data-fragment-index="3" -->

---

#### *JavaScript is simple except

# Promises <!-- .element: class="fragment" data-fragment-index="1" -->

---

<img src="img/snowman.jpg" style="float: right; with: 50%; padding-bottom: -20px;">

### Let's Make a Snowman

1. Testing Framework
2. Assertion library
3. Browser Driver
4. Runner

---

## Testing Framework 

*How tests are written*

- **mocha**
- jasmine
- cucumber
- jest
- ava

---

> Testing frameworks are pretty much the same for e2e 

---

## Assertion Libraries

*How to make assertions*

- chai
- framework-relevant
- native assert

---

> In NodeJS assertion libraries are decoupled from a testing framework. 

---

## Browser Driver

* Protractor
* webdriverio
* Cypress.io
* Puppeteer
* TestCafe
* NightwatchJS
* NightmareJS

---

## Runner

*CLI tool to execute tests*

* mocha
* protractor
* wdio
* cypress
* testcafe

---

## In Details

---

## Browser Control

Internal | External

![](img/webinterface.png)

---

<img src="img/angular-protractor.png" style="float: right;">

### Protractor

- Selenium (built on top of official library)
- Good Documentation
- Jasmine Testing Framework
- Angular support
- Java-like syntax
- Ooooooutdated
- Protractor 6 will break everything

---

### Example

```js
beforeEach(function() {
  browser.get('http://www.angularjs.org');
  todoList = element.all(by.repeater('todo in todoList.todos'));
});

it('should add a todo', function() {
  var addTodo = element(by.model('todoList.todoText'));
  var addButton = element(by.css('[value="add"]'));

  addTodo.sendKeys('write a protractor test');
  addButton.click();

  expect(todoList.count()).toEqual(3);
  expect(todoList.get(2).getText()).toEqual('write a protractor test');
});
```

---

<img src="img/nightwatch.svg" style="float: right; width: 300px;">

### Nightwatch

* Mastodon of web testing in JS
* Selenium (JSON-Wire) based
* Everyone used it before 
* Everyone tries to migrate from it...
* But they have fancy logo 

---

```js
this.demoTestGoogle = function (browser) {
  browser
    .useXpath() // every selector now must be xpath
    .click("//tr[@data-recordid]/span[text()='Search Text']")
    .useCss() // we're back to CSS now
    .setValue('input[type=text]', 'nightwatch')
  browser.expect
    .element('body')
    .to.have.attribute('class')
    .which.contains('found-item');
};
```

---

<img src="img/webdriverio.png" style="float: right; width: 300px;">

### WebdriverIO

- Alternative Selenium implementation 
- Mobile testing (Native Apps) with Appium
- Awesome documentation 
- v4 to v5 upgrade... 
- W3C spec + JSONWire spec 
- Standalone / Jasmine / Mocha / Cucumber integration

---

### Example

```js
// page object
class FormPage extends Page {
    get username () { return $('#username') }
    get password () { return $('#password') }
}
// test
browser.url('/form');
FormPage.username.setValue('foo')
FormPage.password.setValue('bar')
FormPage.submit()
```

---

<img src="img/cypress.png" style="float: right; width: 300px;">

### Cypress.io

* Chrome-based, runs inside a browser   
* Mocha testing framework + chai assertions
* UI Debugger
* Good documentation 
* Auto retry failed steps 
* [LIMITATIONS!!!](https://docs.cypress.io/guides/references/trade-offs.html)
  * No XPath 
  * No file uploads 
  * No multiple browsers, multiple tabs 
  * No iframes 

---

### Example

```js
it('adds 2 todos', function () {
  cy.visit('/');
  cy.get('.new-todo')
    .type('learn testing{enter}')
    .type('be cool{enter}')
  cy.get('.todo-list li').should('have.length', 2)
})
```

---

<img src="img/cypress/Selection_420.png" style="float: left; height: 30%; width: 33%;">
<img src="img/cypress/Selection_421.png" style="float: left; height: 30%; width: 33%;">
<img src="img/cypress/Selection_422.png" style="float: left; height: 30%; width: 33%;">
<img src="img/cypress/Selection_423.png" style="float: left; height: 30%; width: 33%;">
<img src="img/cypress/Selection_424.png" style="float: left; height: 30%; width: 33%;">
<img src="img/cypress/Selection_425.png" style="float: left; height: 30%; width: 33%;">
<img src="img/cypress/Selection_426.png" style="float: left; height: 30%; width: 33%;">
<img src="img/cypress/Selection_427.png" style="float: left; height: 30%; width: 33%;">
<img src="img/cypress/Selection_428.png" style="float: left; height: 30%; width: 33%;">

---

#### How much does your test automation framework cost?

![](img/cypress/Selection_429.png)

---

### Rise of Cypress.io

![](img/cypress/Selection_430.png)

---

> Cypress.io is over-estimated as end 2 end testing framework

---

<img src="img/puppeteer.png" style="float: right; width: 300px;">

### Puppeteer

* Official Google Chrome DevTools library
* Standalone library (no testing framework)
* Good API Documentation
* Provides full browser control
* Headers, Mock requests, Responses
* Multi tab control, incognito mode
* Iframes, file upload...

---

### Example

```js

beforeEach(async () => {
    const page = await browser.newPage()
    await page.setViewport({ width: 1280, height: 800 })
    await page.goto('https://www.walmart.com/ip/Super-Mario-Odyssey-Nintendo-Switch/56011600', { waitUntil: 'networkidle2' })
    await page.click('button.prod-ProductCTA--primary')
    await page.waitForSelector('.Cart-PACModal-ItemInfoContainer')
    await page.screenshot({path: screenshot})
    await browser.close()  
});
```

---

### Rise of Puppeteer

![](img/cypress/Selection_431.png)

---

<img src="img/testcafe.png" style="float: right; width: 300px;">

### TestCafe

* Cross-browser client-side testing
* Proxy server for mocking all requests
* Doesn't control browser
* Custom test framework, assertions, runner
* Parallel execution built-in
* Multi-browser setup

---

## Example

```js
test('Dealing with text using keyboard', async t => {
  await t
    .typeText(page.nameInput, 'Peter Parker') // Type name
    .click(page.nameInput, { caretPos: 5 }) // Move caret position
    .pressKey('backspace') // Erase a character
    .expect(page.nameInput.value).eql('Pete Parker') // Check result
    .pressKey('home right . delete delete delete') // Pick even shorter form for name
    .expect(page.nameInput.value).eql('P. Parker'); // Check result
});
```

---

## How to choose driver

* Learn how the tool works
* Consider its limitations
* Check documentation
* Upgrade strategy
* Look into source code

---

## Asynchronity

*In JavaScript all browser commands are **promises***

|Driver   | Strategy  |
|:--|:--|
| Protractor 5 | control flow |
| Protractor 6 | async/await |
| webdriverio | sync fibers, async/await |
| cypress | control flow |
| Puppeteer | async/await |
| TestCafe | async/await |

---

## How to choose testing framework

* Use the one provided by driver
* Except...

---

<img src="img/cucumber.png" style="float: right; width: 300px;">

## ...Cucumber

* Has its own runner & testing framework
* To hide JS complexity
* To work as BDD tool
* Supported by standalone libraries
  * Protractor
  * WebdriverIO
  * Puppeteer

---

### Example

```gherkin
Feature: Visit the app dashboard
  As a citizen
  I should be able to log in to the app with DigiD
  In order to access my personal information

  Scenario: Log in with DigiD
    Given I am logged in with DigiD as 123456789
    And there are the following toggles: personal
    When I visit the dashboard
    Then I should be greeted with H.A. Janssen
```

---


# Conclusions

* Get your requirements
* Learn ecosystem
* Choose the testing stack

---

## Let's Build Perfect Testing Framework!

<img src="img/snowman.jpg">

---


### We Did It!

<img src="img/shitman.jpg" style="">

...but not that snowman!

---

## Whatever you choose, you lose!

* New testing frameworks emerge <!-- .element: class="fragment" data-fragment-index="1" -->
* Cool fancy library will be legacy tomorow <!-- .element: class="fragment" data-fragment-index="2" -->
* You hit issues with edge cases <!-- .element: class="fragment" data-fragment-index="3" -->
* Different API <!-- .element: class="fragment" data-fragment-index="4" -->
* You can't migrate your code <!-- .element: class="fragment" data-fragment-index="5" -->

---

## Survive The Change

* Write high level test code
* Separate scenario from browser control
* Use Cucumber or **CodeceptJS**

---

<img src="img/codeceptjs.png" style="float: right; width: 300px;">

## CodeceptJS

* Multi-driver testing framework
  * webdriverio
  * Puppeteer
  * Protractor  
* Custom runner, mocha-based test framework 
* High level API (with Cucumber support)
* Interactive debug mode
* Auto retry failed steps

---

### Architecture

![](img/codeceptjs-backends.svg)

---

### Example

```js
Scenario('todomvc', (I, loginPage) => {
  const user = await I.createUser('davert');
  loginPage.login(davert);
  I.see('davert', 'nav');
  I.click('Create Todo');
  I.see('1 item left', '.todo-count');
  I.fillField('What needs to be done?', 'Write a test');
  I.pressKey('Enter');
  I.see('Write a test', '.todo-list');
  I.see('2 items left', '.todo-count');
  I.fillField('What needs to be done?', 'Write a code');
  I.pressKey('Enter');
  I.see('Write a code', '.todo-list');
  I.see('3 items left', '.todo-count');
});
```

---

### Live Development

```js
I.amOnPage('/');
pause();
```

* Call `pause()` to interrupt the test
* Use interactive shell to try different commands
* Copy successful commands into a test

---


## Conclusions

* For advanced e2e testing use **webdriverio**
* For full browser control use **puppeteer**
* For high-level automated e2e tests use **codeceptjs**
* For component testing use **cypress.io**
* For simple multi-browser testing use **testcafe**
* For BDD use CucumberJS

---

## Thank You!

Michael Bodnarchuk **@davert**

* Web developer from Kyiv, Ukraine
* Author of **CodeceptJS** testing frameworks
* Consultant @ [SDCLabs](http://sdclabs.com)



