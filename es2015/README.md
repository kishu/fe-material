
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






----

# 변수 선언 방법
![](https://pbs.twimg.com/media/B9-Pt5lIgAAXLOm.png:large)

