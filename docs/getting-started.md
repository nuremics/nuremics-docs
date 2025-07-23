# Getting Started

## Installation

Once the present `nuremics-labs` repository has been forked and cloned to your local machine, installation proceeds as follows:

1. **(Recommended) Create a dedicated coding environment.** While not mandatory, using [micromamba](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html) is highly recommended for fast, reproducible, and lightweight environment management:

```bash
micromamba create -n nrs-env python=3.12
micromamba activate nrs-env
```

2. **Install the required dependencies.** Use `pip` to install the packages listed in the `requirements.txt` file. If you need additional packages to support domain-specific application development, remember to add them to this file before running the command:

```bash
pip install -r requirements.txt
```

3. **Set the environment variable.** Add the absolute path of the `src` directory from your cloned `nuremics-labs` repository to the `PYTHONPATH` environment variable of your local system. If `PYTHONPATH` does not already exist, please create it. This allows Python to locate all project modules correctly.

```bash
/absolute/path/to/nuremics-labs/src
```

4. **(Optional) Define a default working directory.** It is also suggested to define another environment variable named `WORKING_DIR`, which serves as the default root folder where all your **Apps** write their results. This becomes particularly useful when working on multiple **Apps**, as it allows you to consistently manage output locations across your projects.

```bash
/absolute/path/to/default/working_dir
```

You're now ready to begin your coding journey with **NUREMICS®** 🧬

## Run the DEMO_APP