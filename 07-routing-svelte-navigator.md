# 7. Client-Side Routing with svelte-navigator 🧭

[<- Back: Props](./06-props.md) | [Next: Data Flow ->](./08-data-flow-imports.md)

## Table of Contents

- [What is Client-Side Routing?](#what-is-client-side-routing)
- [Introduction to `svelte-navigator`](#introduction-to-svelte-navigator)
- [Installation](#installation)
- [Basic Usage Concept](#basic-usage-concept)

## What is Client-Side Routing? 🌐

In traditional web applications, navigating between pages involves requesting a new HTML document from the server for each distinct URL.

In **Single Page Applications (SPAs)**, like those often built with Svelte, the application initially loads a single HTML page. Subsequent navigation between different "views" or "pages" within the app is handled **client-side** by JavaScript. This means the browser's URL changes, and the content on the page updates _without_ a full page reload from the server. This typically results in a faster, smoother user experience. ⚡

A **router** library helps manage this client-side navigation. It maps browser URL paths to specific components that should be displayed.

## Introduction to `svelte-navigator` 🧩

`svelte-navigator` is a popular client-side router library specifically designed for Svelte applications.

**Definition:**

- A declarative router for Svelte apps, inspired by libraries like React Router and Reach Router. It helps you define which components should render based on the current URL path.

**Explanation:**

- It uses the browser's native `history.pushState` API to change the URL without triggering a server request. 🔄
- It allows you to define routes declaratively within your Svelte components. 📝
- It provides components like `<Router>`, `<Route>`, and `<Link>` to manage navigation and rendering. 🧰

**Key Features:**

- **Declarative Routing:** Define routes using components. 🗺️
- **Nested Routes:** Structure routes within other routes. 🔄
- **Route Parameters:** Extract dynamic segments from the URL (e.g., `/users/:userId`). 🔍
- **Query Parameters:** Access data from the URL query string (e.g., `/search?q=svelte`). 🔎
- **Programmatic Navigation:** Navigate using JavaScript functions. 🧮
- **Route Guards:** Control access to routes (e.g., require login). 🔒
- **Accessibility:** Built with accessibility in mind. ♿

## Installation 📦

You can add `svelte-navigator` to your project using npm:

```bash
npm install svelte-navigator
```

## Basic Usage Concept 💡

While a full tutorial is beyond this introductory note, here's the basic idea:

1. **Wrap your app (or relevant part) in `<Router>`**: This component provides the routing context. Usually done in `App.svelte`. 🏠

2. **Define `<Route>` components**: Each `<Route>` maps a URL path to a Svelte component. 🛣️

3. **Use `<Link>` components**: Create navigation links that update the URL correctly without full page reloads. 🔗

```js
<!-- App.svelte (Simplified Example) -->
<script>
  import { Router, Route, Link } from "svelte-navigator";
  import Home from "$lib/pages/Home.svelte";
  import About from "$lib/pages/About.svelte";
  import UserProfile from "$lib/pages/UserProfile.svelte";
</script>

<Router>
  <nav>
    <Link to="/">Home 🏠</Link> |
    <Link to="/about">About ℹ️</Link> |
    <Link to="/users/123">User 123 👤</Link>
  </nav>

  <main>
    <Route path="/" component={Home} />
    <Route path="/about" component={About} />
    <!-- Example with a route parameter :userId -->
    <Route path="/users/:userId" component={UserProfile} />
  </main>
</Router>
```

This setup allows users to navigate between the Home, About, and UserProfile views using the links, with svelte-navigator handling the URL changes and rendering the appropriate component within the `<main>` section. ✨

---

[<- Back: Props](./06-props.md) | [Back to Main Note ->](./README.md)
