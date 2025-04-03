# 8. Data Flow Between Components 🔄

[<- Back: Routing](./07-routing-svelte-navigator.md) | [Back to Main Note ->](./README.md)

## Table of Contents

- [Imports and Exports](#imports-and-exports)
- [Component Communication Patterns](#component-communication-patterns)
- [Working with Utility Functions](#working-with-utility-functions)
- [Stores for Global State](#stores-for-global-state)
- [Context API for Component Trees](#context-api-for-component-trees)

## Imports and Exports 📦

In a Svelte application, components, utilities, and other modules need to be imported before they can be used. This follows standard JavaScript module syntax.

### Importing Components

```js
// Importing a Svelte component
import Header from "./Header.svelte";
import { SidePanel, Footer } from "./layout/index.js";
import UserProfile from "$lib/components/UserProfile.svelte"; // Using path alias
```

### Exporting from Utility Files

```js
// utils/formatters.js
export function formatDate(date) {
  return new Date(date).toLocaleDateString();
}

export function formatCurrency(amount) {
  return new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD",
  }).format(amount);
}

// Default export
export default function formatText(text, maxLength = 100) {
  return text.length > maxLength ? text.substring(0, maxLength) + "..." : text;
}
```

### Importing Utilities

```js
// Importing named exports
import { formatDate, formatCurrency } from "../utils/formatters.js";

// Importing default export
import formatText from "../utils/formatters.js";

// Importing both named and default exports
import formatText, { formatDate } from "../utils/formatters.js";
```

## Component Communication Patterns 💬

Svelte provides several ways for components to communicate and share data:

### 1. Props (Parent to Child) 👇

Props are the primary way to pass data from parent to child components.

```svelte
<!-- Parent.svelte -->
<script>
  import Child from './Child.svelte';
  let userData = $state({ name: 'Alice', role: 'Admin' });
</script>

<Child user={userData} theme="dark" />
```

```svelte
<!-- Child.svelte -->
<script>
  const { user, theme = 'light' } = $props();
</script>

<div class="card {theme}">
  <h2>{user.name}</h2>
  <p>Role: {user.role}</p>
</div>
```

### 2. Events (Child to Parent) 👆

Components can dispatch custom events to communicate upward.

```svelte
<!-- Child.svelte -->
<script>
  import { createEventDispatcher } from 'svelte';

  // Create a dispatcher function
  const dispatch = createEventDispatcher();

  function handleClick() {
    // Dispatch a custom event with optional data
    dispatch('message', {
      text: 'Hello from child!',
      timestamp: new Date()
    });
  }
</script>

<button on:click={handleClick}>Send Message to Parent</button>
```

```svelte
<!-- Parent.svelte -->
<script>
  import Child from './Child.svelte';

  let lastMessage = $state('No messages yet');

  function handleMessage(event) {
    // Access the custom event data
    const { text, timestamp } = event.detail;
    lastMessage = `${text} (${timestamp.toLocaleTimeString()})`;
  }
</script>

<Child on:message={handleMessage} />
<p>Last message: {lastMessage}</p>
```

### 3. Bind: Directive (Two-way Binding) 🔄

For select cases, `bind:` can create two-way data binding between parent and child.

```svelte
<!-- Parent.svelte -->
<script>
  import ProfileEditor from './ProfileEditor.svelte';

  let userProfile = $state({
    name: 'Alex',
    email: 'alex@example.com'
  });
</script>

<!-- Changes in the child will update the parent's userProfile -->
<ProfileEditor bind:profile={userProfile} />
<pre>{JSON.stringify(userProfile, null, 2)}</pre>
```

```svelte
<!-- ProfileEditor.svelte -->
<script>
  // When using bind:, declare the prop as a writable value
  let { profile } = $props();
</script>

<div>
  <label>
    Name:
    <input bind:value={profile.name} />
  </label>
  <label>
    Email:
    <input bind:value={profile.email} />
  </label>
</div>
```

## Working with Utility Functions 🛠️

Utility functions help keep your code DRY (Don't Repeat Yourself) and organized. They're typically pure JavaScript functions that can be used across multiple components.

### Creating a Utility File

```js
// utils/validation.js
export function validateEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}

export function validatePassword(password) {
  // At least 8 chars, 1 uppercase, 1 lowercase, 1 number
  const regex = /^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}$/;
  return regex.test(password);
}
```

### Using Utilities in Components

```svelte
<!-- SignupForm.svelte -->
<script>
  import { validateEmail, validatePassword } from '../utils/validation.js';

  let email = $state('');
  let password = $state('');

  // Computed values using $derived
  let isEmailValid = $derived(validateEmail(email));
  let isPasswordValid = $derived(validatePassword(password));
  let canSubmit = $derived(isEmailValid && isPasswordValid);

  function handleSubmit() {
    // Submit the form
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <div>
    <label>
      Email:
      <input type="email" bind:value={email} />
    </label>
    {#if email && !isEmailValid}
      <p class="error">Please enter a valid email.</p>
    {/if}
  </div>

  <div>
    <label>
      Password:
      <input type="password" bind:value={password} />
    </label>
    {#if password && !isPasswordValid}
      <p class="error">Password must be at least 8 characters with uppercase, lowercase, and number.</p>
    {/if}
  </div>

  <button type="submit" disabled={!canSubmit}>Sign Up</button>
</form>
```

## Stores for Global State 🌎

When you need to share state across components that aren't directly related (not in a parent-child relationship), Svelte provides a built-in store system.

### Creating a Store

```js
// stores/userStore.js
import { writable } from "svelte/store";

// Create a writable store with initial value
export const user = writable({
  id: null,
  name: "",
  isAuthenticated: false,
});

// Optional: Add helper functions
export function login(userData) {
  user.update((current) => ({
    ...current,
    ...userData,
    isAuthenticated: true,
  }));
}

export function logout() {
  user.set({
    id: null,
    name: "",
    isAuthenticated: false,
  });
}
```

### Using Stores in Components

```svelte
<!-- Navbar.svelte -->
<script>
  import { user, logout } from '../stores/userStore.js';
</script>

<nav>
  <div class="logo">MyApp</div>

  {#if $user.isAuthenticated}
    <span>Welcome, {$user.name}</span>
    <button on:click={logout}>Log Out</button>
  {:else}
    <a href="/login">Log In</a>
  {/if}
</nav>
```

```svelte
<!-- LoginForm.svelte -->
<script>
  import { login } from '../stores/userStore.js';

  let username = $state('');
  let password = $state('');

  async function handleSubmit() {
    try {
      // Assume this function makes an API call
      const userData = await authenticateUser(username, password);
      login(userData);
      // Redirect after login
    } catch (error) {
      // Handle error
    }
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <!-- Form inputs -->
</form>
```

## Context API for Component Trees 🌲

The Svelte context API provides a way to share data between a component and its descendants without explicitly passing props through each level.

### Setting Context

```svelte
<!-- ThemeProvider.svelte -->
<script>
  import { setContext } from 'svelte';

  // The key can be any value, but using symbols helps avoid collisions
  const THEME_KEY = Symbol('theme');

  let currentTheme = $state('light');

  // Make the theme and a setter function available to all descendants
  setContext(THEME_KEY, {
    getTheme: () => currentTheme,
    setTheme: (newTheme) => {
      currentTheme = newTheme;
    }
  });

  // Export the key so other components can use it
  export { THEME_KEY };
</script>

<slot></slot>
```

### Using Context

```svelte
<!-- DeepChildComponent.svelte -->
<script>
  import { getContext } from 'svelte';
  import { THEME_KEY } from './ThemeProvider.svelte';

  // Get the theme context
  const { getTheme, setTheme } = getContext(THEME_KEY);

  // We can use the getTheme function in a reactive statement
  let theme = $derived(getTheme());

  function toggleTheme() {
    setTheme(theme === 'light' ? 'dark' : 'light');
  }
</script>

<div class="card {theme}">
  <p>Current theme: {theme}</p>
  <button on:click={toggleTheme}>Toggle Theme</button>
</div>

<style>
  .card.light {
    background-color: #fff;
    color: #222;
  }
  .card.dark {
    background-color: #222;
    color: #fff;
  }
</style>
```

### App Structure

```svelte
<!-- App.svelte -->
<script>
  import ThemeProvider from './ThemeProvider.svelte';
  import MainLayout from './MainLayout.svelte';
</script>

<ThemeProvider>
  <MainLayout />
</ThemeProvider>
```

```svelte
<!-- MainLayout.svelte -->
<script>
  import Sidebar from './Sidebar.svelte';
  import Content from './Content.svelte';
</script>

<div class="layout">
  <Sidebar />
  <Content />
</div>
```

```svelte
<!-- Content.svelte -->
<script>
  import DeepChildComponent from './DeepChildComponent.svelte';
</script>

<main>
  <h1>Main Content</h1>
  <DeepChildComponent />
</main>
```

---

By understanding these patterns for component communication and data flow, you'll be able to build well-structured Svelte applications that are maintainable and scalable. Each pattern has its own use cases, and often you'll use a combination of them in a single application.

---

[<- Back: Routing](./07-routing-svelte-navigator.md) | [Back to Main Note ->](./README.md)
