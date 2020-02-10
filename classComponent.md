# Component

> Component is an independent piece that generates some JSX (which generates HTML eventually), used in the react code.

> There are 2 types of components: Class Components and Function Components.

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
<br/><br/>
<span style="background-color: #FFFF00; padding:5px;">
Component names should start with Capital letters for react to differentiate them from HTML elements.
</span>

