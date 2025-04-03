# 5. Reactivity and State with Svelte 5 Runes ⚡

[<- Back: Svelte Components](./04-svelte-components.md) | [Next: Props ->](./06-props.md)

## Table of Contents

- [Introducing Svelte 5 Runes](#introducing-svelte-5-runes-✨)
- [`$state()` for Reactive Variables](#using-state-for-reactive-variables-🔄)
- [`$props()` for Component Properties](#working-with-props-for-component-properties-🔄)
- [`$derived()` for Computed Values](#using-derived-for-computed-values-🧮)
- [`$effect()` for Side Effects](#creating-side-effects-with-effect-⚡)

## Introducing Svelte 5 Runes ✨

Svelte 5 introduced **Runes**, which are special symbols (like `$state`, `$props`, `$derived`, `$effect`) that provide more explicit control over reactivity. They aim to make Svelte's reactivity model clearer, especially in complex scenarios.

_(Note: These notes focus on the Runes API. If you are using Svelte 4 or earlier, reactivity works differently, primarily through `let` declarations and assignments.)_

## Using `$state()` for Reactive Variables 🔄

The `$state()` rune is used to declare **reactive state** within a component's `<script>` block. When the value of a variable declared with `$state()` changes, Svelte knows it needs to potentially update the parts of the component's template that depend on that variable.

**Definition:**

- **`$state(initialValue)`**: A function (rune) that creates and returns a reactive state variable initialized with `initialValue`.

**Explanation:**

- Any variable declared using `$state()` becomes a source of reactivity.
- When you _assign_ a new value to this variable, Svelte schedules an update to the component's output (DOM).

**Example:** 🧪

--

Counter.svelte

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

<main>
  <p>Current count: {count}</p>

<button on:click={increment}>+</button>
<button on:click={decrement}>-</button>

</main>

<style>
  button { margin: 0 5px; }
  main {
    padding: 1em;
    background-color: grey;
    border-radius: 5px;
    width: fit-content;
    text-align: left;
  }
</style>

--

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
<main>
  <p>Current count: {count}</p>

<button on:click={increment}>+</button>
<button on:click={decrement}>-</button>

</main>

<style>

  button { margin: 0 5px; }
  main {
    padding: 1em;
    background-color: grey;
    border-radius: 5px;
    text-align: left;
  }

</style>
```

In this example:

- `let count = $state(0);` creates a reactive variable `count`. 📊

- The template displays its value: `<p>Current count: {count}</p>`. 👀

- When the increment or decrement functions are called (via button clicks), they assign a new value to `count`. 🔼🔽

- Svelte detects this assignment and automatically updates the `<p>` tag in the DOM to show the new count. ✅

## Working with `$props()` for Component Properties 🔄

The `$props()` rune allows components to accept external data through properties. It provides a clean and explicit way to define and access the props that a component receives from its parent.

**Definition:**

- **`$props()`**: A function that defines component properties and returns an object containing them.

**Explanation:**

- The `$props()` rune defines the interface of your component - what data it can receive from parent components.
- Props are reactive - when a parent component updates a prop value, the child component automatically re-renders with the new value.
- Props can include default values and type definitions.

**Example:** 🧪

```js
<!-- Greeting.svelte -->
<script>
  // Define component props with defaults
  const { name = "World", color = "blue" } = $props();

  // You can use props like any other variable
  let message = $state(`Hello, ${name}!`);

  function updateGreeting() {
    message = `Hey there, ${name}!`;
  }
</script>

<div style="color: {color}">
  <p>{message}</p>
  <button on:click={updateGreeting}>Change greeting</button>
</div>

<!-- In a parent component: -->
<!--
<Greeting name="Alice" color="purple" />
-->
```

In this example:

- `const { name = "World", color = "blue" } = $props();` defines two props with default values. 📦
- The props can be used throughout the component like regular variables. 🔄

- When a parent component includes `<Greeting name="Alice" color="purple" />`, the values passed override the defaults. 🎨

- If the parent component updates these values, the Greeting component will automatically re-render. ✨

## Using `$derived()` for Computed Values 🧮

The `$derived()` rune creates values that automatically update when their dependencies change. It's perfect for computed properties that depend on other reactive values.

**Definition:**

- **`$derived(expression)`**: A function that returns a computed value based on the provided expression, which automatically updates when any dependencies in the expression change.

**Explanation:**

- Derived values are recalculated whenever their dependencies (other reactive values used in the expression) change.
- They are read-only - you cannot directly assign to a derived value.
- They are perfect for calculations, transformations, or any value that depends on other state.

**Example:** 🧪

```js
<!-- TaxCalculator.svelte -->
<script>
  let price = $state(100);
  let taxRate = $state(0.1); // 10% tax

  // This will automatically update when price or taxRate changes
  let taxAmount = $derived(price * taxRate);
  let totalPrice = $derived(price + taxAmount);

  function updatePrice(event) {
    price = Number(event.target.value);
  }

  function updateTaxRate(event) {
    taxRate = Number(event.target.value) / 100;
  }
</script>

<div>
  <label>
    Item Price: $
    <input type="number" value={price} on:input={updatePrice} min="0" />
  </label>

  <label>
    Tax Rate: %
    <input type="number" value={taxRate * 100} on:input={updateTaxRate} min="0" max="100" />
  </label>

  <p>Tax Amount: ${taxAmount.toFixed(2)}</p>
  <p>Total Price: ${totalPrice.toFixed(2)}</p>
</div>
```

In this example:

- `price` and `taxRate` are reactive state variables that can be updated by user input. 💰
- `taxAmount` is a derived value that recalculates whenever `price` or `taxRate` changes. 🧮
- `totalPrice` is also derived, and it updates whenever `price` or `taxAmount` changes. 💵
- The UI always shows the current calculated values without needing explicit update logic. ✅

## Creating Side Effects with `$effect()` ⚡

The `$effect()` rune lets you run side effects whenever reactive dependencies change. It's useful for tasks like updating the DOM, interacting with browser APIs, or any operation that should happen in response to state changes.

**Definition:**

- **`$effect(callback)`**: A function that executes the provided callback whenever any reactive values accessed within the callback change.

**Explanation:**

- The callback function runs once when the component is initialized.
- It then automatically runs again whenever any reactive values it references are updated.
- Perfect for side effects like DOM manipulation, API calls, or synchronizing with external systems.

**Example:** 🧪

```js
<!-- LocalStorage.svelte -->
<script>
  let name = $state(localStorage.getItem('username') || '');

  // This effect runs when 'name' changes
  $effect(() => {
    // Save to localStorage whenever name changes
    if (name) {
      localStorage.setItem('username', name);
      console.log('Username saved:', name);
    }
  });

  function handleInput(event) {
    name = event.target.value;
  }
</script>

<div>
  <h2>User Profile</h2>
  <label>
    Your Name:
    <input type="text" value={name} on:input={handleInput} />
  </label>

  {#if name}
    <p>Hello, {name}! Your name is saved automatically.</p>
  {:else}
    <p>Please enter your name.</p>
  {/if}
</div>
```

In this example:

- We initialize `name` from localStorage or as an empty string. 📄
- The `$effect()` watches the `name` variable and saves it to localStorage whenever it changes. 💾
- When the user types in the input field, the effect automatically triggers. ⌨️
- This creates persistent state that survives page refreshes, all without explicit event handling for the save operation. 🔄
- The side effect (saving to localStorage) is cleanly separated from the state management. 🧩

---

[<- Back: Svelte Components](./04-svelte-components.md) | [Next: Props ->](./06-props.md)
