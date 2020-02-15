<img src="img/codeceptjs.png" style="float: right; width: 300px;">

## CodeceptJS

* Multi-driver testing framework
  * Webdriverio
  * Puppeteer
  * Playwright
  * TestCafe
  * Protractor  
* Custom runner, mocha-based test framework 
* High level API (with Cucumber support)
* Interactive debug mode

---

## Same tests for different engines

![](img/codecept_ci.png)

From TravisCI for CodeceptJS project 👆

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

## Puppeteer-Playwright Features

* Assertions
* Auto retry on failed contexts
* RetryFailedStep plugin
* AutoDelay plugin

---

### CodeceptUI

![](img/codecetui.gif)

---

### Writing Test in CodeceptUI

![](img/new-test.gif)

---

## Conclusion

* Puppeteer is a modern alternative to WebDriver <!-- .element: class="fragment" data-fragment-index="1" -->
* Playwright extends Puppeteer to all major browsers <!-- .element: class="fragment" data-fragment-index="2" -->
* CodeceptJS unites all browser engines with a framework on top of it <!-- .element: class="fragment" data-fragment-index="3" -->

---

## Questions

* Michael Bodnarchuk @davert
* Author of CodeceptJS: [codecept.io](https://codecept.io)
 
