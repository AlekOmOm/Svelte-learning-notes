# 7. Client-Side Routing with svelte-routing 🧭

[<- Back: Props](./06-props.md) | [Next: Data Flow ->](./08-data-flow-imports.md)

## Table of Contents

- [What is Client-Side Routing?](#what-is-client-side-routing)
- [Introduction to `svelte-routing`](#introduction-to-svelte-routing)
- [Installation](#installation)
- [Basic Usage](#basic-usage)
- [Advanced Routing Concepts](#advanced-routing-concepts)

## What is Client-Side Routing? 🌐

In traditional web applications, navigating between pages involves requesting a new HTML document from the server for each distinct URL.

In **Single Page Applications (SPAs)** like those built with Svelte, the application initially loads a single HTML page. Subsequent navigation between different "views" or "pages" within the app is handled **client-side** by JavaScript. This means the browser's URL changes, and the content on the page updates _without_ a full page reload from the server, resulting in a faster, smoother user experience. ⚡

A **router** library manages this client-side navigation by mapping browser URL paths to specific components that should be displayed.

## Introduction to `svelte-routing` 🧩

`svelte-routing` is a declarative router library specifically designed for Svelte applications.

**Definition:**
- A lightweight router that uses HTML5 history API to handle client-side navigation in Svelte applications.

**Key Features:**
- **Declarative Routing:** Define routes using components. 🗺️
- **Nested Routes:** Structure routes within other routes. 🔄
- **Route Parameters:** Extract dynamic segments from the URL (e.g., `/users/:userId`). 🔍
- **Programmatic Navigation:** Navigate using JavaScript functions. 🧮
- **Link Component:** Create navigation links that update the URL correctly. 🔗

## Installation 📦

Add `svelte-routing` to your project using npm:

```bash
npm install svelte-routing
```

## Basic Usage 💡

Here's how to implement basic routing with `svelte-routing`:

### 1. Set Up the Router

In your main App component (usually `App.svelte`), set up the router:

```svelte
<!-- App.svelte -->
<script>
  import { Router, Link, Route } from "svelte-routing";
  import Home from "./pages/Home.svelte";
  import About from "./pages/About.svelte";
  import Profile from "./pages/Profile.svelte";
  
  // The url prop is necessary for SSR (server-side rendering)
  // In a pure client-side app, you can omit this or set it to an empty string
  export let url = "";
</script>

<Router {url}>
  <nav>
    <Link to="/">Home</Link>
    <Link to="/about">About</Link>
    <Link to="/profile">Profile</Link>
  </nav>

  <div>
    <Route path="/">
      <Home />
    </Route>
    <Route path="/about">
      <About />
    </Route>
    <Route path="/profile">
      <Profile />
    </Route>
  </div>
</Router>
```

### 2. Create Page Components

Create separate components for each page or route:

```svelte
<!-- pages/Home.svelte -->
<h1>Home Page</h1>
<p>Welcome to our website!</p>
```

```svelte
<!-- pages/About.svelte -->
<h1>About Us</h1>
<p>Learn more about our company and mission.</p>
```

### 3. Route Components and Parameters

Handle dynamic routes with parameters:

```svelte
<!-- App.svelte (with route parameter) -->
<script>
  import { Router, Link, Route } from "svelte-routing";
  import Home from "./pages/Home.svelte";
  import UserProfile from "./pages/UserProfile.svelte";
  
  export let url = "";
</script>

<Router {url}>
  <nav>
    <Link to="/">Home</Link>
    <Link to="/users/123">User 123</Link>
    <Link to="/users/456">User 456</Link>
  </nav>

  <div>
    <Route path="/" component={Home} />
    <!-- Route with parameter -->
    <Route path="/users/:id" let:params>
      <UserProfile id={params.id} />
    </Route>
  </div>
</Router>
```

```svelte
<!-- pages/UserProfile.svelte -->
<script>
  const { id } = $props();
</script>

<h1>User Profile</h1>
<p>Viewing profile for user ID: {id}</p>
```

## Advanced Routing Concepts 🔍

### Nested Routes

You can create nested routes by placing Route components inside other components:

```svelte
<!-- App.svelte -->
<Router {url}>
  <Route path="/dashboard/*">
    <Dashboard />
  </Route>
</Router>
```

```svelte
<!-- Dashboard.svelte -->
<script>
  import { Route, Link } from "svelte-routing";
  import Overview from "./Overview.svelte";
  import Settings from "./Settings.svelte";
</script>

<div class="dashboard">
  <nav>
    <Link to="/dashboard">Overview</Link>
    <Link to="/dashboard/settings">Settings</Link>
  </nav>
  
  <div>
    <Route path="/" component={Overview} />
    <Route path="/settings" component={Settings} />
  </div>
</div>
```

### Programmatic Navigation

For programmatic navigation (e.g., after form submission):

```svelte
<script>
  import { navigate } from "svelte-routing";
  
  function handleSubmit() {
    // Process form data...
    
    // Then navigate to another route
    navigate("/success");
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <!-- Form fields -->
  <button type="submit">Submit</button>
</form>
```

### Active Route Styling

You can style the active route links:

```svelte
<script>
  import { Link } from "svelte-routing";
</script>

<nav>
  <Link to="/" getProps={({ isActive }) => ({ class: isActive ? 'active' : '' })}>
    Home
  </Link>
  <Link to="/about" getProps={({ isActive }) => ({ class: isActive ? 'active' : '' })}>
    About
  </Link>
</nav>

<style>
  .active {
    font-weight: bold;
    color: #ff3e00;
  }
</style>
```

### Not Found (404) Routes

Handle non-existent routes with a catch-all route:

```svelte
<Router {url}>
  <Route path="/" component={Home} />
  <Route path="/about" component={About} />
  
  <!-- This should be the last route -->
  <Route path="*" component={NotFound} />
</Router>
```

```svelte
<!-- NotFound.svelte -->
<h1>404 - Page Not Found</h1>
<p>The page you're looking for doesn't exist.</p>
<Link to="/">Go back home</Link>
```

---

With `svelte-routing`, you can create elegant navigation for your Svelte application that provides a smooth, fast user experience without the need for full page reloads. The library's declarative approach makes it easy to define and manage your application's routes while keeping your code clean and maintainable.

---

[<- Back: Props](./06-props.md) | [Next: Data Flow ->](./08-data-flow-imports.md)
