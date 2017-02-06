
Introduction
===

---

# ECMA 스크립트 
[Ecma 인터내셔널](http://www.ecma-international.org/)의 ECMA-262 기술 규격

| year | desc |
| :- | :- |
| 1995 | 넷스케이프의 Brendan Eich 10일만에 Mocha 완성 |
| 1995.09 | LiveScript로 변경 |
| 1995.12 | Sun에서 상표 라이센스를 받으며 JavaScript로 변경 |
| 1996 - 97 | 표준화를 위해 JavaSciript 기술 규격을 Ecma에 제출 |
| 2015.06 | 6번째 에디션 ES6, ES2015 |
| 2016.07 | 7번째 에디션 ES7, ES2016 |

ES.Next - 앞으로의 JavaScript

---

# Using ES6
http://kangax.github.io/compat-table/es6/

| browser / mobile | compatibility |
| :- | :-: |
| Edge 15 | 95% |
| FireFox 51 | 94% |
| Chrome 58 / Opera 45 | 97% |
| Safari 10.1 | 100% |
| Android 5.1 | 25% |
| iOS 10 | 100% |

---

New ES6 Syntax
===

---

# New ES6 Syntax

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



