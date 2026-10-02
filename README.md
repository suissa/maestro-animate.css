<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/user-attachments/assets/bcf253f7-3b7f-4e23-a4b9-dfa7cba0f8dd">
    <img alt="maestro-animate.css logo" src="https://github.com/user-attachments/assets/fd3b13a9-1cfd-4088-9a16-54e18af1b483" width="600">
  </picture>
</p><p align="center">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License MIT">
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs Welcome">
  <img src="https://img.shields.io/badge/TypeScript-Ready-blue.svg" alt="TypeScript Ready">
  <img src="https://img.shields.io/badge/Elm-Supported-60B5CC.svg" alt="Elm Supported">
</p>

<h1 style="opacity:0" >maestro-animate.css</h1>

A declarative orchestration layer for Animate.css, powered by a single framework-agnostic runtime: UbiQ Maestro animate.css.

Animate.css remains fully responsible for animation implementation.

"maestro-animate.css" decides:

- when an animation starts;
- what triggered it;
- what enters;
- what exits;
- how long an element waits;
- which element comes next;
- what ends a sequence;
- what state remains after completion.

In short:

Animate.css
    =
how an element moves

UbiQ Maestro animate.css
    =
when it moves
why it moves
what comes before it
what comes after it
when it finishes
what state remains

---

✨ Features

- Declarative Behavior Contract — describe motion behavior through HTML attributes or framework adapters.
- Single Runtime — TypeScript, React and Elm target the same orchestration semantics.
- Sequential Motion — coordinate enter, wait, exit and next-element behavior.
- Event-Driven Triggers — page load, click, visibility, custom events and dependencies.
- Groups — run independent timelines on the same page.
- Lifecycle Events — observe every stage of execution.
- Per-Element Overrides — override animation, wait time or final state locally.
- Final States — keep visible, hide or remove an element.
- Reduced Motion Support — respects "prefers-reduced-motion".
- Framework Agnostic — adapters expose the same canonical runtime contract.
- Animate.css Native — no animation implementation is duplicated in JavaScript.

---

🎼 Why “Maestro”?

"maestro-animate.css" does not create animations.

It conducts them.

Animate.css provides movements such as:

fadeIn
fadeOut
zoomInDown
zoomOutDown
bounceIn
bounceOut
hinge
rubberBand
...

The Maestro coordinates those movements into a behavior flow:

Trigger
   ↓
Enter
   ↓
Wait
   ↓
Exit
   ↓
Finish
   ↓
Next

A complete sequence can look like:

page-loaded
     ↓
 element 1
     ↓
   el-in
     ↓
   wait
     ↓
   el-out
     ↓
 element 2
     ↓
   el-in
     ↓
   wait
     ↓
   el-out
     ↓
   ...
     ↓
   last
     ↓
  visible

The animation itself is an implementation detail.

The behavior is the contract.

---

🧠 UbiQ Maestro animate.css

The core of this project is a single orchestration runtime:

«UbiQ Maestro animate.css»

It is framework-agnostic.

TypeScript, React and Elm are not separate motion engines.

They are different adapters for the same behavior model.

HTML
  │
TypeScript
  │
React
  │
Elm
  │
Web Components
  │
Future adapters
  │
  ▼
Canonical Behavior Contract
  │
  ▼
UbiQ Maestro animate.css
  │
  ├── trigger resolution
  ├── sequencing
  ├── timing
  ├── groups
  ├── lifecycle
  ├── dependency chaining
  ├── final state
  └── replay/reset
  │
  ▼
Animate.css

The syntax used by an adapter may change.

The behavior must not.

---

One runtime, many adapters

A sequence described in HTML:

<div
  data-behavior="animate el-in el-out"
  data-motion-in="zoomInDown"
  data-motion-out="zoomOutDown"
  data-motion-wait="2s"
>
  First
</div>

<div
  data-behavior="animate el-in last"
  data-motion-in="bounceIn"
  data-motion-finish="visible"
>
  Last
</div>

represents the same semantic behavior as React:

<div
  {...motionProps({
    enter: "zoomInDown",
    exit: "zoomOutDown",
    wait: "2s"
  })}
>
  First
</div>

<div
  {...motionProps({
    enter: "bounceIn",
    last: true,
    finish: "visible"
  })}
>
  Last
</div>

Elm can declare that same contract through its typed adapter.

HTML ──────┐
           │
React ─────┼──► Behavior Contract ───► UbiQ Maestro
           │
Elm ───────┘

The source changes.

The semantics do not.

---

Why only one runtime?

Independent runtimes would eventually diverge.

For example, this would be undesirable:

TypeScript:
last = keep element visible

React:
last = only skip exit

Elm:
last = terminate the group

Instead:

                    ┌── TypeScript
                    │
Behavior Contract ──┼── React
                    │
                    ├── Elm
                    │
                    └── future adapters
                    │
                    ▼
              UbiQ Maestro
                    │
                    ▼
           one semantic runtime

There is one authoritative definition of:

- what "last" means;
- when an exit occurs;
- when a sequence begins;
- how groups behave;
- how wait durations are interpreted;
- when an element is considered finished;
- which lifecycle events are emitted;
- which final state is applied.

Adapters should remain intentionally thin.

---

🚀 How it works

1. Define the orchestration configuration

const animate = {
  in: "zoomInDown",
  out: "zoomOutDown",

  start: ["page-loaded"],

  wait: "10s",

  finish: "hidden",
  lastFinish: "visible"
}

This configuration defines the default behavior of a sequence.

---

2. Declare behaviors in the markup

<img
  data-behavior="animate el-in el-out"
  src="logo1.png"
>

<img
  data-behavior="animate el-in el-out"
  src="logo2.png"
>

<input
  type="text"
  data-behavior="animate el-in el-out"
>

<input
  type="submit"
  data-behavior="animate el-in last"
>

The runtime discovers those elements and orchestrates them.

---

3. UbiQ Maestro executes the behavior graph

motion:start
      ↓
motion:enter
      ↓
Animate.css
      ↓
motion:entered
      ↓
motion:wait
      ↓
motion:exit
      ↓
Animate.css
      ↓
motion:exited
      ↓
motion:finish
      ↓
next

For a "last" element:

motion:start
      ↓
motion:enter
      ↓
Animate.css
      ↓
motion:entered
      ↓
motion:finish

---

📐 Canonical Behavior Contract

The runtime is based on a small vocabulary:

Behavior
Trigger
Enter
Wait
Exit
Finish
Group
Last
Lifecycle

At DOM level, these concepts are represented by attributes.

data-behavior
data-motion-group
data-motion-in
data-motion-out
data-motion-wait
data-motion-finish

---

🎭 Behaviors

The basic declaration is:

data-behavior="animate el-in el-out"

Supported behavior tokens:

Token| Meaning
"animate"| element participates in Maestro orchestration
"el-in"| execute the enter animation
"el-out"| execute the exit animation
"last"| mark the final element of the sequence

Example:

<div data-behavior="animate el-in el-out">
  First
</div>

<div data-behavior="animate el-in el-out">
  Second
</div>

<div data-behavior="animate el-in last">
  Final
</div>

---

⏱️ Timing

The Maestro accepts durations in milliseconds or seconds.

100ms
250ms
800ms
1s
1.5s
10s

Global configuration:

{
  wait: "2s"
}

Per-element override:

<div
  data-behavior="animate el-in el-out"
  data-motion-wait="800ms"
>
  Custom timing
</div>

---

🔀 Triggers

A sequence does not need to begin only when the page loads.

Triggers are part of the behavior contract.

Page loaded

start: ["page-loaded"]

---

Click

start: ["click:#login"]

---

Element becomes visible

start: ["visible:#pricing"]

This uses "IntersectionObserver".

---

Custom browser event

start: ["event:User.LoggedIn"]

The event can then be emitted normally:

window.dispatchEvent(
  new CustomEvent("User.LoggedIn")
);

---

After another element finishes

start: ["after:#logo"]

This allows dependency chaining between independent motion flows.

Logo
  ↓
finish
  ↓
Hero
  ↓
finish
  ↓
Form

---

🧩 Per-element overrides

Any element can override the default runtime configuration.

<div
  data-behavior="animate el-in el-out"
  data-motion-in="fadeInUp"
  data-motion-out="fadeOutDown"
  data-motion-wait="800ms"
  data-motion-finish="hidden"
>
  Custom behavior
</div>

Supported attributes:

Attribute| Purpose
"data-motion-group"| places the element inside a named sequence
"data-motion-in"| overrides the enter animation
"data-motion-out"| overrides the exit animation
"data-motion-wait"| overrides wait duration
"data-motion-finish"| overrides the final state

---

🏁 Final states

An element can finish in one of three states:

visible
hidden
remove

Visible

<div
  data-behavior="animate el-in last"
  data-motion-finish="visible"
>
  Keep me visible
</div>

Hidden

<div
  data-behavior="animate el-in el-out"
  data-motion-finish="hidden"
>
  Hide me
</div>

Remove

<div
  data-behavior="animate el-in el-out"
  data-motion-finish="remove"
>
  Remove me from the DOM
</div>

---

🧱 Groups

Multiple independent Maestro sequences can coexist on the same page.

<div
  data-behavior="animate el-in el-out"
  data-motion-group="hero"
>
  Hero A
</div>

<div
  data-behavior="animate el-in last"
  data-motion-group="hero"
>
  Hero B
</div>

And another independent group:

<div
  data-behavior="animate el-in el-out"
  data-motion-group="login"
>
  Login A
</div>

<div
  data-behavior="animate el-in last"
  data-motion-group="login"
>
  Login B
</div>

A group can be executed independently:

await runtime.run("hero");

---

🔄 Lifecycle Events

UbiQ Maestro exposes its execution lifecycle through native DOM events.

motion:start
motion:enter
motion:entered
motion:wait
motion:exit
motion:exited
motion:finish

Example:

document.addEventListener(
  "motion:finish",
  event => {
    console.log(event.detail);
  }
);

Event detail:

{
  element,
  index,
  group,
  phase
}

This makes orchestration observable without coupling application code to the implementation of the runtime.

---

🧭 Execution model

Normal element:

start
  ↓
enter
  ↓
entered
  ↓
wait
  ↓
exit
  ↓
exited
  ↓
finish

Final element:

start
  ↓
enter
  ↓
entered
  ↓
finish

Sequence:

Element 1
   │
   ├─ enter
   ├─ wait
   └─ exit
       │
       ▼
Element 2
   │
   ├─ enter
   ├─ wait
   └─ exit
       │
       ▼
Element 3
   │
   ├─ enter
   └─ last
       │
       ▼
    finish

---

🟦 TypeScript

Pure TypeScript uses the Maestro runtime directly.

import {
  animate
} from "./motion-behaviors/index";

const runtime = animate({
  in: "zoomInDown",
  out: "zoomOutDown",

  start: ["page-loaded"],

  wait: "1.5s",

  finish: "hidden",
  lastFinish: "visible"
});

Replay:

runtime.reset();

await runtime.run();

Run a specific group:

await runtime.run("hero");

---

🌐 Browser ESM

A browser-ready ES module is also provided.

motion-behaviors/browser.js

No TypeScript compilation is required.

<link
  rel="stylesheet"
  href="./animate.min.css"
>

<script type="module">
  import {
    MotionBehaviors
  } from "./motion-behaviors/browser.js";

  new MotionBehaviors({
    in: "zoomInDown",
    out: "zoomOutDown",

    start: ["page-loaded"],

    wait: "2s",

    finish: "hidden",
    lastFinish: "visible"
  }).init();
</script>

---

⚛️ React Adapter

React does not implement another runtime.

It adapts React props to the canonical Maestro behavior contract.

import {
  MotionRoot,
  motionProps
} from "./motion-behaviors/react";

Example:

const config = {
  in: "zoomInDown",
  out: "zoomOutDown",

  start: ["page-loaded"],

  wait: "1.5s",

  finish: "hidden",
  lastFinish: "visible"
} as const;

export function App() {
  return (
    <MotionRoot config={config}>

      <img
        src="/logo1.png"
        {...motionProps({
          exit: "zoomOutDown"
        })}
      />

      <div
        {...motionProps({
          enter: "fadeInUp",
          exit: "fadeOutDown"
        })}
      >
        Second
      </div>

      <button
        {...motionProps({
          enter: "bounceIn",
          last: true,
          finish: "visible"
        })}
      >
        Continue
      </button>

    </MotionRoot>
  );
}

Conceptually:

React props
     │
     ▼
motionProps(...)
     │
     ▼
Behavior Contract
     │
     ▼
UbiQ Maestro

---

🔷 Elm Adapter

Elm follows the same principle.

It does not define a different motion language.

The adapter produces the same canonical Maestro contract using Elm's type system.

Example motion definition:

{ enter = "zoomInDown"
, exit = Just "zoomOutDown"
, wait = "2s"
, finish = Hidden
, group = "hero"
, last = False
}

The adapter produces attributes equivalent to:

data-behavior="animate el-in el-out"
data-motion-group="hero"
data-motion-in="zoomInDown"
data-motion-out="zoomOutDown"
data-motion-wait="2s"
data-motion-finish="hidden"

Conceptually:

Elm values
    │
    ▼
behaviorAttributes
    │
    ▼
Behavior Contract
    │
    ▼
UbiQ Maestro

This preserves one authoritative orchestration model across all supported UI technologies.

---

🔌 Adapter architecture

Adapters should only translate framework-native concepts into the Maestro contract.

They should not reimplement orchestration.

Adapter
   │
   ├── HTML
   ├── TypeScript
   ├── React
   ├── Elm
   └── future adapters
   │
   ▼
Normalize
   │
   ▼
Canonical Behavior Contract
   │
   ▼
UbiQ Maestro animate.css
   │
   ▼
Animate.css

This makes it possible to add support later for:

Vue
Svelte
Solid
Web Components
A2UI
server-generated HTML
custom DSLs

without creating another motion engine.

The goal is not:

«many runtimes.»

The goal is:

«many ways to talk to the same runtime.»

---

▶️ Manual execution

A runtime can also be configured without an automatic trigger.

const runtime =
  new MotionBehaviors({
    in: "fadeIn",
    out: "fadeOut",

    start: []
  });

Then executed explicitly:

await runtime.run();

This is useful when the application itself owns the trigger.

---

♿ Reduced Motion

Animate.css supports:

@media
(prefers-reduced-motion: reduce)

The Maestro runtime also checks the user's reduced-motion preference.

When motion reduction is enabled, visual animation can be skipped while the behavior flow itself continues.

This means the semantic sequence remains valid even when animation is disabled.

---

🧪 Examples

The repository contains examples for the supported interfaces.

examples/
├── all-possibilities.html
├── vanilla/
│   └── main.ts
└── react/
    └── App.tsx

motion-behaviors/
├── core.ts
├── index.ts
├── browser.js
├── react.tsx
└── elm/
    ├── elm.json
    └── src/
        ├── Main.elm
        └── MotionBehaviors.elm

The "all-possibilities.html" example demonstrates:

page-loaded
enter
exit
wait
last
groups
visible
hidden
remove
click trigger
custom event trigger
IntersectionObserver trigger
per-element overrides
lifecycle events

---

🧩 Separation of responsibilities

The architecture deliberately separates three concerns.

Adapter
    =
how behavior is declared


UbiQ Maestro animate.css
    =
how behavior is orchestrated


Animate.css
    =
how movement is rendered

Or:

Declaration
    ↓
Orchestration
    ↓
Animation

This prevents orchestration logic from becoming scattered across:

setTimeout(...)
classList.add(...)
animationend callbacks
React effects
Elm-specific logic
DOM selectors
manual next()
conditional callbacks

Instead, those semantics belong to one runtime.

---

🎯 Design principle

The central design principle is:

Implementation
      ≠
Orchestration

Animate.css implements movement.

UbiQ Maestro conducts movement.

Adapters describe behavior.

       Adapter
          │
          ▼
     What should happen
          │
          ▼
     UbiQ Maestro
          │
          ▼
     When it happens
          │
          ▼
      Animate.css
          │
          ▼
      How it moves

---

🔭 Extensibility

Because the runtime is independent from its adapters, new declaration formats can be introduced without changing the execution semantics.

For example:

Vue
  ↓
Vue Adapter
  ↓
Behavior Contract
  ↓
UbiQ Maestro

or:

A2UI
  ↓
Maestro Adapter
  ↓
Behavior Contract
  ↓
UbiQ Maestro

or even:

Custom DSL
    ↓
Parser / Adapter
    ↓
Behavior Contract
    ↓
UbiQ Maestro

The runtime remains the same.

---

🧬 In one sentence

«UbiQ Maestro animate.css is a single framework-agnostic orchestration runtime for Animate.css, exposed through multiple adapters that share one canonical declarative behavior contract.»

---

📄 License

This project is licensed under the MIT License.

---

🙏 Credits

"maestro-animate.css" uses Animate.css as its animation engine.

Animate.css remains responsible for the CSS animation implementation.

"UbiQ Maestro animate.css" adds the orchestration layer responsible for behavior, lifecycle, sequencing and execution semantics.
