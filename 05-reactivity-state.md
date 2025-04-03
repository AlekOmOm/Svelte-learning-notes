# 5. Reactivity and State with `$state` (Svelte 5 Runes)

[<- Back: Svelte Components](./04-svelte-components.md) | [Next: Props ->](./06-props.md)

## Table of Contents

- [Introducing Svelte 5 Runes](#introducing-svelte-5-runes)
- [Using `$state()` for Reactive Variables](#using-state-for-reactive-variables)

## Introducing Svelte 5 Runes

Svelte 5 introduced **Runes**, which are special symbols (like `$state`, `$props`, `$derived`, `$effect`) that provide more explicit control over reactivity. They aim to make Svelte's reactivity model clearer, especially in complex scenarios.

_(Note: These notes focus on the Runes API. If you are using Svelte 4 or earlier, reactivity works differently, primarily through `let` declarations and assignments.)_

## Using `$state()` for Reactive Variables

The `$state()` rune is used to declare **reactive state** within a component's `<script>` block. When the value of a variable declared with `$state()` changes, Svelte knows it needs to potentially update the parts of the component's template that depend on that variable.

**Definition:**

- **`$state(initialValue)`**: A function (rune) that creates and returns a reactive state variable initialized with `initialValue`.

**Explanation:**

- Any variable declared using `$state()` becomes a source of reactivity.
- When you _assign_ a new value to this variable, Svelte schedules an update to the component's output (DOM).

**Example:**

```js
<!-- Counter.svelte -->
<script>
  // Declare a reactive 'count' variable, initialized to 0
  let count = $state(0);

  function increment() {
    // Assigning a new value triggers reactivity
    count = count + 1;
    // Or shorthand: count += 1;
  }

  function decrement() {
    count = count - 1;
    // Or shorthand: count -= 1;
  }
</script>

<p>Current count: {count}</p>

<button on:click={increment}>+</button>
<button on:click={decrement}>-</button>

<style>
  button { margin: 0 5px; }
</style>
```

In this example:

- let `count = $state(0);` creates a reactive variable count.

- The template displays its value: `<p>Current count: {count}</p>`.

- When the increment or decrement functions are called (via button clicks), they assign a new value to count.

- Svelte detects this assignment and automatically updates the <p> tag in the DOM to show the new count.

---

[<- Back: Svelte Components](./04-svelte-components.md) | [Next: Props ->](./06-props.md)
