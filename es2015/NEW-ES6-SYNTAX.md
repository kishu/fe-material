New ES6 Syntax
===

---

## New ES6 Syntax

`let`, `const` and Block Scoping
Arrow Functions `=>`
Default Function Parameters
Rest and Speread `...`
Object Literal Extensions
`for ... of` loops
Octal and Binary Literals
Template Literals
Destructuring

---

`let`, `const` and Block Scoping
===

---

# let - temporal dead zone
[temporal dead zone](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let)
초기화 하기 전에 액세스 하면 Reference Error 발생

``` js
console.log(productId);      // undefined
var productId = 12;
```

``` js
console.log(productId);      // ReferenceError
let productId = 12;
```

``` js
let productId = 12;
console.log(productId);      // 12
```

``` js
let productId;
console.log(productId);      // undefined
```

---

# let - block scope 
`let`으로 선언한 변수는 블록 범위이며 현재 블록에만 존재

``` js
let produceId = 12;
{
  let productId = 2000;
}
console.log(productId)      // 12
```

``` js
{
  let productId = 2000;
}
console.log(productId);    // ReferenceError
```

---

``` js
let productId = 42;

for (let productId = 0; productId < 10; productId++) {

}

console.log(productId);      // 42
```

----

``` js
let updateFunctions = [];

for (var i = 0; i < 2; i++) {
  updateFunctions.push(function() { 
    return i;
  });
}

console.log(udpateFunction[0]());      // 2
```

---

``` js
let updateFunctions = [];

for (let  i = 0; i < 2; i++) {
  updateFunctions.push(function() { 
    return i;
  });
}

console.log(udpateFunction[0]());      // 0
```

---

# const

`const` 변수는 나중에 바꿀 수없는 값으로 즉시 초기화 함

``` js
const MARKUP_PCT = 100;
console.log(MARKUP_PCT);    // 100
```

``` js
const MARKUP_PCT;
console.log(MARKUP_PCT);    // SyntaxError
```

``` js
const MARKUP_PCT = 100;
MARKUP_PCT = 10;
console.log(MARKUP_PCT);   // TypeError
                           // Assignment to constant variable.
```

---

``` js
const MARKUP_PCT = 100;

if (MARKUP_PCT > 0) {
  const MARKUP_PCT = 10;
}

console.log(MARKUP_PCT);    // 100
                            // block scope
```

---

# 변수 선언 방법

![](https://pbs.twimg.com/media/B9-Pt5lIgAAXLOm.png:large)

---

Arrow Function `=>`
===

---

# syntax

``` js
let getPrice = function() {
  return 5.59;
}
```

to Arrow Function

``` js
let getPrice = () => 5.99;

console.log(typeof getPrice);    // function
console.log(getPrice());         // 5.99
```


``` js
let getPrice = ()
    => 5.99;
console.log(typeof getPrice);    // SyntaxError
```

---

# argument

``` js
let getPrice = count => count * 4.00;
console.log(getPrice(2));        // 8
```

``` js
let getPrice = (count, tax) => count * 4.00 * (1 + tax);
console.log(getPrice(2, .07);    // 8.56 
```

---

# block

``` js
let getPrice = (count, tax) => {
  let price = count * 4.00;
  price *= (1 + tax);
  return price;
}

console.log(getPrice(2, .07);    // 8.56
```

---

# this context

``` js
document.addEventListener("click", function() {
  console.log(this);    // #document
});
```

``` js
document.addEventListener("click", () => console.log(this));
// Window { ... }
```

---

``` js
let invoice = {
  number: 123,
  process: function() {
    console.log(this);
  }
};

invocie.process();    // Object { number: 123, ... } 
```

---

``` js
let invoice = {
  number: 123,
  process: () => console.log(this);
};

invocie.process();    // Window { ... }
```

---

``` js
let invoice = {
  number: 123,
  process: function() {
    return () => console.log(this.number);
  }
};

invocie.process()();    // 123
```

---

``` js
let invoice = {
  number: 123,
  process: function() {
    return () => console.log(this.number);
  }
};

let newInvoice = {
  number: 456
};

invoice.process().bind(newInvoice)();    // 123
invoice.process().call(newInvoice);      // 123
```

---

``` js
let getPrice = () => 5.99;
console.log(getPrice.hasOwnProperty("prototype")); // false
```

---

Default Function Parameters
===

---

``` js
let getProduct = function(productId = 1000) {
  console.log(productId);
};

getProduct();    // 1000
```

``` js
let getProduct = function(productId = 1000, type = "software") {
  console.log(productId + ", " + type);
};

getProduct(undefined, "hardware");    // 1000, hardware
```

---

``` js
let getTotal = function(price, tax = price * 0.07) {
  console.log(price + tax);
};

getTotal(5.00);    // 5.35
```

---

``` js
let baseTax = 0.07;
let getTotal = function(price, tax = price * baseTax) {
  console.log(price + tax);
};

getTotal(5.00);    // 5.35
```

---

``` js
let generateBaseTax = () => 0.07;
let getTotal = function(price, tax = price * generateBaseTax()) {
  console.log(price + tax);
};

getTotal(5.00);    // 5.35
```

---

``` js
let getTotal = function(price, tax = price * 0.07) {
  console.log(arguments.length);
};

getTotal(5.00);    // 1
```

---

``` js
let getTotal = function(price = adjustment, adjustment = 1.00) {
  console.log(price + adjustment);
};

getTotal();    // SyntaxError
               // Use before declaration
```

---

``` js
let getTotal = function(price = adjustment, adjustment = 1.00) {
  console.log(price + adjustment);
};

getTotal(5.00);    // 6
```

---

``` js
let getTotal = new Function("price = 20.00", "return price;");
console.log(getTotal());    // 20
```

---

Rest and Spread
===

---

`...` as rest operator

``` js
let showCategories = function(productId, ...categories) {
  console.log(categories instanceof Array);
  console.log(categories);
};

showCategories(123, "search", "advertising");

// true
// ["search, "advertising"]
```

---

``` js
let showCategories = function(productId, ...categories) {
  console.log(categories);
};

showCategories(123);    // []
```

---

``` js
let showCategories = function(productId, ...categories) {
};

console.log(showCategories.length);               // 1
```

``` js
let showCategories = function(productId, ...categories) {
  console.log(arguments.length);
};

showCategories(123, "search", "advertising");    // 3
```

``` js
let showCategories = 
  new Function("...categories", "return categories;");

console.log(showCategories("search", "advertising"));
// ["search", "advertising"]
```

---

`...` as spread operator

```js
let prices = [12, 20, 18];
let maxPrice = Math.max(...prices);

console.log(maxPrice);          // 20
```

``` js
let prices = [12, 20, 18];
let newPriceArray = [...prices];

console.log(newPriceArray);    // [12, 20, 18]
```

---

``` js
let newPriceArray = Array(...[,,]);
console.log(newPriceArray);    // [undefined, undefined]
```

``` js
let newPriceArray = [...[,,]];
console.log(newPriceArray);    // [undefined, undefined]
```

---

``` js
let maxCode = Math.max(..."43210");
console.log(maxCode);    // 4
```

``` js
let codeArray = ["A", ..."BCD", "E"];
console.log(codeArray);    // ["A", "B", "C", "D", "E]
```

----

Object Literal Extension
===

----

``` js
let price = 5.99;
let quantity = 30;

let productView = {
 price,
 quantity
};

console.log(productView);    // { price: 5.99, quantity: 30 }
```

---
shorthand function is arrow function
``` js
let price = 5.99;
let quantity = 30;

let productView = {
 price,
 quantity,
 calculateValue() {
   return this.price * this.quantity;
 }
};

console.log(productView.calculatevalue());
// 50.900000000000006
```

---


``` js
let price = 5.99;
let quantity = 30;

let productView = {
 price: 7.99,
 quantity: 1,
 calculateValue() {
   return this.price * this.quantity;
 }
};

console.log(productView.calculatevalue());
// 50.900000000000006
```

---

``` js
let price = 5.99;
let quantity = 30;

let productView = {
 price,
 quantity,
 "calculate Value"() {
   return this.price * this.quantity;
 }
};

console.log(productView.calculatevalue());
// 50.900000000000006
```

---

``` js
let field = "dynamicField";
let price = 5.99;

let productView = {
  [field]: price
};

console.log(productView);    // { dynamicField: 5.99 }
```

----

``` js
let field = "dynamicField";
let price = 5.99;

let productView = {
  [field + "-001"]: price
};

console.log(productView);    // { dynamicField-001: 5.99 }
```

---

``` js
let method = "doIt";
let productView = {
  [method + "-001"]() {
    console.log("in a method");
  };
};

productView["doIt-001"]();    // in a method
```

---

getter / setter

``` js
let ident = "productId";
let productView = {
  get [ident]() {
    return true;
  },
  set [ident](value) { }
};

console.log(productView.productId);    // true
```

----

for ... of Loops
===

---

``` js
let categories = ["hardware", "software", "vaporware"];

for (let item of caegories) {
  console.log(item);
  // hardware
  // software
  // vaporware
}
```

---

``` js
let categories = [,,];

for (let item of categories) {
  console.log(item);
  // undefined
  // undefined
} 
```

---

``` js
let codes = "ABCDF";
let count 0;

for (let code of codes) {
  count++;
}

console.log(count);    // 5
```

---

Octal and Binary Literals
===

---

# Octal value

``` js
let value = 0o10;
console.log(value);    // 8

let value = 0O10;
console.log(value);    // 8

let value = 0o71;
console.log(value);    // 56
```

``` js

```

---

# Binary value

``` js
let value = 0b10;
console.log(value);    // 2

let value = 0B10;
coonsole.log(value)    // 2

let value = 0b1101;
coonsole.log(value)    // 13
```
---

Template Literals
===

---

``` js
let invoiceNum = "1350";
console.log(`Invoice Number: ${invoiceNum}`);

// Invoice Number: 1350
```

``` js
let invoiceNum = "1350";
console.log(`Invoice Number: \${invoiceNum}`);

// Invoice Number: ${invoiceNum}
```

---

``` js
let message = `A
B
C`;

console.log(message);
```
```
A
B
C
```

---

``` js
let invoiceNum = "1350";
console.log("Invoice Number: ${"INV-" + invoiceNum}`);

// invoiceNumber: INV-1350
```

---

``` js
function showMessage(message) {
  let invoiceNum = "99";
  console.log(message);
}

let invoiceNum = "1350";
showMessage("Invoice Number: ${invoiceNum}");

// Invoice Number: 1350
```

---

# tagged template

``` js
function processInvoice(segments) {
  console.log(segments);
}

processInvoice `template`;    // ["template"]
```

``` js
function processInvoice(segments, ...values) {
  console.log(segments);
  console.log(values);
}

let invoiceNum = "1350";
let amount = "2000";

processInvoice `Invoice: ${invoiceNum} for ${amount}`;

// ["Invoice: ", " for ", ""]
// [1350, 2000]
```

---

Destructuring
===

---

``` js
let salary = ["32000", "50000", "75000"];
let [ low, average, high ] = salary;

console.log(average);    // 50000
```

``` js
let salary = ["32000", "50000"];
let [ low, average, high ] = salary;

console.log(high);    // undefined
```

``` js
let salary = ["32000", "50000", "75000"];
let [ low, , high ] = salary;

console.log(high);    // 75000
```

---

``` js
let salary = ["32000", "50000", "75000"];
let [ low, ...remaining ] = salary;

console.log(remaining);    // ["50000", "75000"]
```

``` js
let salary = ["32000", "50000"];
let [low, average, high = "88000"] = salary;

console.log(high);    // 88000
```

``` js
let salary = ["32000", "50000", ["88000", "99000]];
let [low, average, [actualLow, actualHigh]] = salary;

console.log(actualLow);    // 88000
```

---

``` js
let salary = ["32000", "50000"];
let low, average, high;
[low, average, high = "88000"] = salary;

console.log(high);    // 88000
```

---

``` js
function reviewSalary([low, average], high = "88000") {
  console.log(average);
}
 
reviewSalary(["32000", "50000"]);    // 50000
```

---

``` js
let salary = {
  low: "32000",
  average: "5000",
  high: "75000"
};

let { low, average, high } = salary;
console.log(high);    // 75000
```

----

# rename property
``` js
let salary = {
  low: "32000",
  average: "5000",
  high: "75000"
};

let { low: newLow, average: newAverage, high: newHigh } = salary;
console.log(newHigh);    // 75000
```

---
``` js
let salary = {
  low: "32000",
  average: "5000",
  high: "75000"
};

let newLow, newAverage, newHigh;
{ low: newLow, average: newAverage, high: newHigh } = salary;
console.log(newHigh);    // SyntaxError
```

---
``` js
let salary = {
  low: "32000",
  average: "5000",
  high: "75000"
};

let newLow, newAverage, newHigh;
({ low: newLow, average: newAverage, high: newHigh } = salary);
console.log(newHigh);    // 75000
```

---

``` js
let [maxCode, minCode] = "AZ";
console.log(`min: ${minCode} max: ${maxCode}`);
// min Z max A
```

---

Advanced Destructuring
===

---

``` js
let [ high, low ] = [,];
console.log(`high: ${high} low: ${low}`);
// high: undefined low: undefined
```

``` js
let [ high, low ] = undefined;
console.log(`high: ${high} low: ${low}`);
// RuntimeError: Unable to get property 'Symbol.iterator'
// of undefined or null reference
```

``` js
let [ high, low ] = null;
console.log(`high: ${high} low: ${low}`);
// RuntimeError: Unable to get property 'Symbol.iterator'
// of undefined or null reference
```

---

``` js
try {
  let [ high, low, ] = undefined;
}
catch(e) {
  console.log(e.name);    // TypeError
}
```

---

``` js
let [ high, low, ] = [500, 200];
console.log(`high: ${high} low: ${low}`);
// high: 500 low: 200
```

``` js
for (let [a, b] of [[5, 10]]) {
  console.log(`${a} ${b}`);    // 5 10
  count++;
}

console.log(count);            // 1
```
---

summary
===

---

# let, const and Block Scoping

``` js
let productId = 12;
console.log(productId);
```

---

# Arrow Functions

``` js
let getPrice = count => count * 4.00;
console.log(getPrice(2));
```

---

# Default Function Parameters

``` js
let getTotal = function(price, tax = price * 0.07) {
  console.log(price + tax);
}

getTotal(5.00);
```

---

# Rest and Spread Operators

``` js
let showCategories = function(productId, ...categories) {
}

console.log(showCategories.length);
```

``` js
let prices = [12, 20, 18];
let maxPrice = Math.max(...prices);

console.log(maxPrice);
```

---

# Object Literal Extensions

``` js
let method = "doIt";
let productView = {
  [method + "-001"]() {
    console.log("in a method");
  }
};

productView["doIt-001"]();
```

---

# for ... of Loops

``` js
let codes = "ABCDF";
let count = 0;

for (let code of codes) {
  count++;
}

console.log(count);
```

---

# Octal and Binary Literals

``` js
let value = 0o10;
console.log(value);
```

``` js
let value = 0b10;
console.log(value);
```

---

# Template Literals

``` js
let invoiceNum = "1350";
console.log("Invoice Nuber: ${invoiceNum}');
```

---

# Destructuring

``` js
let salary = ["32000", "50000", "75000"];
let [ low, average, high ] = salary;

console.log(average);
```

---