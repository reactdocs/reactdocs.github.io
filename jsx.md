# JSX

> JSX is an extension for JavaScript.

JSX is used to generate HTML for React pages.

Since JSX is JavaScript extention, it can go anywhere JavaScript goes.

But as JSX is JavaScript extention, it needs a compiler like Babel to be translated to JavaScript.

*Example-*

> `const injection = <p>New student</p>` is valid JSX expression. It is an example of a react element.

If we had a `<div>` element
```
<div id="appDiv">
</div>
```

In plain JavaScript HTML can be inserted into div like this:

```javascript
const injection = "<p>New student</p>";
document.getElementById("appDiv").innerHTML = injection;
```
In React/JSX environment same is done by:

```javascript

import React from 'react';
import ReactDOM from 'react-dom';

const injection = <p>New student</p>;
ReactDOM.render(
  injection,
  document.getElementById('appDiv')
);
```
check [rendering](./rendering.md)

### Usage

Use of variables within JSX

```javascript
const studentType = 'brilliant';
const studentStatus = <p>New {brilliant} student</p>;
```

Use of Template literals (Template strings) within JSX

```javascript
const studentType = 'brilliant';
const studentStatus = <p>`New ${brilliant} student`</p>;
```

So as you may have guessed, you can put any JavaScript within JSX, that evaluates to something.

```javascript

function studentActivity() { return "Admitted";}
const studentStatus = <p>New student {studentActivity} </p>;

const studentGrade = 9;
const studentStatus = <p>New {studentGrade > 8 ? "High school":""} student  </p>;
```

Or you can return JSX from functions, even conditionally.

```javascript

function studentActivity() { return <i>Admitted</i>;}
const studentStatus = <p>New student {studentActivity} </p>;

```

### Only 1 outer JSX element allowed

Note above code will result in `const studentStatus = <p>New student <i>Admitted</i> </p>`

This is valid because there is only 1 outer element `<p>`

> It is good practice to wrap JSX in parentheses and break it to multiple line for redability


So above code would look like 

```javascript

const studentStatus = (
  <p>
    New student
    <i>
      Admitted
    </i>
  </p>
  );

```

Previous: [React Introduction](README.md) | Next: [Rendering](rendering.md)



