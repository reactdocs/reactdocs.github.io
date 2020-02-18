# Lifecycle

> In a nutshell, a component's lifecycle consistes of **Mounting**, **Updating**, and **Unmounting**.


## Main Methods-

#### Mounting Methods:

###### Constructor

> If a constructor is present, it is the first method to get called, amking it the best place for initializing things like initial state.

###### Render

> Required method that puts HTML in DOM.

###### componentDidMount

> componentDidMount is called after the first render which puts ibto HTML the initial values.

*Example-*

Here, just after first render the state will be changed to ```reader: "AfterMountReader"``` which will show up in screen.

```javascript
class ExampleClassComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
        reader: "Student"
        };
  }
  componentDidMount() {
      this.setState({reader: "AfterMountReader"})
      this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  changeReader = () => {
    this.setState({
        reader: "Clicker"
    });
  }

  render() {
    return <h1 onClick={this.changeReader}>Hello, {this.state.reader}</h1>;
  }
}
```

#### componentDidMount using hooks

> useEffect hooks runs after every render. So to replicate componentDidMount which only runs once, pass an empty array as second parameter, which basically says watch for no changes and so dont excecute again.

```javascript
import React, { useState, useEffect } from 'react';

function ExampleFunctionComponent () {
    const [reader, setReader] = useState('Student');

    useEffect(() => {
        setReader('AfterMountReader');
    }, []); // Only run once

    return (
        <h1 onClick={() => setReader('Clicker')}>Hello, {reader}</h1>
        );
}
```


Previous: [Function Component State via Hooks](functionState.md) | Next: [Mini Project 1](./project1.md)

