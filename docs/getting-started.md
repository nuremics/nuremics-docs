# Getting Started

## Installation

1. **Fork and clone the [`nuremics-labs`](https://github.com/nuremics/nuremics-labs){:target="_blank"} repository.** You have two options to get started:
    
    - Option A (recommended): Fork the repository to your own GitHub or GitLab account, then clone your fork. This allows you to modify the code and push your changes to your personal version of the project.

        ```bash
            nuremics-labs  →  fork  →  your-labs  →  clone
        ```
    
    - Option B (quick start): If you just want to try the framework without making changes, you can simply clone the main repository directly.

        === "HTTPS"

            ```bash
            git clone https://github.com/nuremics/nuremics-labs.git
            ```
        === "SSH"

            ```bash
            git clone git@github.com:nuremics/nuremics-labs.git
            ```

2. **Create the NUREMICS virtual environment.** From the root directory of your cloned repository, use either conda or micromamba to create and install the environment using the provided `environment.yml` file.

    _- Creation -_
    === "Conda"

        ```bash
        conda create -f environment.yml
        ```
    === "Micromamba"

        ```bash
        micromamba create -f environment.yml
        ```

    _- Activation -_
    === "Conda"

        ```bash
        conda activate nrs-env
        ```
    === "Micromamba"

        ```bash
        micromamba activate nrs-env
        ```

    This will create a reproducible virtual environment with all required dependencies, including the `nuremics` core package itself.

3. **Install the demo application.** Each application in `nuremics-labs` can be installed independently. You can start by installing the [DEMO_APP](labs/Apps/General/DEMO_APP.md){:target="_blank"}.

    ```bash
    pip install .[DEMO_APP]
    ```

## Run the demo