# Class Component

> Class component extends the React.Component class and have access to React.Component's functions.

> render() function is required in class components, which renders some JSX.

*Example-*

Component:

```javascript
class ExampleClassComponent extends React.Component {
  render() {
    return <h1>Hello, Student</h1>;
  }
}
```

Rendering:

```javascript
ReactDOM.render(
  <ExampleClassComponent />,
  document.getElementById('appDiv')
);
```

Note how we can use the component in the ReactDOM.render() function, and how ```<ExampleClassComponent />``` can be self closing tag.


Previous: [Component](component.md) | Next: [Function Component](functionComponent.md)
