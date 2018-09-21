# Effective Tests

---

## Qualities of Tests

* Readability <!-- .element: class="fragment" data-fragment-index="1" -->
* Stability <!-- .element: class="fragment" data-fragment-index="2" -->
  * to changes <!-- .element: class="fragment" data-fragment-index="3" -->
  * to execution <!-- .element: class="fragment" data-fragment-index="4" -->
* Speed <!-- .element: class="fragment" data-fragment-index="5" -->

---

## ATDD

* Acceptance
* Test 
* Driven
* Development

---

![](img/atdd.png)

---

## ATDD vs BDD

* BDD is focused on **business scenarios** <!-- .element: class="fragment" data-fragment-index="1" -->
* BDD abstracts from interface <!-- .element: class="fragment" data-fragment-index="2" -->
* BDD requires implementation for each step <!-- .element: class="fragment" data-fragment-index="3" -->


---

## E2E Test Before Code

* to have a scope for the feature <!-- .element: class="fragment" data-fragment-index="1" -->
* to plan interaction scenario <!-- .element: class="fragment" data-fragment-index="2" -->
* to think about interface <!-- .element: class="fragment" data-fragment-index="3" -->

---

## Can we predict future?

```js
I open page '/checkout'
I fill field 'First Name', 'Michael'
I fill field 'Last Name', 'Bodnarchuk'
I select option 'shipment', 'local collection'
I click 'Next'
I click 'Pay with Card'
I fill field 'Card number', '12334556789'
```

---

## Is this a test?

# YES!

(a failing test)

---

## How To Keep It Readable

* Keep the same code <!-- .element: class="fragment" data-fragment-index="1" -->
* Focus around one feature <!-- .element: class="fragment" data-fragment-index="2" -->
* Split long scenarios <!-- .element: class="fragment" data-fragment-index="3" -->
* Keep semantic locators <!-- .element: class="fragment" data-fragment-index="4" -->
* Hide implementation details <!-- .element: class="fragment" data-fragment-index="5" -->

---

## How to Keep it Stable

* To changes
  * Pick stable parts of application <!-- .element: class="fragment" data-fragment-index="1" -->
  * Use element captions/ids/texts <!-- .element: class="fragment" data-fragment-index="2" -->
* To execution
  * Wait for changes on page <!-- .element: class="fragment" data-fragment-index="3" -->
  * Retry failed steps <!-- .element: class="fragment" data-fragment-index="4" -->

---

## How to Locate Elements

* CSS *(most common)*
* XPath *(most powerful)*
* Button | Link Texts
* Field Names *(most stable)*
* Field Labels *(most readable)*

---

## Locators

* Short
* Semantic
* Flexible on position

---

## ATDD Bonuses

* We focus on a persona, not on web elements <!-- .element: class="fragment" data-fragment-index="1" -->
* We start with a simplest locators <!-- .element: class="fragment" data-fragment-index="2" -->
* We start with business critical scenarios <!-- .element: class="fragment" data-fragment-index="3" -->
* We get an effective test <!-- .element: class="fragment" data-fragment-index="4" -->
* We never go untested <!-- .element: class="fragment" data-fragment-index="5" -->
