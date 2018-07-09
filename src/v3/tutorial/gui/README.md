# GUI

OS.js uses [Hyperapp](https://hyperapp.js.org/) for its GUI components by default.

*This does not mean that you are restricted to usage of Hyperapp. You can use React, Vue or anything you like.*


## Basic Example

![Basic Example](example-1.png)

```javascript
import {h, app} from 'hyperapp';

const createView = (state, actions) => h('div', {}, [
  h('div', {}, String(state.counter)),
  h('button', {type: 'button', onclick: () => actions.increment()}, 'Increment counter')
]);

const createApp = (proc, win, $content) => {
  app({
    counter: 0
  }, {
    increment: () => state => ({counter: state.counter + 1})
  }, createView, $content);
};

// ...
win.render($content => createApp(proc, win, $content));

// Or if you've chained your calls when creating the window:

proc.createWindow(/* ... */)
  .render(($content, win) => createApp(proc, win, $content));
// ...
```

### Using Components

![Basic Example](example-2.png)

Use the provided GUI module:

```javascript
import {Button} from '@osjs/gui';

const createView = (state, actions) => h('div', {}, [
  h('div', {}, String(state.counter)),
  h(Button, {onclick: () => actions.increment(), label: 'Increment counter'})
]);
```

### JSX

You can also use JSX syntax instead of the programatic approach.

In your `webpack.js` file:

```
module.exports = (options, {createWebpack}) => createWebpack(__dirname, {
  jsx: {
    pragma: 'h'
  }
});
```

and then in your source:

```javascript
const createView = (state, actions) => (
  <div>
    <div>{state.counter}</div>
    <button type="button" onclick={() => actions.increment()}>Increment counter</button>
  </div>
);
```

## Components

This is the list of standard GUI components.

All components uses Flexbox and supports the following props:

* `orientation` - Orientation of children (horizontal/vertical)
* `grow` - Grow factor (default `0`)
* `shrink` - Shrink factor (default `0`)
* `basis` - Base size (default `auto`)
* `align` - Align items
* `justify` - Justify content
* `margin / padding` - Padding/Margin (boolean/string/number for `Box`)

> Notes:
> 1. Non-container elements gets the flexbox model from the `box` property.
> 2. All components based on browser elements supports the standard properties.
>
> *The component set is under development*
>

### Containers

| Name                | Description                       | Custom Props                                      |
| ------------------- | --------------------------------- | ------------------------------------------------- |
| Box                 | Flexbox container (padded)        | *See above*                                       |
| BoxContainer        | Flexbox container (simple)        | *See above*                                       |
| Toolbar             | Flexbox container (spaced)        | *See above*                                       |
| Menubar             | Toolbar, except for menus         |                                                   |
| Tabs                | Tabbed container(s)               | `{labels: [String,...]}`                          |
| Panes               | Resizable container(s)            | `{orientation: String, sizes: Array[Number,...]}` |

### Fields

All fields have extended events that passes on the current value:

```
onchange: (event, value) => {}
oninput: (event, value) => {}
```

| Name                | Description                                 | Custom Props                    |
| ------------------- | ------------------------------------------- | ------------------------------- |
| Button              | `<button>` Element                          | `{label: String, icon: String}` |
| RangeField          | `<input type="range" />` Field              |                                 |
| TextField           | `<input type="text,password,..." />` Field  |                                 |
| ToggleField         | `<input type="checkbox,radio" />`           |                                 |
| SelectField         | `<select>` Field                            | `{choices: Map<*, *>}`          |
| TextareaField       | `<textarea>` Field                          |                                 |

### Views

| Name                | Description                        | Custom Props |
| ------------------- | ---------------------------------- | ------------ |
| ListView            | A listview (not quite done)        |              |
| Iframe              | `<iframe>` View                    |              |
| Statusbar           | A statusbar with label             |              |

### Other

| Name                | Description                        | Custom Props           |
| ------------------- | ---------------------------------- | ---------------------- |
| Progressbar         | A progressbar with label           | `{value: Number}`      |
| Image               | `<img />` element                  |                        |
| Video               | `<video />` element                |                        |