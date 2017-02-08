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