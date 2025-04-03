# 2. Project Setup with Vite

[<- Back: Introduction](./01-introduction.md) | [Next: Project Structure ->](./03-project-structure.md)

Vite is a modern frontend build tool that provides an extremely fast development experience for web projects. It's the recommended way to start a new Svelte project.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Creating a New Svelte + Vite Project](#creating-a-new-svelte--vite-project)
- [Installing Dependencies and Running the Dev Server](#installing-dependencies-and-running-the-dev-server)

## Prerequisites

- **Node.js:**
  - Ensure you have Node.js installed (which includes npm). You can download it from [nodejs.org](https://nodejs.org/). Vite requires Node.js version 18+.

## Creating a New Svelte + Vite Project

    ```bash
    # cd ./<project-root>
    npm create vite@latest
    ```

4.  **Follow the prompts:**

    - **Project name:** 'my-svelte-app'
    - **Select a framework:** choose `Svelte`.
    - **Select a variant:** Choose `JavaScript` (or `TypeScript` if you prefer).

    ex:

    ```bash
    > npm create vite@latest

    Need to install the following packages:
      create-vite@5.x.x
    Ok to proceed? (y) y

    ✔ Project name: … my-svelte-app
    ✔ Select a framework: › Svelte
    ✔ Select a variant: › JavaScript

    Scaffolding project in C:\path\to\your\projects\my-svelte-app...

    Done. Now run:

      cd my-svelte-app
      npm install
      npm run dev
    ```

## Installing Dependencies and Running the Dev Server

1.  **Navigate into your new project directory:**

    ```bash
    cd my-svelte-app
    ```

2.  **Install the necessary dependencies:**

    ```bash
    npm install
    ```

3.  **Start the development server:**

    ```bash
    npm run dev
    ```

Vite will start the development server and print a local URL (usually `http://localhost:5173/`). Open this URL in your web browser to see your new Svelte application running! The server features Hot Module Replacement (HMR), meaning changes you make to your code will often update in the browser instantly without a full page reload.

[<- Back: Introduction](./01-introduction.md) | [Next: Project Structure ->](./03-project-structure.md)
