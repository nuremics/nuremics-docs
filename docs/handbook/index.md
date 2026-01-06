# Handbook

<div style="text-align: center; margin-bottom: 2em;">
  <iframe width="640" height="360"
          src="https://www.youtube.com/embed/HyUkWXGqEIM?autoplay=1&loop=1&playlist=HyUkWXGqEIM&mute=1"
          frameborder="0"
          allow="autoplay"
          allowfullscreen>
  </iframe>
</div>

The software architecture of **nuRemics** is illustrated in the diagram below. It follows the layered structure recommended by the **IEC 62304** standard, distinguishing between _software systems_, _software items_, and _software units_. This representation provides a clear, high-level view of how the different software components of the project are organized, and how they interact within a structured yet flexible development framework. It also highlights the relationship between the core framework (`nuremics`) and its domain-specific applications (`nuremics-labs`), emphasizing the modular and extensible nature of the overall architecture.

In the context of **nuRemics**:

- A _software unit_ corresponds to a single, testable function. It is the smallest building block of logic.

- A _software item_ typically takes the form of a Python class that encapsulates related functions (units) to serve a specific purpose.

- A _software system_ refers to a complete application designed to be executed by an end-user, replacing traditional scripts or notebooks.

<img src="https://raw.githubusercontent.com/julien-siguenza/nuremics-data/main/assets/architecture.svg" alt="NUREMICS Architecture" width="100%">

In practice, the core framework `nuremics` is composed of three foundational classes:

- The `Process` class defines a generic process component. It provides a flexible base structure that can be extended to implement domain-specific processes within `nuremics-labs`.

- The `Workflow` class orchestrates the execution of multiple processes in a defined sequential order. It encapsulates the coordination logic and manages the progression of tasks throughout the workflow.

- The `Application` class is the top-level component. It instantiates and executes a workflow, acting as the main entry point for any end-user application developed within `nuremics-labs`.

In `nuremics-labs`, two main types of software components are developed to build domain-specific applications:

- **Procs** (_software items_) — such as `Proc1, Proc2, ..., ProcX` — are implemented by subclassing the core `Process` class. Each **Proc** is defined as a class that encapsulates several functions (_software units_), which represent elementary operations (**Ops**) executed sequentially within the `__call__` method of the **Proc**. This design enables the creation of independent, reusable **Procs** that can be executed on their own or integrated into larger workflows.

- **Apps** (_software systems_) — such as `APP1, APP2, ..., APPX` — are the end-user-facing software applications. They import and assemble the required **Procs**, executing them in a defined order through the `Workflow` class, by instantiating the `Application` class. This modular architecture promotes flexibility and reusability, allowing the same **Procs** to be used across multiple **Apps** tailored to different scientific purposes.

---

<div align="center" style="font-weight: bold; font-size: 1.0rem;">
Explore nuRemics in:
</div>

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="theory/" class="md-button md-button--primary">
    Theory
  </a>
  <a href="practice/" class="md-button md-button--primary">
    Practice
  </a>
</div>

---