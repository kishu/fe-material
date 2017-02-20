Arrays and Collections
===

---

Array Extensions
ArrayBuffer and Typed Arrays
DataView and Endianness
Map and WeakMap
Set and WeakSet
Subclassing

---

Array Extensions
===

---

``` js
let salaries = Array(90000);
console.log(salaries.length);
```

``` text
> 90000
```

---

# Array.of()

``` js
let salaries = Array.of(90000);
let prices = Array.of(300, 400, 500);
let arr = Array.of(undefined);

console.log(salaries.length);
console.log(prices);
console.log(arr);
```

``` text
> 1
> [300, 400, 500]
> [undefined]
```

---

# Array.from()

``` js
let amounts = [800, 810, 820];
let salaries = Array.from(amounts, v => v + 100);

console.log(salaries);
```

``` text
> [900, 920, 920]
```

---

``` js
let amounts = [800, 810, 820];
let salaries = Array.from(amounts, function(v) {
  return v + this.adjustment;
}, { adjustment: 50 });

console.log(salaries);
```

``` text
> [850, 860, 870]
```

---

``` js
let amounts = [800, 810, 820];
let salaries = Array.from(amounts,
  v => v + this.adjustment, 
  { adjustment: 50 });

console.log(salaries);
```

``` text
>  [NaN, NaN, NaN]
```

---

# Array.fill()

``` js
let salaries = [600, 700, 800];
salaries.fill(900);

console.log(salaries);
```

``` text
> [900, 900, 900]
```

---

``` js
let salaries = [600, 700, 800];
salaries.fill(900, 1);

console.log(salaries);
```

``` text
> [600, 700, 800]
```

---

``` js
let salaries = [600, 700, 800];
salaries.fill(900, 1, 2);

console.log(salaries);
```

``` text
> [600, 900, 800]
```

---

``` js
let salaries = [600, 700, 800];
salaries.fill(900, -1);

console.log(salaries);
```

``` text
> [600, 700, 900]
```

---

# Array.find()

``` js
let salaries = [600, 700, 800];
let result = salaries.find(v => v >= 750);

console.log(result);
```

``` text
> 800
```

---

``` js
let salaries = [600, 700, 800];
let result = salaries.find(v => v >= 650);

console.log(result);
```

``` text
> 700
```

---

# Array.findIndex()

``` js
let salaries = [600, 700, 800];
let result = salaries.findIndex(function(value, index, array) {
  return value == this;
}, 700);

console.log(result);
```

``` text
> 1
```

---

# Array.copyWithin()

``` js
let salaries = [600, 700, 800];
salaries.copyWithin(2, 0);

console.log(salaries);
```

``` text
> [600, 700, 600]
``` 

---

``` js
let ids = [1, 2, 3, 4, 5];
ids.copyWithin(0, 1);

console.log(ids);
```

``` text
> [2, 3, 4, 5, 5]
```

---

``` js
let ids = [1, 2, 3, 4, 5];
ids.copyWithin(3, 0, 2);

console.log(ids);
```

``` text
> [1, 2, 3, 1, 2]
```

---

# Array.entries()

``` js
let ids = ["A", "B", "C"];
console.log(...ids.entries());
```

``` text
>[0, "A], [1, "B"], [2, "C"]
```

---

# Array.keys()

``` js
let ids = ["A", "B", "C"];
console.log(...ids.keys());
```

``` text
> 0 1 2
```

---

# Array.values()

``` js
let ids = ["A", "B", "C"];
console.log(...ids.values());
```

``` text
> A B C
```

---


