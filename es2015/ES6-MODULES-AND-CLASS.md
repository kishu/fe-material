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

extends and super
===

---

# extends

``` js
class Project {
  constructor() {
    console.log("constructing Project");
  }
}

class SoftwareProject extends Project {

}

let p = new SoftwareProject();
```

``` text
constructing Project
```

---

# extends with constructor parameter

``` js
class Project {
  constructor(name) {
    console.log("constructing Project: + ${name}");
  }
}

class SoftwareProject extends Project {

}

let p = new SoftwareProject("Mazatlan");
```

``` text
constructing Project: Mazatlan
```

---

# super

``` js
class Project {
  constructor() {
    console.log("constructing Project");
  }
}

class SoftwareProject extends Project {
  constructor() {
    super();
    console.log("constructing SoftwareProject");
  }
}

let p = new SoftwareProject();
```

``` text
constructing Project
construction SoftwareProject
```

---

``` js
class Project {
  constructor() {
    console.log("constructing Project");
  }
}

class SoftwareProject extends Project {
  constructor() {
    // super();
    console.log("constructing SoftwareProject");
  }
}

let p = new SoftwareProject();
```

``` text
ReferenceError: this is not defined
```

---

``` js
class Project {
  // constructor() {
  //   console.log("constructing Project");
  // }
}

class SoftwareProject extends Project {
  constructor() {
    // super();
    console.log("constructing SoftwareProject");
  }
}

let p = new SoftwareProject();
```

``` text
ReferenceError: this is not defined
```

---

# using method

``` js
class Project {
  getTaskCount() {
    return 50;
  }
}

class SoftwareProject extends Project {

}

let p = new SoftwareProject();
console.log(p.getTaskCount());    // 50
```

---


``` js
class Project {
  getTaskCount() {
    return 50;
  }
}

class SoftwareProject extends Project {
  getTaskCount() {
    return 66;
  }
}

let p = new SoftwareProject();
console.log(p.getTaskCount());    // 66
```
---

``` js
class Project {
  getTaskCount() {
    return 50;
  }
}

class SoftwareProject extends Project {
  getTaskCount() {
    return super.getTaskCount() + 6;
  }
}

let p = new SoftwareProject();
console.log(p.getTaskCount());    // 56
```

---

``` js
class Project {
  getTaskCount() {
    return 50;
  }
}

let SoftwareProject = {
  getTaskCount() {
    return super.getTaskCount() + 7;
  }
}

Object.setPrototypeOf(softwareProject, project);

console.log(softwareProject.getTaskCount());    // 57
```

---

Properties for Class Instances
===

---

``` js
class Project {
  constructor() {
    this.location = "Mazatlan";
  }
}

class SoftwareProject extends Project {
  constructor() {
    super();
  }
}

let p = new SoftwareProject();
console.log(p.location);    // Mazatlan
```

---

``` js
class Project {
  constructor() {
    let location = "Mazatlan";
  }
}

class SoftwareProject extends Project {
  constructor() {
    super();
  }
}

let p = new SoftwareProject();
console.log(p.location);    // undefined
```

---

``` js
class Project {
  constructor() {
    this.location = "Mazatlan";
  }
}

class SoftwareProject extends Project {
  constructor() {
    super();
    this.location = this.location + " Beach";
  }
}

let p = new SoftwareProject();
console.log(p.location);    // Mazatlan Beach
```

---

Static Members
===

---

``` js
class Projcet {
  static getDefaultId() {
    return 0;
  }
}

console.log(Project.getDefaultId());    // 0
```

---

# static method

``` js
class Projcet {
  static getDefaultId() {
    return 0;
  }
}

var p = new Project();
console.log(p.getDefaultId());
```

``` text
Error: Object doesn't support property or method getDefaultId
```

---

# static property is not supprot

``` js
class Project {
  static let id = 0;
}

console.log(Project.id);
```

``` text
Syntax Error: ( expected
```

---

# not recommended

``` js
class Project {
  
}

Project.id = 99;

console.log(Project.id);    // 99
```

----

new.target
===

---

``` js
class Project {
  constructor() {
    console.log(typeof new.target);
    console.log(new.target);
  }
}

let p = new Project();
```

``` text
> function
> constructor() {
    console.log(typeof new.target);
    console.log(new.target);
  }
````

---

``` js
class Project {
  constructor() {
    console.log(new.target);
  }
}

class SoftwareProject extends Project {
  constructor() {
    super();
  }
}

let p = new SoftwareProject();
```

``` text
> constructor() {
    super();
  }
```

---

``` js
class Project {
  constructor() {
    console.log(new.target);
  }
}

class SoftwareProject extends Project {

}

let p = new SoftwareProject();
```

``` text
> constructor(...args) {
    super(...args);
  }
```

---

``` js
class Project {
  constructor() {
    console.log(new.target.getDefaultId());
  }
}

class SoftwareProject extends Project {
  static getDefaultId() {
    return 99;
  }
}

let p = new SoftwareProject();
```

``` text
> 99
```

---

summary
===

---

# Module Basics

``` js
// base.js
import { projectId } from "module1.js";
console.log(projectId);
```

``` js
//module1.js;
export let projectId = 99;
```

---

# Named Exports
``` js
// base.js
import { project } from "module1.js";
project.projectId = 8000;
console.log(project.projectId);
```

``` js
//module1.js
export let project = {
  projectId: 99;
}
```

---

# classes

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

---

# extends and super

``` js
class Project {
  constructor() {
    console.log("constructing Project");
  }
}

class SoftwareProject extends Project {
  constructor() {
    super();
    console.log("constructing SoftwareProject");
  }
}

let p = new SoftsareProject();
```

---

# Constructor Function Properties

``` js
class Project {
  constructor() {
    this.name = "Mazatlan";
  }
}

class SoftwareProject extends Project {
  constructor() {
    super();
  }
}

let p = new SoftsareProject();
console.log(p.name);
```

---

# Static Members

``` js
class Project {
  static getDefaultId() {
    return 0;
  }
}

console.log(Project.getDefaultId());
```

---

# new.target

``` js
class Project {
  constructor() {
    console.log(new.target);
  }
}

class SoftwareProject extends Project {

}

let p = new SoftwareProject();
```




