New Types and Object Extensions
===

---
<!-- page_number: true -->

Symbols
Well-known Symbols
Object Extensions
String Extensions
Number Extension
Math Extehsions
RegExp Extensions
Function Extensions

---

Symbols
===
 
> A symbol is a unique and immutable data type and
> may be used as an identifier for object properties
> "Mozilla Developer Network"

---

# symbol type

``` js
let eventSymbol = Symbol("resize event");
console.log(typeof eventSymbol);    
console.log(eventSymbol.toString());
```

``` text
> symbol
> Symbol(resize event)
```

---

``` js
const CALCULATE_EVENT_SYMBOL = Symbol("calculate event");
console.log(CALCULATE_EVENT_SYMBOL.toString());
```

``` text
> Symbol(calculate event)
```

---

``` js
let s = Symbol("event");
let s2 = Symbol("event");

console.log(s === s2);
```

``` text
> false
```

---

# Symbol.for()

``` js
let s = Symbol.for("event");
console.log(s.toString());
```

``` text
> Symbol(event)
```

---

``` js
let s = Symbol.for("event");
let s2 = Symbol.for("event");

console.log(s === s2);
```

``` text
> true
```

---

``` js
let s = Symbol.for("event");
let s2 = Symbol.for("event2");

console.log(s === s2);
```

``` text
> false
```

---

# Symbol.keyFor()

``` js
let s = Symbol.for("event");
let description = Symbol.keyFor(s);

console.log(description);
```

``` text
> event
```

---

``` js
let article = {
  tytle: "Whiteface Mountain",
  [Symbol.for("article")]: "My Article"
};

let value = article[Symbol.for("article")];
console.log(value);
```

``` text
> My Article
```

---

``` js
let article = {
  tytle: "Whiteface Mountain",
  [Symbol.for("article")]: "My Article"
};

console.log(Object.getOwnPropertyNames(article)));
console.log(Object.getOwnPropertySymbols(article)));
```

``` text
> ["title"]
> [Symbol(article)]
```

---

Well-known Symbols
===

---

``` js
let Blog = function() {

};

let blog = new Blog();
console.log(blog.toString());
```

``` text
> [object Object]
```

---

``` js
let Blog = function() {

};

Blog.prototype[Symbol.toStringTag] = "Blog Class";

let blog = new Blog();
console.log(blog.toString());
```

``` text
> [object Blog Class]
```

---

``` js
let values = [8, 12, 16];
console.log([].concat(values));
```

``` text
> [8, 12, 16]
```

---

``` js
let values = [8, 12, 16];
values[Symbol.isConcatSpreadable] = false;

console.log([].concat(values));
```

``` text
> [ [8, 12, 16] ]
```

---

``` js
let values = [8, 12, 16];
let sum = values + 100;

console.log(sum);
```

``` text
> 8,12,16100
```

---

``` js
let values = [8, 12, 16];
values[Symbol.toPrimitive] = function(hint) {
  console.log(hint);
  return 42;
};

let sum = values + 100;
console.log(sum);
```

``` text
> default
> 142
```

---

Object
===

---

``` js
let a = { x: 1 };
let b = { y: 2 };

Object.setPrototypeOf(a, b);
console.log(a.y);
```

``` text
> 2
```

---

``` js
let a = { a: 1 };
let b = { b: 2 };

let target = {};
Object.assign(target, a, b);

console.log(target);
```

``` text
> { a: 1, b: 2 }
```

---

``` js
let a = { a: 1 };
let b = { a: 5, b: 2 };

let target = {};
Object.assign(target, a, b);

console.log(target);
```

``` text
> { a: 5, b: 2 }
```

---

``` js
let a = { a: 1 };
let b = { a: 5, b: 2 };

Object.defineProperty(b, "c", {
  value: 10,
  enumerable: false
});

let target = {};
Object.assign(target, a, b);

console.log(target);
```

``` text
> { a: 5, b: 2 }
```

---

``` js
let a = { a: 1 };
let b = { a: 5, b: 2 };
let c = { c: 20 }

Object.setPrototypeOf(b, c);

let target = {};
Object.assign(target, a, b);

console.log(target);
```

``` text
> { a: 1, b: 2 }
``` 

---

# Object.is

``` js
let amount = NaN;
console.log(amount === amount);
```

``` text
> false
```

<br>

``` js
let amount = NaN;
console.log(Object.is(amount, amount));
```

``` text
> true
```

---

``` js
let amount = 0;
let total = -0;

console.log(amount === total);
```

``` text
> true
```

<br>

``` js
let amount = 0;
let total = -0;

console.log(Object.is(amount, total));
```

``` text
> false
```

---

String Extensions
===

---

``` js
let title = "Santa Barbara Surf Riders";
console.log(title.startsWith("Santa"));
console.log(title.endsWith("Rider"));
console.log(title.includes("ba"));
```

``` text
> true
> false
> true
```

---

``` js
let title = "Surfer's \u{1f3c4} Blog";
console.log(title);

let surfer = "\u{1f3c4}";
console.log(surfer.length);
```

``` text
> Surfer's 🏄  Blog
> 2
```

---

``` js
var surfer = "\u{1f30a}\u{1f3c4}\u{1f40b}";

console.log(Array.from(surfer).length);
console.log(surfer);
```

``` text
> 3
> 🌊 🏄 🐋
```

---

``` js
let title = "Mazatla\u0301n";
console.log(`${title}: ${title.length}`);

let normalizeTitle = title.normalize();
console.log(`${normalizeTitle}: ${normalizeTitle.length}`);
```

``` text
Mazatlán: 9
Mazatlán: 8
```

---

``` js
let title = "Mazatla\u0301n";

console.log(title.normalize().codePointAt(7));
console.log(title.normalize().codePointAt(7).toString(16));
console.log(String.fromCodePoint(0x1f3c4);
```

``` text
> 110
> 6e
> 🏄
```

---

``` js
let title = "Surfer";
let output = String.raw`${title} \u{1f3c4|\n`;

console.log(output);
```

``` text
> Surfer \u{1f3c4}\n
```

---

# repeat

``` js
let wave = "\u{1f30a}";
console.log(wave.repeat(10));
```

``` text
> 🌊🌊🌊🌊🌊🌊🌊🌊🌊🌊
```

---

Number Extensions
===

---

# Number.parseInt, Number.parseFloat

``` js
console.log(Number.parseInt === parseInt);
console.log(Number.parseFloat === parseFloat);
```

``` text
> true
> true
```

---

# Number.isNaN

``` js
let s = "NaN";
console.log(isNaN(s));
console.log(Number.isNaN(s));
console.log(Number.isNaN === isNan);
```

``` text
> true
> false
> false
```

---

# Number.isFinite

``` js
let s = "8000";
console.log(isFinite(s));
console.log(Number.isFinite(s));
console.log(Number.isFinite === isFinite);
```

``` text
> true
> false
> false
```

---

# Number.isInteger

```js
let sum = 408.2;
console.log(Number.isInteger(sum));
```

``` text
> false
```

<br>

``` js
console.log(Number.isInteger(NaN));         // false
console.log(Number.isInteger(Infinity));    // false
console.log(Number.isInteger(undefined));   // false
console.log(Number.isInteger(3));           // true
```

---

# Number.isSafeInteger

``` js
let a = Math.pow(2, 53) - 1;
console.log(Number.isSafeInteger(a));

let b = Math.pow(2, 53);
console.log(Number.isSafeInteger(a));
```

``` text
> true
> false
```

<br>

**Safe Integer**
* IEEE-754 배정도 수로 정확하게 표현할 수 있는 정수
* 2^53 - 1 ~ -(2^53 - 1)

---

# Constants of Number

``` js
console.log(Number.EPSILON);
console.log(Number.MAX_SAFE_INTEGER);
console.log(Number.MIN_SAFE_INTEGER);
```

``` text
> 2.220446049250313e-16
> 9007199254740991
> -9007199254740991
```

---

Math Extensions
===

---

# Hyperbolic(쌍곡선) Functions

## Math.sinh(), Math.cosh(), Math.tanh()
* 상수 e를 사용해 표현할 수 있는 쌍곡선 sin, cos, tan 리턴

## Math.asinh(), Math.acosh(), Math.atanh()
* 쌍곡선 arc-sin, arc-cos, arc-tan 리턴

## Math.hypot()
* 인수의 제곱합에 대한 제곱근 리턴

---

# Arithmetic(산술) Functions

| function | desc |
| -------- | ---- |
| cbrt()   | 큐브 루트(세제곱근) |
| clz32()  | 32bit integer에서 선행 0bit의 수|
| expm1()  | exp(x) - 1 |
| log2()   | log base 2 |
| log10()  | log base 10 |
| clog1p() | log(x + 1) |
| imul()   | 32 bit interger 곱 |

---

# Miscellaneous Functions

| function | desc |
| -------- | ---- |
| sign()   | 부호 리턴. -1, 1 0, -0, NaN |
| trunc()  | 정수부 리턴 |
| fround() | 가장 가까운 32bit 부동 소수점 값으로 반올림 |

---

# Math.sign

``` js
console.log(Math.sign(0));
console.log(Math.sign(-0));
console.log(Math.sign(-20));
console.log(Math.sign(20));
console.log(Math.sign(NaN));
```

``` text
> 0
> -0
> -1
> 1
> NaN
```

---

# Math.cbrt

``` js
console.log(Math.cbrt(27));
```

``` text
> 3
```

# Math.trunc

``` js
console.log(Math.trunc(27.1));
console.log(Math.trunc(-27.9));
```

``` text
> 27
> -27
```

---

RegExp Extensions
===

---

``` js
let pattern = /\u{1f3c4}/;
console.log(pattern.text("🏄"));
```

``` text
> false
```

<br>

``` js
let pattern = /\u{1f3c4}/u;
console.log(pattern.text("🏄"));
```

``` text
> true
```

---

``` js
let pattern = /^.Surfer/;
console.log(pattern.text("🏄Surfer"));
```

``` text
> false
```

<br>

``` js
let pattern = /^.Surfer/u;
console.log(pattern.text("🏄Surfer"));
```

``` text
> true
```

---

``` js
let pattern = /900/y;
console.log(pattern.lastIndex);
console.log(pattern.text("800900"));
```

``` text
> 0
> false
```

<br>

``` js
let pattern = /900/y;
pattern.lastIndex = 3 ;
console.log(pattern.text("800900"));
```

``` text
> true
```

---

``` js
let pattern = /900/yg;
console.log(pattern.flags);
```

``` text
> gy    // order will be "gimuy"
```

---

Function Extensions
===

----

``` js
let fn = function calc() {
  return 0;
};

console.log(fn.name);
```

``` text
> calc
```

---

``` js
let fn = function() {
  return 0;
};

console.log(fn.name);
```

``` text
> fn
```

---

``` js
let fn = function() {
  return 0;
};

let newFn = fn;
console.log(newFn.name);
```

``` text
> fn
```

<br>

Function.name isnt't writable
Configurable with Object.defineProperty()

---

``` js
class Calculator {
  constructor() {
  }
  
  add() {
  }
}

let c = new Calculator();

console.log(Calculator.name);
console.log(c.add.name);
```

``` text
> Calculator
> add
``` 

---

Summary
===

---

# Symbols

``` js
let eventSymbol = Symbol("resize event");
console.log(typeof eventSymbol);
```

# Well-known Symbols

``` js
let values = [8, 12, 16];
values[Symbol.isConcatSpreadable] = false;
console.log([].concat(values));
```

---

# Object Extensions

``` js
let a = {
  x: 1
};

let b = {
  y: 2
};

Object.setPrototypeOf(a, b);
console.log(a.y);
```

---

# String Extensions

``` js
let title = "Surfer's \u{1f3c4} Blog";
console.log(title);
```

---

# Number Extensions

``` js
let s = "NaN";

console.log(isNaN(s));
console.log(Number.isNaN(s));
```

---

# Math Extensions

``` js
console.log(Math.sign(0));
console.log(Math.sign(-0));
console.log(Math.sign(-20));
console.log(Math.sign(20));
console.log(Math.sign(NaN));
```

---

# RegExp Extensions

``` js
let pattern = /\u{1f3c4}/u;
console.log(pattern.test("🏄"));
```

---

# Function Extensions

``` js
let fn = function() {
  return 0;
};

console.log(fn.name)
```
