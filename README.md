# React Composition - HOC's, Render props, Hooks and Context

It is easy to share logic between React components, as long as it doesn't involve state. Stateful logic is tied deeply to the component itself. This is a problem for logic that we wish to share between many components. Our example today will be a toggle.

Simple enough, right? All we need is a boolean in state, and a function to flip. However, to avoid writing that a million times we embark upon a significant journey with many possible outcomes.

There are a number of common patterns

- Higher Order Components 

The goal of this pattern is to wrap our view layer inside a pre-built and re-usable component that contains our stateful layer. 

```js
const ToggleComponent = withToggle(StatelessToggleComponent)
```

`withToggle` creates a new component wherein `StatelessToggleComponent` receives the props `isOn` and `toggle`, or equivalents, from a parent component.

- Render Props

```
<ToggleComponent>
  {({ isOn, toggle}) => (
    <button onClick={toggle}>
      {inOn ? 'On' : 'off'}
    </button>
  )}
</ToggleComponent>
```

Render props (in this case, render children) follow a similar pattern, but can be a bit more flexible. We provide a function, on the fly, that takes the toggle functionality as arguments.

- Hooks

```
const ToggleComponent () => {
  ...
  const { isOn, toggle } = useToggle();
  ...
  return (
    <button onClick={toggle}>
      {inOn ? 'On' : 'off'}
    </button>
  );
}
```

Hooks are a relatively new method for composing stateful logic now that functional components can hold state. This allows us to directly share the state and logic between components without wrapping our component.

- Context

```
const ToggleButton () => {
  const { isOn, toggle } = useToggleContext();

  return (
    <button onClick={toggle}>
      {inOn ? 'On' : 'off'}
    </button>
  );
}
const WhileOn ({ children }) => {
  const { isOn } = useToggleContext();
  if (isOn) {
   return children;
  }
  return null;
}

...
<ToggleProvider>
  <ToggleButton />
  ....
  <WhileOn>
    The toggle is on
  </WhileOn>
</ToggleProvider>
```

While Context has existed longer than hooks it is a bit more ergonomic. (Previously, you had to use render props.) This allows us to compose and _share_ the state and logic with many components at once.

This involves some amount of DOM manipulation, which is a good thing! We can't forget that React exists in our web browser, our vanilla DOM and JS knowledge is still valuable, and React cannot abstract *everything* away.

## Goals

* [ ] Create a class based Toggle component
* [ ] Create a function based Toggle component
* [ ] Share this logic with a Higher Order Component
* [ ] Share this logic with Render Props
* [ ] Share this logic with a Custom Hook
* [ ] Share this logic with Context

## Stretch Goals

* [ ] Understand why the React Router has both a `component` and a `render` prop. It may help to take a look at the source code. Or, google it.

## Notes

* Another pattern may have occurred to you. A shockingly simple pattern. What if we isolate all of our logic from our `setState`, or equivalent, functions. What if we remove those side-effects and have our logic simply take the previous state, and output the new state? A pure, functional system. Then we won't have to worry at all about the specifics of React. 

Sounds wonderful, right? Well, you just invented the reducer pattern. We won't talk about it today, but we will revisit it with a later project on `useReducer` and `Redux`.
