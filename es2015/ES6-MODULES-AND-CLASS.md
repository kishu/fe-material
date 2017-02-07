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




