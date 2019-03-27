# work-work-react
==============

My life for the Horde.

### React

What Is React?

React is a declarative, efficient, and flexible JavaScript library for building user interfaces. It lets you compose complex UIs from small and isolated pieces of code called “components”.

React has a few different kinds of components, but we’ll start with `React.Component` subclasses:

```
class Example extends React.Component {
  render() {
    return (
      <div className="example">
        <h1>Example for {this.props.name}</h1>
        <button className="example-two" onClick={() => alert('click')}>
          {this.props.value}
        </button>
      </div>
    );
  }
}
```

We use components to tell React what we want to see on the screen. When our data changes, React will efficiently update and re-render our components.

Here, Example is a `React component class`, or `React component type`. A component takes in parameters, called `props` (short for “properties”), and returns a hierarchy of views to display via the `render` method.

The `render` method returns a description of what you want to see on the screen. React takes the description and displays the result. In particular, `render` returns a `React element`, which is a lightweight description of what to render. Most React developers use a special syntax called “JSX” which makes these structures easier to write. The `<div />` syntax is transformed at build time to `React.createElement('div')`.

The example above is equivalent to:

return React.createElement('div', {className: 'example'},
  React.createElement('h1', null, "Example for ", props.name),
  React.createElement('button', {className: 'example-two', onClick: alert('click'), props.value},
);

JSX comes with the full power of JavaScript. You can put any JavaScript expressions within braces inside JSX. Each React element is a JavaScript object that you can store in a variable or pass around in your program.

The `Example` component above only renders built-in DOM components like 1<div />1. But you can compose and render custom React components too. For example, we can now refer to the whole example by writing `<Example />`. Each React component is encapsulated and can operate independently; this allows you to build complex UIs from simple components.

Passing props is how information flows in React apps, from parents to children.

Notice how with onClick={() => alert('click')}, we’re passing a function as the onClick prop. React will only call this function after a click. Forgetting () => and writing onClick={alert('click')} is a common mistake, and would fire the alert every time the component re-renders.

React components can have state by setting `this.state` in their constructors. `this.state` should be considered as private to a React component that it’s defined in. Let’s store the current value of the button in this.state, and change it when the button is clicked.

```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.sate = {
      value: null
    };
  }
  
  render() {
    return (
      <div className="example">
        <h1>Example for {this.props.name}</h1>
        <button className="example-two" onClick={() => this.setState({value: `X`})}>
          {this.state.value}
        </button>
      </div>
    );
  }
}
```

In `JavaScript classes`, you need to always call `super` when defining the constructor of a subclass. All React component classes that have a `constructor` should start it with a `super(props)` call.

By calling `this.setState` from an `onClick` handler in the Example’s `render` method, we tell React to re-render that Example whenever its `<button>` is clicked. After the update, the Example’s this.state.value will be 'X', so we’ll see the X on the button. If you click on the button, an X should show up.

When you call `setState` in a component, React automatically updates the child components inside of it too.

To collect data from multiple children, or to have two child components communicate with each other, you need to declare the shared state in their parent component instead. The parent component can pass the state back down to the children by using props; this keeps the child components in sync with each other and with the parent component.

Lifting state into a parent component is common when React components are refactored.

The DOM `<button>` element’s `onClick` attribute has a special meaning to React because it is a built-in component. For custom components like Example, the naming is up to you. We could give any name to the button’s `onClick` prop or Example’s `handleClick` method, and the code would work the same. In React, it’s conventional to use `on[Event]` names for props which represent events and `handle[Event]` for the methods which handle the events.

There are generally two approaches to changing data. The first approach is to mutate the data by directly changing the data’s values. The second approach is to replace the data with a new copy which has the desired changes.

Data Change with Mutation

```
var player = {score: 1, name: 'Jeff'};
player.score = 2;
// Now player is {score: 2, name: 'Jeff'}
```

Data Change without Mutation

```
var player = {score: 1, name: 'Jeff'};

var newPlayer = Object.assign({}, player, {score: 2});
// Now player is unchanged, but newPlayer is {score: 2, name: 'Jeff'}

// Or if you are using object spread syntax proposal, you can write:
// var newPlayer = {...player, score: 2};
```

The end result is the same but by not mutating (or changing the underlying data) directly, we gain several benefits described below.

Complex Features Become Simple

Immutability makes complex features much easier to implement. Later in this tutorial, we will implement a “time travel” feature that allows us to review the tic-tac-toe game’s history and “jump back” to previous moves. This functionality isn’t specific to games — an ability to undo and redo certain actions is a common requirement in applications. Avoiding direct data mutation lets us keep previous versions of the game’s history intact, and reuse them later.

Detecting Changes

Detecting changes in mutable objects is difficult because they are modified directly. This detection requires the mutable object to be compared to previous copies of itself and the entire object tree to be traversed.

Detecting changes in immutable objects is considerably easier. If the immutable object that is being referenced is different than the previous one, then the object has changed.

Determining When to Re-Render in React

The main benefit of immutability is that it helps you build pure components in React. Immutable data can easily determine if changes have been made which helps to determine when a component requires re-rendering.

Function Components

In React, **function components** are a simpler way to write components that only contain a `render` method and don’t have their own state. Instead of defining a class which extends `React.Component`, we can write a function that takes `props` as input and returns what should be rendered. Function components are less tedious to write than classes, and many components can be expressed this way.
