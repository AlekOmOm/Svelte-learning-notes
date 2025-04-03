# 3. Understanding the Project Structure

[<- Back: Project Setup](./02-project-setup-vite.md) | [Next: Svelte Components ->](./04-svelte-components.md)

## Table of Contents

- [Purpose of Key Files](#purpose-of-key-files)

When you create a Svelte project with Vite, you get a standard set of files and folders. Here's a breakdown of the important ones:

```markdown
my-svelte-app/
├── node_modules/ # Stores project dependencies (managed by npm)
├── public/ # Static assets copied directly to the build output
│ └── vite.svg # Example static asset
├── src/ # Your application's source code
│ ├── app.css # Global CSS styles (can be imported in main.js or App.svelte)
│ ├── assets/ # Assets processed by Vite (e.g., images, fonts)
│ │ └── svelte.svg # Example processed asset
│ ├── lib/ # Recommended place for reusable components, utilities, stores
│ │ └── Counter.svelte # Example component
│ ├── App.svelte # The main/root Svelte component
│ └── main.js # The application entry point (initializes Svelte)
├── .gitignore # Files/folders for Git to ignore
├── index.html # The main HTML page template (Vite entry point)
├── package.json # Project metadata and dependencies
├── package-lock.json # Records exact dependency versions
├── svelte.config.js # Svelte compiler configuration
├── vite.config.js # Vite build tool configuration
└── README.md # Project documentation
```

## Purpose of Key Files

- **`index.html`**:

  - The main HTML file served to the browser.
  - Contains a `<body>` tag, often with a `<div id="app"></div>`.
  - Includes a `<script type="module" src="/src/main.js"></script>` tag, which tells Vite where your JavaScript application starts.

- **`src/main.js`**:

  - The JavaScript entry point for your application.
  - It imports the root Svelte component (`App.svelte`).
  - It creates a new instance of the `App` component and tells it where to render in the DOM (targeting the `#app` div from `index.html`).

  ```js
  // src/main.js
  import "./app.css"; // Optional: Import global styles
  import App from "./App.svelte";

  const app = new App({
    target: document.getElementById("app"),
  });

  export default app;
  ```

- **`src/App.svelte`**:

  - The top-level Svelte component of your application.
  - This is where you typically start building your UI by composing other components.

- **`src/lib/`**:

  - A conventional directory to store reusable parts of your application, such as:
    - Shared Svelte components (`.svelte` files)
    - Utility functions (`.js` files)
    - Svelte stores (for state management)
  - You can import from here using aliases like `import MyComponent from '$lib/MyComponent.svelte';` (this requires configuration, often default in SvelteKit, but might need setup in plain Vite/Svelte). Or just use relative paths: `import Counter from './lib/Counter.svelte';`.

- **`vite.config.js`**:

  - Configuration file for the Vite build tool.
  - Used to add plugins (like the Svelte plugin), configure the dev server, set up build options, define aliases, etc.

  ```js
  // vite.config.js
  import { defineConfig } from "vite";
  import { svelte } from "@sveltejs/vite-plugin-svelte";

  // https://vitejs.dev/config/
  export default defineConfig({
    plugins: [svelte()], // Tells Vite how to handle .svelte files
  });
  ```

- **`svelte.config.js`**:

  - Configuration file specifically for the Svelte compiler.
  - Used to define preprocessors (like for TypeScript or SCSS), compiler options, etc. Svelte 5 Runes mode is typically enabled by default or via this file.

  ```js
  // svelte.config.js
  import { vitePreprocess } from "@sveltejs/vite-plugin-svelte";

  export default {
    // Consult https://svelte.dev/docs#compile-time-svelte-preprocess
    // for more information about preprocessors
    preprocess: vitePreprocess(),
    compilerOptions: {
      runes: true, // Ensure Runes mode is enabled (often default in new projects)
    },
  };
  ```

[<- Back: Project Setup](./02-project-setup-vite.md) | [Next: Svelte Components ->](./04-svelte-components.md)
