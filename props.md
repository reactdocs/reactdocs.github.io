# Props

> Props is a short form of 'Properties', which are values passed to the component from calling Instance.


*Example-*

##### Class component:

Class components use ```this.props``` to access props.

```javascript
class ExampleClassComponent extends React.Component {
  render() {
    return <h1>Hello, {this.props.reader}</h1>;
  }
}
```

Call:

```javascript
ReactDOM.render(
  <ExampleClassComponent reader="Student"/>,
  document.getElementById('appDiv')
);
```

Note how a prop can be passed using double quotes like HTML attributes.

##### Function component:

Function components use ```props``` to access props.

```javascript
function ExampleFunctionComponent(props) {
  return <h1>Hello, {props.reader}</h1>;
}
```

Note how ```props``` is passed as argument.

Call:

```javascript
const propsItem = "Student";
ReactDOM.render(
  <ExampleFunctionComponent reader={propsItem} />,
  document.getElementById('appDiv')
);
```

Note how a prop can be passed using Javascript inside parenthesis.

## Props are Read-Only

Calling parent component decides enclosed component's props. A component should never attempt to change its props, just should read it.

