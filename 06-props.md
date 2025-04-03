# 6. Component Props with `$props` (Svelte 5 Runes) 🔄

[<- Back: Reactivity & State](./05-reactivity-state.md) | [Next: Routing ->](./07-routing-svelte-navigator.md)

## Table of Contents

- [Introducing `$props()`](#introducing-props)
- [Example](#example)
- [How it Works](#how-it-works)

**Props** (short for properties) are a way to pass data **down** from a parent component to a child component. This allows you to create reusable components that can be configured differently depending on where they are used. 📫

## Introducing `$props()` 📦

In Svelte 5 with Runes enabled, the `$props()` rune is the standard way for a component to declare and access the props passed to it by its parent.

**Definition:**

- **`$props()`**: A function (rune) called within a component's `<script>` block that returns an _object_ containing all the props passed to that component instance.

**Explanation:**

- You typically use destructuring assignment with `$props()` to extract the specific props your component expects. 🧩
- You can provide default values for props in case the parent doesn't pass them. 🎁
- Props are generally treated as **read-only** within the child component. If a child needs to modify the data, it should typically manage its own state or emit events back up to the parent. 🔒

## Example 🧩

Let's create a `Greeting.svelte` component that accepts a `name` prop.

**Child Component (`src/lib/Greeting.svelte`):**

```svelte
<!-- src/lib/Greeting.svelte -->
<script>
  // Use $props() to get the props object
  // Destructure 'name' from the props object
  // Provide a default value 'Stranger' if 'name' is not passed
  const { name = "Stranger" } = $props();
</script>

<h2>Hello, {name}! 👋</h2>

<style>
  h2 {
    color: steelblue;
  }
</style>
```

**Parent Component (`src/App.svelte`):**

```svelte
<!-- src/App.svelte -->
<script>
  import Greeting from './lib/Greeting.svelte';

  let userName = $state("Alice"); // Reactive state in the parent
</script>

<h1>Welcome to the App 🏠</h1>

<!-- Use the Greeting component and pass the 'name' prop -->
<Greeting name={userName} />

<!-- Use the Greeting component without passing the 'name' prop -->
<!-- It will use its default value "Stranger" -->
<Greeting />

<hr/>
<label>
  Update Name: ✏️
  <input bind:value={userName} />
</label>
```

## How it Works ⚙️

- In `App.svelte`, we import `Greeting`.

- We use the `Greeting` component twice:

  - In the first instance (`<Greeting name={userName} />`), we pass the current value of the `userName` state variable (initially "Alice") as the `name` prop. 🧒
  - In the second instance (`<Greeting />`), we don't pass a name prop. 🎭

- Inside `Greeting.svelte`, `$props()` makes the passed props available.

  - `const { name = "Stranger" } = $props();` extracts the `name` prop. If name was passed (like in the first instance), it uses that value ("Alice"). If not (like in the second instance), it uses the default value "Stranger".

- When `userName` changes in `App.svelte` (e.g., via the input field), Svelte automatically passes the updated value down to the first Greeting component, which then re-renders with the new name. ✨

---

[<- Back: Reactivity & State](./05-reactivity-state.md) | [Next: Routing ->](./07-routing-svelte-navigator.md)
