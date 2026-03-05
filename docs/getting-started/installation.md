# Installation

<!-- The **nuRemics** project is organized into three complementary repositories:

- [**`nuremics`**](https://github.com/nuremics/nuremics){:target="_blank"}: This repository is the core Python library. It provides the foundational components to create modular and extensible software workflows.

- [**`nuremics-labs`**](https://github.com/nuremics/nuremics-labs){:target="_blank"}:
This repository contains examples of end-user applications built using the **nuRemics** framework. It is intended to be **forked** by developers to initiate their own `nuremics-labs` project and build custom scientific applications tailored to their specific use cases.

- [**`nuremics-studio`**](https://github.com/nuremics/nuremics-labs){:target="_blank"}:
This repository serves as the central hub providing a graphical front-end for managing end-user applications built with the **nuRemics** framework. It acts as the main entry point for launching and orchestrating multiple **nuRemics** applications through a generic, extensible GUI that can be customized to create application-specific interfaces. -->

<div style="text-align: center; margin-top: 2em;">
  <iframe width="640" height="360"
          src="https://www.youtube.com/embed/1F0HBIPNypY?autoplay=1&loop=1&playlist=1F0HBIPNypY&mute=1"
          frameborder="0"
          allow="autoplay"
          allowfullscreen>
  </iframe>
</div>

Installation proceeds as follows:

1. **Install Pixi** <br>
Pixi is used to easily manage mixed [PyPI](https://pypi.org/){:target="_blank"} and [Conda](https://docs.conda.io/projects/conda/en/latest/index.html){:target="_blank"} dependencies commonly encountered in scientific software projects. Follow the installation steps described on the [Pixi website](https://pixi.prefix.dev/dev/installation/){:target="_blank"}.

2. **Clone the [`nuremics-studio`](https://github.com/nuremics/nuremics-studio){:target="_blank"} repository** <br>
This repository provides the central graphical front-end used to launch and manage **nuRemics** applications.

    === "HTTPS"

        ```bash
        git clone https://github.com/nuremics/nuremics-studio.git
        ```
    === "SSH"

        ```bash
        git clone git@github.com:nuremics/nuremics-studio.git
        ```

3. **Install the project using Pixi** <br>
From the root of the repository, Pixi will resolve and install all required dependencies, creating a fully configured and reproducible development environment.

    ```bash
    pixi install
    ```

---

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="../launch/" class="md-button md-button--primary">
    Launch
  </a>
</div>

---