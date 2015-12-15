# Prevent missing displayName in a React component definition (display-name)

DisplayName allows you to name your component. This name is used by React in debugging messages.

## Rule Details

The following patterns are considered warnings:

```js
var Hello = React.createClass({
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});
```

The following patterns are not considered warnings:

```js
var Hello = React.createClass({
  displayName: 'Hello',
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});
```

## Rule Options

```js
...
"display-name": [<enabled>, {
  "acceptTranspilerName": <boolean>,
  "forbidExplicitDefinition": <boolean>
}]
...
```

### `acceptTranspilerName`

When `true` the rule will accept the name set by the transpiler and does not require a `displayName` property in this case.

The following patterns are considered okay and do not cause warnings:

```js
var Hello = React.createClass({
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});
module.exports = Hello;
```

```js
export default class Hello extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}
```

With the following patterns the transpiler can not assign a name for the component and therefore it will still cause warnings:

```js
module.exports = React.createClass({
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});
```

```js
export default class extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}
```

```js
function HelloComponent() {
  return React.createClass({
    render: function() {
      return <div>Hello {this.props.name}</div>;
    }
  });
}
module.exports = HelloComponent();
```

### `forbidExplicitDefinition`

When `true` the rule will not accept any explicitly defined `displayName` property, allowing for the name to **only** be set by the transpiler.

**Note:** Setting this rule to `true` will also set `acceptTranspilerName` to `true`.

The same patterns accepted when `acceptTranspilerName` is `true` will also be accepted when this rule is on.

The following patterns will cause warnings:

```js
var Hello = React.createClass({
  displayName: 'Hello',
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});
```

```js
class Hello extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}
Hello.displayName = 'Hello';
```

```js
class Hello extends React.Component {
  static get displayName() {
    return 'Hello';
  }
  render() {
    return <div>Hello {this.props.name}</div>;
  },
}
```

```js
class Hello extends React.Component {
  static displayName = 'Widget'
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}
```

## About component detection

For this rule to work we need to detect React components, this could be very hard since components could be declared in a lot of ways.

For now we should detect components created with:

* `React.createClass()`
* an ES6 class that inherit from `React.Component` or `Component`
* a stateless function that return JSX or the result of a `React.createElement` call.
