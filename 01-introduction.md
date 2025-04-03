# 1. Introduction to Svelte 🌟

[<- Back to Main Note](./README.md) | [Next: Project Setup ->](./02-project-setup-vite.md)

## Table of Contents

- [What is Svelte?](#what-is-svelte)
- [Key Difference: Compiler vs. Framework](#key-difference-compiler-vs-framework)
- [Why Choose Svelte?](#why-choose-svelte)

## What is Svelte? 🤔

Svelte is a modern JavaScript **compiler** that turns your declarative component code into efficient, imperative JavaScript that directly manipulates the DOM.

## Key Difference: Compiler vs. Framework 🔄

- **Traditional Frameworks (React, Vue, Angular):** 🏗️
  - These frameworks do most of their work in the _browser_ at _runtime_. They ship framework code along with your application code, which the browser needs to execute to make your app work (e.g., using a Virtual DOM).
- **Svelte (Compiler):** ⚙️
  - Svelte does its work during the _build step_ (compilation). It analyzes your component code and generates highly optimized, framework-less vanilla JavaScript. This means smaller bundle sizes and potentially faster performance because there's no framework overhead in the browser.

## Why Choose Svelte? 💪

- **Less Code:** 📝 Svelte's syntax is designed to be concise, often requiring less boilerplate than other frameworks.
- **No Virtual DOM:** 🎯 Svelte surgically updates the real DOM directly when state changes, which can be more performant.
- **Truly Reactive:** ⚡ Reactivity is built into the language (especially with Svelte 5 Runes like `$state`), making state management feel natural.
- **Great Performance:** 🚀 Smaller bundles and direct DOM manipulation lead to fast load times and a snappy user experience.
- **Easy to Learn:** 🧠 Many find Svelte's component structure and reactivity model intuitive.

---

[<- Back to Main Note](./README.md) | [Next: Project Setup ->](./02-project-setup-vite.md)
