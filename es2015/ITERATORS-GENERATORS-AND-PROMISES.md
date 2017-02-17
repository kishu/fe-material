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

Promise
===

---

# resolve

``` js
function doAsync() {
  let p = new Promise((resolve, reject) => {
    console.log("in promise code");

     setTimeout(() => {
      console.log("resolving...");
      resolve();
    }, 2000);
  });
  
  return p;
}

let promise = doAsync();
```

``` text
> in promise code

(2 second delay)
> resolving...
```

---

# reject

``` js
function doAsync() {
  let p = new Promise((resolve, reject) => {
    console.log("in promise code");

     setTimeout(() => {
      console.log("rejecting...");
      reject();
    }, 2000);
  });
  
  return p;
}

let promise = doAsync();
```

``` text
> in promise code

(2 second delay)
> rejecting...
```

---

# then

``` js
function doAsync() {
  // returns a Promise will be rejected
}

doAsync().then(() => {
  console.log("Fulfilled!");
}, () => {
  console.log("Rejected!");
)};
```

``` text
(wait for promise code resolution)
> Rejected!
```

---

``` js
function doAsync() {
  // returns a Promise will be resolved
}

doAsync().then(() => {
  console.log("Fulfilled!");
}, () => {
  console.log("Rejected!");
)};
```

``` text
(wait for promise code resolution)
> Fulfilled!
```

---

``` js
function doAsync() {
  // returns a Promise will be rejected using:
  // reject("Database Error");
}

doAsync().then((value) => {
  console.log(`Fulfilled! ${vaule}`);
}, (reason) => {
  console.log(`Rejected! ${value}`);
)};
```

``` text
(wait for promise code resolution)
> Rejected! Database Error
```

---

``` js
function doAsync() {
  // returns a Promise will be resolve using:
  // resolve("OK");
}

doAsync().then(value => {
  console.log(`Fulfilled! ${vaule}`);
}, reason => {
  console.log(`Rejected! ${reason}`);
)};
```

``` text
(wait for promise code resolution)
> Fulfilled! OK
```

---

``` js
function doAsync() {
  // returns a Promise will be resolve using:
  // resolve("OK");
}

doAsync().then(value => {
  console.log(`Fulfilled! ${vaule}`);
  return "For Sure";
}).then(value => {
  console.log(`Really! ${value}`);
)};
```

``` text
(wait for promise code resolution)
> Fulfilled! OK
> Really! For Sure
```

---

``` js
function doAsync() {
  // returns a Promise, will be rejected using:
  // reject("No Go");
}

doAsync().catch(reason => {
  console.log(`Error: ${reason}`);
});
```

``` text
(wait for resolution)
> Error: No Go
```

---

More Promise Features
===

---

``` js
function doAsync() {
  let p = new Promise((resolve, reject) => {
    console.log("in promise code");
    setTimeout(() => {
      resolve(getAnotherPromise());
    }, 2000);
  });
  
  return p;
}

doAsync().then(() => {
  console.log("OK")
}, () => {
  console.log("Nope");
});
```

``` text
> in promise code
> Nope
```

---

``` js
function doAsync() {
  return Promise.resolve("Some String");
}

doAsync().then((value) => {
  console.log(`OK: ${value}`);
}, (reason) => {
  console.log(`Nope: ${reason}`);
});
```

``` text
> OK: Some String
```

---

``` js
function doAsync() {
  return Promise.reject("Some Error");
}

doAsync().then((value) => {
  console.log(`OK: ${value}`);
}, (reason) => {
  console.log(`Nope: ${reason}`);
});
```

``` text
> Nope: Some Error
```

---

# Promise.all with resolve

``` js
let p1 = new Promise(...);
let p2 = new Promise(...);

Promise.all([p1, p2]).then((value) => {
  console.log("OK");
}, (reason) => {
  console.log("Nope");
});

// asume p1 resolves after 3 seconds
// asume p2 resolves after 5 seconds
```

``` text
(5 second delay)
> OK
```

---

# Promise.all with resolve and reject

``` js
let p1 = new Promise(...);
let p2 = new Promise(...);

Promise.all([p1, p2]).then((value) => {
  console.log("OK");
}, (reason) => {
  console.log("Nope");
});

// asume p1 resolves after 1 seconds
// asume p2 is rejected after 2 seconds
```

``` text
(2 second delay)
> Nope
```

---

# Promise.all with reject

``` js
let p1 = new Promise(...);
let p2 = new Promise(...);

Promise.all([p1, p2]).then((value) => {
  console.log("OK");
}, (reason) => {
  console.log("Nope");
});

// asume p1 is rejected after 3 seconds
// asume p2 is rejected after 5 seconds
```

``` text
(3 second delay)
> Nope
```

---

# Promise.race

``` js
let p1 = new Promise(...);
let p2 = new Promise(...);

Promise.race([p1, p2]).then((value) => {
  console.log("OK");
}, (reason) => {
  console.log("Nope");
});

// asume p1 resolves after 3 seconds
// asume p2 resolves after 5 seconds
```

``` text
(3 second delay)
> OK
```

---

# Promise.race

``` js
let p1 = new Promise(...);
let p2 = new Promise(...);

Promise.race([p1, p2]).then((value) => {
  console.log("OK");
}, (reason) => {
  console.log("Nope");
});

// asume p1 is rejected after 3 seconds
// asume p2 resolves after 5 seconds
```

``` text
(3 second delay)
> Nope
```

---

# Promise.race

``` js
let p1 = new Promise(...);
let p2 = new Promise(...);

Promise.race([p1, p2]).then((value) => {
  console.log("OK");
}, (reason) => {
  console.log("Nope");
});

// asume p1 resolves after 4 seconds
// asume p2 is rejected after 5 seconds
```

``` text
(4 second delay)
> OK
```