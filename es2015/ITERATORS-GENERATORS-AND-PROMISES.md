Iterators, Generators, and
Promises

===

---

Iterators
Generators
Yielding in Generators
throw and return
Promises
Promises Features

---

Iterators
===

---

``` js
let ids = [9000, 9001, 9002];
console.log(typeof ids[Symbol.iterator]);
```

``` text
> function
```

---

``` js
let ids = [9000, 9001, 9002];
let it = ids[Symbol.iterator]();
console.log(it.next());
```

``` text
> { done: false, value: 9000 }
```

---

``` js
let ids = [9000, 9001, 9002];
let it = ids[Symbol.iterator]();
it.next();
it.next();
console.log(it.next());
```

``` text
> { done: false, value: 9002 }
```

---

``` js
let ids = [9000, 9001, 9002];
let it = ids[Symbol.iterator]();
it.next();
it.next();
it.next();
console.log(it.next());
```

``` text
> { done: true, value: undefined }
```

---

``` js
let idMaker = {
  [Symbol.iterator]() {
    let nextId = 8000;
    return {
      next() {
        return {
          value: nextId++,
          done: false
        };
      }
    };
  }
};

let it = idMaker[Symbol.iterator]();

console.log(it.next().value);
console.log(it.next().value);
```

``` text
> 8000
> 8001
```

---

``` js
let idMaker = {
  [Symbol.iterator]() {
    let nextId = 8000;
    return {
      next() {
        return {
          value: nextId++,
          done: false
        };
      }
    };
  }
};

for (let v of idMaker) {
  if (v > 8002) break;
  console.log(v);
}
```

``` text
> 8000
> 8001
> 8002
```

---

``` js
let idMaker = {
  [Symbol.iterator]() {
    let nextId = 8000;
    return {
        next() {
        let value = nextId > 8002 ? undefined : nextId ++;
        let done = !value;
        return { value, done };
      }
    };
  }
};

for (let v of idMaker) {
  console.log(v);
}
```

``` text
> 8000
> 8001
> 8002
```

---

``` js
let ids = [8000, 8001, 8002];

function process(id1, id2, id3) {
  console.log(id3);
}

process(...ids);

```

``` text
> 8002
```

---

Generators
===

---

``` js
function *process() {
  yield 8000;
  yield 8001;
}

let it = process();
console.log(it.next());
```

``` text
{ done: false, value: 8000 }
```

---

``` js
function *process() {
  yield 8000;
  yield 8001;
}

let it = process();
it.next();
console.log(it.next());
```

``` text
{ done: false, value: 8001 }
```

---

``` js
function *process() {
  yield 8000;
  yield 8001;
}

let it = process();
it.next();
it.next();
console.log(it.next());
```

``` text
{ done: true, value: undefined }
```

---

``` js
function *process() {
  let nextId = 7000;
  while(true)
    yield(nextId++);
}

let it = process();
it.next();
console.log(it.next().value);
```

``` text
> 7001
```

---

``` js
function *process() {
  let nextId = 7000;
  while(true)
    yield(nextId++);
}

for (let id of process()) {
  if (id > 7002) break;
  console.log(id);
}
```

``` text
> 7000
> 7001
> 7002
```

---

Yielding in Generators
===

---

``` js
function *process() {
  yield 8000;
}

let it = process();
console.log(it.next());
```

``` text
> { done: false, value: 8000 }
```

---

``` js
function *process() {
  yield;
}

let it = process();
console.log(it.next());
```

``` text
> { done: false, value: undefined }
```

---

``` js
function *process() {
  let result = yield;
  console.log(`result is ${result}`);
}

let id = process();
it.next();
it.next(200);
```

``` text
> result is 200
```

---

``` js
function *process() {
  let result = yield;
  console.log(`result is ${result}`);
}

let id = process();
it.next();
console.log(it.next(200));
```

``` text
> result is 200
> { done: true, value: undefined } 
```

---

``` js
function *process() {
  let newArray = [yield, yield, yield];
  console.log(newArray[2]);
}

let it = process();
it.next();
it.next(2);
it.next(4);
it.next(42);
```

``` text
> 42
```

---

``` js
function *process() {
  let value = 4 * (yield 42);
  console.log(value);
}

let it = process();
it.next();
it.next(10);
```

``` text
> 40
```

---

``` js
function *process() {
  yield 42;
  yield [1, 2, 3];
}

let it = process();
console.log(it.next().value);
console.log(it.next().value);
console.log(it.next().value);
```

``` text
> 42
> [1, 2, 3]
> undefined
```

---

``` js
function *process() {
  yield 42;
  yield* [1, 2, 3];
}

let it = process();
console.log(it.next().value);
console.log(it.next().value);
console.log(it.next().value);
console.log(it.next().value);
console.log(it.next().value);
```

``` text
> 42
> 1
> 2
> 3
> undefined
```

---

throw and return
===

---

``` js
function *process() {
  try {
    yield 9000;
    yield 9001;
    yield 9002;
  }
  catch(e) {
  }
}

let it = process();
console.log(it.next().value);
console.log(it.throw('foo'));
console.log(it.next());
```

``` text
> 9000
> { done: true, value: undefined }
> { done: true, value: undefined }
```

---

``` js
function *process() {
  yield 9000;
  yield 9001;
  yield 9002;

}

let it = process();
console.log(it.next().value);
console.log(it.throw('foo'));
console.log(it.next());
```

``` text
> 9000
> Exception: foo (excution terminates)
```

---

``` js
function *process() {
  yield 9000;
  yield 9001;
  yield 9002;
}

let it = process();
console.log(it.next().value);
console.log(it.return("foo"));
console.log(it.next());
```

``` text
> 9000
> { done: true, value: "foo" }
> { done: true, value: undefined }
```

---


