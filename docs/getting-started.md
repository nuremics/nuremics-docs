# Getting Started

## Installation

Installation proceeds as follows:

1. **Fork and clone the [`nuremics-labs`](https://github.com/nuremics/nuremics-labs){:target="_blank"} repository.** You have two options to get started:
    
    - Option A (recommended): Fork the repo to your own GitHub or GitLab account, then clone your fork. This allows you to modify the code and push your changes to your personal version of the project.

        ```bash
            nuremics-labs  →  fork  →  your-labs  →  clone
        ```
    
    - Option B (quick start): If you just want to try the framework without making changes, you can simply clone the main repo directly.

        === "HTTPS"

            ```bash
            git clone https://github.com/nuremics/nuremics-labs.git
            ```
        === "SSH"

            ```bash
            git clone git@github.com:nuremics/nuremics-labs.git
            ```

2. **_(Optional, but recommended)_ Create the NUREMICS virtual environment.** From the root directory of your cloned repo, use either conda or micromamba to create and install the environment using the provided `environment.yml` file.

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

3. **Install NUREMICS with the demo application.** Each application in `nuremics-labs` can be installed independently. You can start by installing the [`DEMO_APP`](Labs/Apps/General/DEMO_APP.md){:target="_blank"}.

    ```bash
    pip install .[DEMO_APP]
    ```

    This will install both the core `nuremics` framework and the `nuremics-labs` demo application.

## Run the demo

To get hands-on experience with the **NUREMICS** framework, you'll start by running the `DEMO_APP` and reproducing the scientific results of the `Study_Shape` and `Study_Velocity` studies, as demonstrated in the video below.

<div style="text-align: center; margin-top: 2em;">
  <iframe width="640" height="360"
          src="https://www.youtube.com/embed/nqGbva-uvTc?autoplay=1&loop=1&playlist=nqGbva-uvTc&mute=1"
          frameborder="0"
          allow="autoplay"
          allowfullscreen>
  </iframe>
</div>

When first run, a **NUREMICS App** creates a local workspace on your system, where you configure studies, set input data, and collect results. In this tutorial, instead of setting up from scratch, you'll start with a preconfigured workspace to reproduce the `Study_Shape` and `Study_Velocity` studies.

Follow these steps:

1. **Download the `NUREMICS_Starter` archive**. Retrieve the preconfigured `NUREMICS_Starter` archive and unzip it anywhere on your system.

    📦 [Download NUREMICS_Starter archive](assets/NUREMICS_Starter.zip)

2. **Prepare the NUREMICS working directory.** Locate the `nrs_working_dir` folder inside the unzipped archive and move it to any location on your system that is most convenient for you.

3. **Prepare the `.nuremics` directory.** Locate the `.nuremics` folder in the same unzipped archive and place it at the root of your forked/cloned `nuremics-labs` repo.

4. **Set the working directory for `DEMO_APP`**. Edit the `"working_dir"` field in `.nuremics/settings.json` and set the full path to your local `nrs_working_dir`.

    === "📄 `nuremics-labs/.nuremics/settings.json`"
        ```json hl_lines="5"
        {
            "default_working_dir": null,
            "apps": {
                "DEMO_APP": {
                    "working_dir": "path/to/your/nrs_working_dir",
                    "studies": [
                        "Study_Shape",
                        "Study_Velocity"
                    ]
                }
            }
        }
        ```

4. **Run the `DEMO_APP`**. From the `nuremics-labs/src/labs/apps/general/DEMO_APP` folder, run the **App**.

    ```python
    python src/labs/apps/general/DEMO_APP/system.py
    ```

    This will launch both studies and store results in `nrs_working_dir/DEMO_APP`.

## Play with it

Now that you've successfully run the `DEMO_APP` and reproduced the scientific results of the `Study_Shape` and `Study_Velocity` studies, let's play with it!

### Skip a study

Want to skip the execution of a specific study when running your **App**? Set its `"execute"` field to `false` in the `studies.json` file.

=== "📄 `nrs_working_dir/DEMO_APP/studies.json`"
    ```json hl_lines="3"
    {
        "Study_Shape": {
            "execute": false,
            ...
        },
        "Study_Velocity": {
            "execute": true,
            ...
        }
    }
    ```

### Skip a process

Want to skip the execution of a specific process (**Proc**) within a study? Set its `"execute"` field to `false` in the `process.json` file.

=== "📄 `nrs_working_dir/DEMO_APP/Study_Velocity/process.json`"
    ```json hl_lines="3"
    {
        "PolygonGeometryProc": {
            "execute": false,
            "silent": false
        },
        "ProjectileModelProc": {
            "execute": true,
            "silent": false
        },
        "TrajectoryAnalysisProc": {
            "execute": true,
            "silent": false
        }
    }
    ```

### Silent a process

Want a specific process (**Proc**) to be executed in silent mode within a study? Set its `"silent"` field to `true` in the `process.json` file.

=== "📄 `nrs_working_dir/DEMO_APP/Study_Velocity/process.json`"
    ```json hl_lines="8"
    {
        "PolygonGeometryProc": {
            "execute": false,
            "silent": false
        },
        "ProjectileModelProc": {
            "execute": true,
            "silent": true
        },
        "TrajectoryAnalysisProc": {
            "execute": true,
            "silent": false
        }
    }
    ```

### Skip an experiment

Want to skip the execution of a specific experiment in a study? Set the value of the `EXECUTE` flag to `0` in the `inputs.csv` file for the experiment you want to skip.

=== "📄 `nrs_working_dir/DEMO_APP/Study_Velocity/inputs.csv`"
```csv hl_lines="3"
ID,EXECUTE
Test1,1
Test2,0
Test3,1
```

### Run a new experiment

Want to run a new experiment in a study? Add a new line with a unique `ID` to the `inputs.csv` file.

=== "📄 `nrs_working_dir/DEMO_APP/Study_Velocity/inputs.csv`"
```csv hl_lines="5"
ID,EXECUTE
Test1,1
Test2,0
Test3,1
MyExp,1
```

Then run the **App**, which will automatically generate the corresponding input folder where you must upload the required `velocity.json` file.

=== "📄 `nrs_working_dir/DEMO_APP/Study_Velocity/0_inputs/0_datasets/MyExp/velocity.json`"
    ```json
    {
        "v0": 15.0,
        "angle": 60.0
    }
    ```

### Modify analysis settings

Want to customize the overall analysis of experiment results for a given study? Use the `analysis.json` file to control how each one is handled.

=== "📄 `nrs_working_dir/DEMO_APP/Study_Velocity/analysis.json`"
    ```json hl_lines="37"
    {
        "points_coordinates.csv": {},
        "polygon_shape.png": {},
        "comparison": {
            "Test1": {
                "add": true,
                "color": "red",
                "linestyle": "None",
                "linewidth": 2.0,
                "marker": "D",
                "markersize": 8,
                "markevery": 20,
                "label": "Model"
            },
            "Test2": {
                "add": true,
                "color": "limegreen",
                "linestyle": "None",
                "linewidth": 2.0,
                "marker": "v",
                "markersize": 10,
                "markevery": 15,
                "label": "Model"
            },
            "Test3": {
                "add": true,
                "color": "dodgerblue",
                "linestyle": "None",
                "linewidth": 2.0,
                "marker": "X",
                "markersize": 8,
                "markevery": 20,
                "label": "Model"
            },
            "MyExp": {
                "add": true,
                "color": "fuchsia",
                "linestyle": "None",
                "linewidth": 2.0,
                "marker": "o",
                "markersize": 8,
                "markevery": 20,
                "label": "Model"
            }
        },
        "overall_comparisons.png": {}
    }
    ```