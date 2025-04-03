# 4. Svelte Component Basics

[<- Back: Project Structure](./03-project-structure.md) | [Next: Reactivity & State ->](./05-reactivity-state.md)

## Table of Contents

- [Structure of a `.svelte` File](#structure-of-a-svelte-file)
- [Key Concepts](#key-concepts)

Svelte applications are built from **components**.

- A component is a reusable piece of UI, typically defined in a `.svelte` file.

content

- [.svelte](#structure-of-a-svelte-file) files contain the component's logic, markup, and styles.
- [Key Concepts](#key-concepts) in Svelte include template syntax, event handling, and scoped styles.

## Structure of a `.svelte` File

A `.svelte` file usually consists of three sections:

1.  **`<script>` block:** Contains the component's JavaScript logic, including state, props, and lifecycle functions.
2.  **Template (HTML-like markup):** Defines the structure and content of the component's output.
3.  **`<style>` block (optional):** Contains CSS styles scoped specifically to this component.

```js
<!-- ExampleComponent.svelte -->
<script>
  // JavaScript logic goes here
  let name = $state('World'); // Using Svelte 5 Runes state

  function handleClick() {
    alert('Button clicked!');
  }
</script>

<!-- HTML-like template -->
<main>
  <h1>Hello {name}!</h1>
  <p>This is a Svelte component.</p>
  <button on:click={handleClick}>Click Me</button>
</main>

<!-- Scoped CSS -->
<style>
  main {
    padding: 1em;
    background-color: #f0f0f0;
    border-radius: 5px;
  }
  h1 {
    color: rebeccapurple;
  }
  /* These styles only apply to elements within this component */
</style>
```

## **_Key Concepts_**

- Template Syntax ({expression}): You can embed JavaScript expressions directly into the HTML markup using curly braces {}. Svelte will automatically update the DOM when the values of these expressions change.

  Example: <h6>Hello {name}!</h1>

  ```html
  <h6>Hello {name}!</h1>
  ```

- Event Handling (on:event): You can listen to DOM events using the on: directive followed by the event name (e.g., on:click, on:input).

  Example:
  <button on:click={handleClick}>Click Me</button>

  ```html
  <button on:click="{handleClick}">Click Me</button> // assigns the handleClick
  function to run when the button is clicked.
  ```

- Scoped Styles:
  - CSS rules defined within the `<style>` block are automatically scoped to the component by default.
  - Svelte adds unique class names to elements and modifies your CSS selectors during compilation, preventing styles from leaking out and affecting other components.

[<- Back: Project Structure](./03-project-structure.md) | [Next: Reactivity & State ->](./05-reactivity-state.md)
