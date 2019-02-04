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

### Architecture

![](img/codeceptjs-backends.svg)


## Tradeoffs

* Relying on a framework
* Dealing with abstractions

---

## Anyway...

---

## We aim for 

* Readability
* Stability
* Speed
* Maintainability <!-- .element: class="fragment" data-fragment-index="1" -->

---

## Maintainability

* Tests are clear and simple
* Developer and QA teams work together
* Everyone is responsible for tests
* Anyone can fix a failing test
* Easy upgrade from legacy tools

---

## Summary

* ATDD for readable/stable/maintainable e2e tests
* CodeceptJS to help 😉 



---

## Questions

* Michael Bodnarchuk @davert
* Author of CodeceptJS: [codecept.io](https://codecept.io)
 
