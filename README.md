
# Svelte Learning Notes (with Vite and Svelte 5 Runes)

my collection of notes on basics of Svelte, with focus on:

-  project setup (Vite)
-  Runes API ($state, $props)

## Learning Path

1.  **[Introduction to Svelte](./01-introduction.md)**
    *   What is Svelte?
    *   Key differences (Compiler vs. Framework)
    *   Why choose Svelte?

2.  **[Project Setup with Vite](./02-project-setup-vite.md)**
    *   Prerequisites (Node.js)
    *   Creating a new Svelte + Vite project
    *   Running the development server

3.  **[Understanding the Project Structure](./03-project-structure.md)**
    *   Overview of the default directories and files
    *   Purpose of key files (`index.html`, `main.js`, `App.svelte`, `vite.config.js`, `svelte.config.js`)

4.  **[Svelte Component Basics](./04-svelte-components.md)**
    *   The structure of a `.svelte` file (`<script>`, template, `<style>`)
    *   Basic template syntax (`{expression}`)
    *   Handling events (`on:event`)

5.  **[Reactivity and State with `$state`](./05-reactivity-state.md)**
    *   What is reactivity?
    *   Introducing Svelte 5 Runes
    *   Using `$state()` for reactive variables

6.  **[Component Props with `$props`](./06-props.md)**
    *   What are props?
    *   Passing data down to child components
    *   Using `$props()` to receive props (Svelte 5 Runes)

7.  **[Client-Side Routing with svelte-navigator](./07-routing-svelte-navigator.md)**
    *   What is client-side routing?
    *   Introduction to `svelte-navigator`
    *   Installation and basic usage (linking concepts, not a full tutorial yet)

---
*(These notes assume you are using Svelte 5 or later, which introduced the Runes API like `$state` and `$props`.)*

---
## dir
```markdown
svelte-learning-notes/
├── lesson-note.md                  # Main entry point / Table of Contents
├── 01-introduction.md              # What is Svelte? Why use it?
├── 02-project-setup-vite.md        # Setting up a Svelte project with Vite
├── 03-project-structure.md         # Understanding the files and folders
├── 04-svelte-components.md         # Basic structure of a .svelte file
├── 05-reactivity-state.md          # Understanding $state() (Svelte 5 Runes)
├── 06-props.md                     # Understanding $props() (Svelte 5 Runes)
├── 07-routing-svelte-navigator.md  # Introduction to routing with svelte-navigator
└── README.md                       # (Optional) Overall description
```

