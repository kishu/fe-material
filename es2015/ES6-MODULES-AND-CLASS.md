ES6 Modules and Classes
===

---

Module Basics
Named Exports in Modules
Class Fundamentals
extends and super
Properties for Class Instances
Static Members
new.target
Summary

---

Module Basics
===

---

# import, export
``` js
// base.js
import { projectId } from "module1.js";
console.log(productId);    // 99
```

```
// module1.js
export let projectId = 99;
```

---

# multiple import, export

``` js
/// base.js
import { projectId, projectName } from "module1.js";
console.log(`${projectName} has id: ${projectId}`);

// BuildIt has id: 99
```

```
// module1.js
export let projectId = 99;
export let projectName = "buildIt";
```

---

# `as` keyword

``` js
/// base.js
import { projectId as id, projectName } from "module1.js";
console.log(`${projectName} has id: ${id}`);

// BuildIt has id: 99
```

```
// module1.js 
export let projectId = 99;
export let projectName = "buildIt";
```

---

``` js
/// base.js
import { projectId as id, projectName } from "module1.js";
console.log(`${projectName} has id: ${projectId}`);

// RuntimeError: projectId is undefined
```

```
// module1.js
export let projectId = 99;
export let projectName = "buildIt";
```

---

# sequence module load

``` js
//base.js
console.log("starting in base");
import { projectId } from "module1.js";
console.log("ending in base");
```

``` js
// module1.js
export let projectId = 99;
console.log("in module1");
```

```
in moudle1
starting in base
ending in base
```

`import` is hoisted

---

# `default` keyword

``` js
// base.js
import someValue from "module1.js";
console.log(someValue);    // buildIt
```

``` js
// module1.js
export let projectId = 99;
let projectName = "buildIt";
export default projectName;
```

---

``` js
// base.js
import { default as myPorjectName from "module1.js";
console.log(myProjectName);    // buildIt
```

``` js
// module1.js
export let projectId = 99;
let projectName = "buildIt";
export default projectName;
```

---

``` js
// base.js
import someValue from "module1.js";
console.log(someValue);    // undefined
```

``` js
// module1.js
let projectId = 99;
let projectName = "buildIt";
export { projectId, projectName };
```

---

``` js
// base.js
import someValue from "module1.js";
console.log(someValue);    // 99
```

``` js
// module1.js
let projectId = 99;
let projectName = "buildIt";
export { projectId as default, projectName };
```

---

# `*`

``` js
// base.js
import * as values from "module1.js";
console.log(values);

// { projectId: 99, projectName: "buildIt" }
```

``` js
// module1.js
let projectId = 99;
let projectName = "buildIt";
export { projectId, projectName };
```

----

Named Exports in Modules
===

---

# read-only

``` js
// base.js
import { projectId } from "module1.js";
projectId = 8000;

console.log(projectId);

// RuntimeError: projectId is read-only
```

``` js
// module1.js
export let projectId = 99;
```

---

# modify property

``` js
// base.js
import { project } from "module1.js";
project.projectId = 8000;

console.log(projectId);    // 8000
```

``` js
// module1.js
export let project = {
  projectId: 99;
};
```

---

``` js
// base.js
import { project, showProject } from "module1.js";
project.projectId = 8000;
showProject();
console.log(project.projectId);      // 8000
```

```
// module1.js
export let project = { projectId: 99 };
export function showProject() {
  console.log(project.projectId);    // 8000
};
```

---

``` js
// base.js
import { showProject, updateFunction } from "module1.js";
showProject();
updateFunction();
showProject();
```

``` js
// module1.js
export function showProject() { console.log("in original"); }
export function updateFunction() {
  showProject = function() { console.log("in updated"); };
}
```

```
// result
in original
in updated
```

---

Class Fundamentals
===

---

``` js
class Task {

}

console.log(typeof Task);    // function
```

---

``` js
class Task {

}

let task = new Task();
console.log(typeof task);             // object
```

``` js
class Task {

}

let task = new Task();
console.log(task instanceof Task);    // true
```

---

``` js
class Task {
  showId() {
    console.log("99");
  }
}

let task = new Task();
task.showId();    // 99

console.log(task.showId === Task.prototype.showId);  // true
```

---

``` js
class Task {
  constructor() {
    console.log("constructing Task");
  }
  showId() {
    console.log("99");
  }
}

let task = new Task();
```

``` text
// result
constructing Task
```

---

``` js
class Task {
  constructor() {
    console.log("constructing Task");
  },
  showId() {
    console.log("99");
  }
}

let task = new Task();
```

``` text
// result
SyntaxError
```

---

``` js
class Task {
  let tsdkId = 9000;
  constructor() {
    console.log("constructing Task");
  }
  showId() {
    console.log("99");
  }
}

let task = new Task();
```

``` text
// result
SyntaxError
```

---

# class is not hoisted

``` js
let task = new Task();

class Task {
  constructor() {
    console.log("constructing Task");
  }
}
```

``` text
// result
Error: Use before declaration
```

---

``` js
let newClass = class Task {
  constructor() {
    console.log("constructing Task");
  }
}

new newClass();
```

``` text
// result
constructing Task
```

---

``` js
let Task = function() {
  console.log("constructing Task");
};

let task = {};
Task.call(task);
```

``` text
// result
constructing Task
```

---

``` js
class Task {
  constructor() {
    console.log("constructing Task");
  }
}

let task = {};
Task.call(task);
```

``` text
// result
Error: class constructor cannot be called with the new keyword 
```

---

``` js
function Project() { }
console.log(window.Project === Project);    // true
```

``` js
class Task { }
console.log(window.Task === Task);         // false
```

---









