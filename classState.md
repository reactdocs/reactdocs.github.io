# Class Component State

> State of a class is an Object that maintains internal data in ```key: value``` format

> State object of a class is derived from React.Component

> State object is initialized in constructor.

> Class components always call constructor with props as argument like ```onstructor(props)```.

>Constructor in turn calls React.Component passing props via ```super(props)```.


*Example-*

```javascript
class ExampleClassComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
        reader: "Student"
        };
  }
  render() {
    return <h1>Hello, {this.state.reader}</h1>;
  }
}
```

#### Getting State

> Note above how Class components use ```this.state``` to access state properties like ```this.state.reader```.

#### Setting State

> Class component use ```this.setstate()``` method to set state properties.

*Example-*

```javascript
class ExampleClassComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
        reader: "Student"
        };
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



Previous [State](state.md) | Next: [State](./state.md)

