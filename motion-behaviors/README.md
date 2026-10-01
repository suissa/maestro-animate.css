<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/user-attachments/assets/bcf253f7-3b7f-4e23-a4b9-dfa7cba0f8dd">
    <img alt="maestro-animate.css logo" src="https://github.com/user-attachments/assets/fd3b13a9-1cfd-4088-9a16-54e18af1b483" width="600">
  </picture>
</p>

### 

<p align="center">
  <img src="https://shields.io" alt="License MIT">
  <img src="https://shields.io" alt="PRs Welcome">
  <img src="https://shields.io" alt="TypeScript Ready">
  <img src="https://shields.io" alt="Elm Supported">
</p> 

---

# maestro-animate.css runtime

A declarative maestro (behavior/orchestration) layer for **Animate.css**. 

The runtime focus is strictly orchestration. **Animate.css** remains fully responsible for the actual animation implementation.

## ✨ Features

- **Purely Declarative:** Manage complex animation timelines entirely through HTML attributes.
- **Decoupled Architecture:** Separates animation behavior/triggers from styles.
- **Cascading Animations:** Easily chain sequential animations without complex CSS delays.
- **Polyglot Adapters:** Built-in support for Pure TypeScript, React, and Elm.

## 🚀 How it works

### 1. Define the Configuration Contract

You map out the orchestration layer using a clean JavaScript/TypeScript configuration object:

```ts
const animate = {
  in: "zoomInDown",
  out: "zoomOutDown",
  start: ["page-loaded"],
  wait: "10s",
  finish: "hidden",
  lastFinish: "visible"
}
```

### 2. Markup your HTML

Simply use the `data-behavior` and `data-motion` attributes. The runtime listener intercepts these elements and applies the orchestration hooks seamlessly.

```html
<img data-behavior="animate el-in el-out" src="logo1.png">
<img data-behavior="animate el-in el-out" src="logo2.png">
<input data-behavior="animate el-in el-out">
<input type="submit" data-behavior="animate el-in last">
```

---

## 🛠️ API Reference

### Triggers
Control exactly *when* an animation lifecycle starts:
* `page-loaded` — Fires immediately when the DOM content is ready.
* `click:#selector` — Triggers when the specified element is clicked.
* `visible:#selector` — Triggers via IntersectionObserver when the target enters the viewport.
* `event:Event.Name` — Listens to any native or custom browser event.
* `after:#element-id` — Chains animations! Starts right after the specified element finishes its own cycle.

### Per-Element Attributes
Customize orchestration constraints directly inside individual HTML nodes:
* `data-motion-group` — Groups elements into a shared timeline state.
* `data-motion-in` — Overrides the default entry animation class.
* `data-motion-out` — Overrides the default exit animation class.
* `data-motion-wait` — Injects a specific delay duration (e.g., `0.5s`).
* `data-motion-finish` — Defines the end state visibility property (`visible`, `hidden`, `remove`).

### Lifecycle Events
Hook into execution side-effects with native DOM events:
`motion:start` • `motion:enter` • `motion:entered` • `motion:wait` • `motion:exit` • `motion:exited` • `motion:finish`

---

## 🔌 Supported Adapters

The project architecture isolates side effects so you can map state cleanly anywhere:

* **Pure TypeScript:** Driven directly inside `index.ts`.
* **React:** Composed reactively via `react.tsx`.
* **Elm:** Safe compiler-driven contracts inside `elm/src/MotionBehaviors.elm`.

> 💡 **The Elm Paradigm:** The Elm adapter emits the exact same declarative data contract securely using the type-system, while the TypeScript runtime handles the physical browser-side DOM mutations.

## 📄 License

This project is licensed under the MIT License.
