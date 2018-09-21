# CodeceptJS

![](https://codecept.io/images/cjs-base.png)

[codecept.io](https://codecept.io)

---

## CodeceptJS

* **end to end** testing framework
* helpers for popular testing backend
* high-level **unified APIs** for all backends
* **~15K** week installations 

[codecept.io](https://codecept.io)

---

## Purpose of CodeceptJS

* Ideas taken from [Codeception](http://codeception.com)
* High-level BDD-style language
* Run a single test over multiple backends
* Don't worry about asychronity

---

### Sample Scenario

```js
Scenario('todomvc', (I) => {
  I.amOnPage('http://todomvc.com/examples/react/');
  I.waitForElement('.new-todo');
  I.dontSeeElement('.todo-count');
  I.fillField('What needs to be done?', 'Write a guide');
  I.pressKey('Enter');
  I.see('Write a guide', '.todo-list');
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

![](img/todo.gif)

---

### Goals

* Focus on scenario not on implementation
* Easy to read and write
* Separate test code from support code

---

#### Reability & PageObjects 

```js
Scenario('post article', async (I, loginPage) => {  
  const user = await I.createUser('davert');
  loginPage.login(davert);
  // ..
  I.see('User logged in', loginPage.messageBox);
})

```

---

#### Write test in your language

```js
Scenario('пробую написать реферат', (Я) => {
    Я.на_странице('http://yandex.ru/referats');
    Я.вижу("Написать реферат по");
    Я.выбираю_опцию('Психологии');
    Я.кликаю("Написать реферат");
    Я.вижу("Реферат по психологии");
});
```

---

## Readability bonus

We don't define how the test is executed!

---

### Architecture

![](img/codeceptjs-backends.svg)

---

## Backends & Dependencies

* WebDriverIO => 
  * `webdriverio` package
  * Selenium Server
  * ChromeDriver or GeckoDriver
* Protractor
  * `protractor` package
  * ChromeDriver
* Puppeteer
  * `puppeteer` package
* Nightmare
  * `nightmare` package

---

<video onclick="this.paused ? this.play() : this.pause();" src="/img/codeceptjs_demo.mp4"  controls=""></video>


---

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
 
