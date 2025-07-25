# Practice

## Create App

This section walks you through the process of building a custom **NUREMICS** application from scratch. You'll start by implementing your own [**Procs**](theory.md#proc), which encapsulate domain-specific logic and computational tasks. Then, you’ll learn how to assemble these building blocks into a fully operational [**App**](theory.md#app), ready to run studies and generate structured results.

Whether you're developing a quick prototype or a full-scale scientific workflow, this guide will help you translate your ideas into modular, reusable, traceable and scalable software components.

### Implement Procs

We start by defining the core building blocks of the **App** to be created: the **Procs**. Each **Proc** is a reusable item that encapsulates a specific piece of logic executed within the overall workflow. Internally, this logic can be further decomposed into elementary operations (**Ops**), implemented as individual functions (units) within the **Proc** itself.

To implement our first **Proc**, we begin by importing the `Process` base class from **NUREMICS**, which all custom **Procs** must inherit from. To make this inheritance simple and structured, we also import the `attrs` library, which helps define clean, data-driven Python classes.

```python
import attrs
from nuremics import Process
```

We then declare our first **Proc** as a Python class named `PolygonGeometryProc`, inheriting from the `Process` base class. This marks it as a modular item of computation which can be executed within a **NUREMICS** workflow.

```python
import attrs
from nuremics import Process

@attrs.define
class PolygonGeometryProc(Process):
```

We now declare the input data required by our `PolygonGeometryProc`, grouped into two categories: **Parameters** and **Paths**. Each input is defined using `attrs.field()` and marked with `metadata={"input": True}`.

This metadata is essential: it tells the **NUREMICS** framework that these attributes are expected as input data, ensuring they are properly tracked and managed throughout the workflow.

```python
import attrs
from pathlib import Path
from nuremics import Process

@attrs.define
class PolygonGeometryProc(Process):

    # Parameters
    radius: float = attrs.field(init=False, metadata={"input": True})
    n_sides: int = attrs.field(init=False, metadata={"input": True})
  
    # Paths
    title_file: Path = attrs.field(init=False, metadata={"input": True}, converter=Path)
```

In addition to the previously declared input data, a **Proc** can also define internal variables: attributes used during the execution of its internal logic but not provided as input data.

These internal variables, like `df_points` in our example below, are declared without the `metadata={"input": True}` tag, signaling to the **NUREMICS** framework that they are not exposed to the workflow and will be set or computed within the **Proc** itself.

```python
import attrs
import pandas as pd
from pathlib import Path
from nuremics import Process

@attrs.define
class PolygonGeometryProc(Process):

    # Parameters
    radius: float = attrs.field(init=False, metadata={"input": True})
    n_sides: int = attrs.field(init=False, metadata={"input": True})
  
    # Paths
    title_file: Path = attrs.field(init=False, metadata={"input": True}, converter=Path)

    # Internal
    df_points: pd.DataFrame = attrs.field(init=False)
```

The operations executed by the **Proc** are finally implemented as elementary functions (**Ops**), which are then sequentially called within the `__call__()` method to define the overall logic of the **Proc**.

```python
import attrs
import pandas as pd
from pathlib import Path
from nuremics import Process

@attrs.define
class PolygonGeometryProc(Process):

    # Parameters
    radius: float = attrs.field(init=False, metadata={"input": True})
    n_sides: int = attrs.field(init=False, metadata={"input": True})
  
    # Paths
    title_file: Path = attrs.field(init=False, metadata={"input": True}, converter=Path)

    # Internal
    df_points: pd.DataFrame = attrs.field(init=False)

    def __call__(self):
        super().__call__()

        self.generate_polygon_shape()
        self.plot_polygon_shape()
    
    def generate_polygon_shape(self):
        # </> your code </>

    def plot_polygon_shape(self):
        # </> your code </>
```

Note that the **Proc** should at some point produce output data, typically in the form of files or folders generated during the execution of its **Ops**. To make these output data trackable by the **NUREMICS** framework, each must be registered in the `self.output_paths` dictionary using a label that is unique to the **Proc** (e.g., `"coords_file"`, `"fig_file"`).

Using the dictionary syntax `self.output_paths["coords_file"]` effectively declares an output variable named `coords_file`, which will later be instantiated by assigning it a specific file or folder name when integrating the **Proc** into a broader application workflow.

```python
import attrs
import pandas as pd
from pathlib import Path
from nuremics import Process

@attrs.define
class PolygonGeometryProc(Process):

    # Parameters
    radius: float = attrs.field(init=False, metadata={"input": True})
    n_sides: int = attrs.field(init=False, metadata={"input": True})
  
    # Paths
    title_file: Path = attrs.field(init=False, metadata={"input": True}, converter=Path)

    # Internal
    df_points: pd.DataFrame = attrs.field(init=False)

    def __call__(self):
        super().__call__()

        self.generate_polygon_shape()
        self.plot_polygon_shape()
    
    def generate_polygon_shape(self):
        # </> your code </>
        file = self.output_paths["coords_file"]
        # </> Write file </>

    def plot_polygon_shape(self):
        # </> your code </>
        file = self.output_paths["fig_file"]
        # </> Write file </>
```

Even though **Procs** are not intended to be executed independently by end-users, they are still designed with the possibility to run _out of the box_. This allows developers to easily execute them during the development phase or when implementing dedicated unit tests for a specific **Proc**.

In such cases, it is important to set `set_inputs=True` when instantiating the **Proc**, to explicitly inform the **NUREMICS** framework that the input data are being provided manually, outside of any workflow context.

```python
import attrs
import pandas as pd
from pathlib import Path
from nuremics import Process

@attrs.define
class PolygonGeometryProc(Process):

    # Parameters
    radius: float = attrs.field(init=False, metadata={"input": True})
    n_sides: int = attrs.field(init=False, metadata={"input": True})
  
    # Paths
    title_file: Path = attrs.field(init=False, metadata={"input": True}, converter=Path)

    # Internal
    df_points: pd.DataFrame = attrs.field(init=False)

    def __call__(self):
        super().__call__()

        self.generate_polygon_shape()
        self.plot_polygon_shape()
    
    def generate_polygon_shape(self):
        # </> your code </>
        file = self.output_paths["coords_file"]
        # </> Write file </>

    def plot_polygon_shape(self):
        # </> your code </>
        file = self.output_paths["fig_file"]
        # </> Write file </>

if __name__ == "__main__":
    
    # Define working directory
    working_dir = ...

    # Go to working directory
    os.chdir(working_dir)

    # Create dictionary containing input data
    dict_inputs = {
        "radius": ...,
        "n_sides": ...,
        "title_file": ...,
    }
    
    # Create process
    process = PolygonGeometryProc(
        dict_inputs=dict_inputs,
        set_inputs=True,
    )
    process.output_paths["coords_file"] = "points_coordinates.csv"
    process.output_paths["fig_file"] = "polygon_shape.png"

    # Run process
    process()
    process.finalize()
```

### Assemble Procs into App

Most of the development effort has already been carried out when implementing the individual **Procs**. The next step consists in assembling them into a coherent **App**, where each **Proc** is instantiated, connected, and orchestrated to form a complete, executable workflow.

We start by defining the name of our **App**.

```python
APP_NAME = "DEMO_APP"
```

We then import the `Application` class from the **NUREMICS** framework, which serves as the container and manager to define a workflow composed of multiple **Procs**.

```python
from nuremics import Application

APP_NAME = "DEMO_APP"
```

We now import the two **Procs**, `PolygonGeometryProc` and `ProjectileModelProc`, that we previously implemented. These will be the building blocks to assemble into our final **App**.

```python
from nuremics import Application
from procs.general.PolygonGeometryProc.item import PolygonGeometryProc
from procs.general.ProjectileModelProc.item import ProjectileModelProc

APP_NAME = "DEMO_APP"
```

The source code of the **App** then adopts the structure of a standard Python script, which can both be executed directly or imported as a module. This is achieved by defining a `main()` function and guarding it with the typical `if __name__ == "__main__":` statement.

```python
from nuremics import Application
from procs.general.PolygonGeometryProc.item import PolygonGeometryProc
from procs.general.ProjectileModelProc.item import ProjectileModelProc

APP_NAME = "DEMO_APP"

def main():
    # Application logic here

if __name__ == "__main__":
    main()
```

In the `main()` function, we add two input arguments that the end-user must specify when launching the **App** inside the `if __name__ == "__main__":` block:

- `working_dir`: the working directory from which the **App** will be executed.

- `studies`: a list of study names that the end-user wants to perform with the **App**.

```python
from pathlib import Path
from nuremics import Application
from procs.general.PolygonGeometryProc.item import PolygonGeometryProc
from procs.general.ProjectileModelProc.item import ProjectileModelProc

APP_NAME = "DEMO_APP"

def main(
    working_dir: Path = None,
    studies: list = ["Default"],
):
    # Application logic here

if __name__ == "__main__":

    # ------------------------ #
    # Define working directory #
    # ------------------------ #
    working_dir = Path(os.environ["WORKING_DIR"])

    # -------------- #
    # Define studies #
    # -------------- #
    studies = [
        "Study_Shape",
        "Study_Velocity",
    ]

    # --------------- #
    # Run application #
    # --------------- #
    main(
        working_dir=working_dir,
        studies=studies,
    )
```

Inside the `main()` function, we define a list called `workflow` which contains the sequence of **Procs** to be executed, in the order specified. This list is made up of dictionaries, where each dictionary describes the characteristics of a particular **Proc**.

Let's first define the key `"process"` of each dictionary, which specifies the **Proc** class (previously imported, e.g., `PolygonGeometryProc` and `ProjectileModelProc`) to instantiate and execute within the **App** workflow.

This dictionary-based structure offers flexibility to easily add more parameters or options later by simply adding new keys to each dictionary in the workflow.

```python
from pathlib import Path
from nuremics import Application
from procs.general.PolygonGeometryProc.item import PolygonGeometryProc
from procs.general.ProjectileModelProc.item import ProjectileModelProc

APP_NAME = "DEMO_APP"

def main(
    working_dir: Path = None,
    studies: list = ["Default"],
):
    # --------------- #
    # Define workflow #
    # --------------- #
    workflow = [
        {
            "process": PolygonGeometryProc,
        },
        {
            "process": ProjectileModelProc,
        },
    ]

if __name__ == "__main__":

    # ------------------------ #
    # Define working directory #
    # ------------------------ #
    working_dir = Path(os.environ["WORKING_DIR"])

    # -------------- #
    # Define studies #
    # -------------- #
    studies = [
        "Study_Shape",
        "Study_Velocity",
    ]

    # --------------- #
    # Run application #
    # --------------- #
    main(
        working_dir=working_dir,
        studies=studies,
    )
```

We now create an `Application` object `app`, which acts as the core engine of our **App**. This object is instantiated using the previously defined inputs:

- `app_name`: the name of the **App**.

- `working_dir`: the root directory from which the **App** is executed.

- `workflow`: the ordered list of **Procs** to run.

- `studies`: the list of studies the end-user wishes to perform.

Once the `Application` object is created, calling `app()` launches the workflow execution of all the defined **Procs** for each study.

```python
from pathlib import Path
from nuremics import Application
from procs.general.PolygonGeometryProc.item import PolygonGeometryProc
from procs.general.ProjectileModelProc.item import ProjectileModelProc

APP_NAME = "DEMO_APP"

def main(
    working_dir: Path = None,
    studies: list = ["Default"],
):
    # --------------- #
    # Define workflow #
    # --------------- #
    workflow = [
        {
            "process": PolygonGeometryProc,
        },
        {
            "process": ProjectileModelProc,
        },
    ]

    # ------------------ #
    # Define application #
    # ------------------ #
    app = Application(
        app_name=APP_NAME,
        working_dir=working_dir,
        workflow=workflow,
        studies=studies,
    )
    # Run it!
    app()

if __name__ == "__main__":

    # ------------------------ #
    # Define working directory #
    # ------------------------ #
    working_dir = Path(os.environ["WORKING_DIR"])

    # -------------- #
    # Define studies #
    # -------------- #
    studies = [
        "Study_Shape",
        "Study_Velocity",
    ]

    # --------------- #
    # Run application #
    # --------------- #
    main(
        working_dir=working_dir,
        studies=studies,
    )
```

When running the **App**, **NUREMICS** first provides the following terminal feedback:

- A visual banner indicating the launch of a **NUREMICS App**.

- A structured overview of the assembled workflow, showing each registered **Proc**, its associated **Ops** (functions), and their order of execution within the **App** workflow.

👤🔄🖥️
```shell
                      ___
                     /\  \                      ___
     ___            /::\  \        ___         /\  \
    /\__\          /:/\:\  \      /\__\       /::\  \
   /::|  |        /::\~\:\  \    /::|  |     /:/\:\  \
  /:|:|  |       /:/\:\ \:\__\  /:|:|  |    /:/  \:\  \
 /:/|:|  |__     \/_|::\/:/  / /:/|:|__|__ /:/__/ \:\__\
/:/ |:| /\__\  ___  |:|::/  / /:/ |::::\__\\:\  \  \/__/
\/__|:|/:/  / /\__\ |:|\/__/  \/__/~~/:/  / \:\  \
    |:/:/  / /:/  / |:|  |  ___     /:/  /   \:\  \
    |::/  / /:/  /   \|__| /\  \   /:/  / ___ \:\__\ ___
    /:/  / /:/  /  ___    /::\  \ /:/  / /\  \ \/__//\  \
    \/__/ /:/__/  /\__\  /:/\:\  \\/__/  \:\  \    /::\  \
          \:\  \ /:/  / /::\~\:\  \      /::\__\  /:/\ \  \
           \:\  /:/  / /:/\:\ \:\__\  __/:/\/__/ _\:\~\ \  \
            \:\/:/  /  \:\~\:\ \/__/ /\/:/  /   /\ \:\ \ \__\
             \::/  /    \:\ \:\__\   \::/__/    \:\ \:\ \/__/
              \/__/      \:\ \/__/    \:\__\     \:\ \:\__\
                          \:\__\       \/__/      \:\/:/  /
                           \/__/                   \::/  /
                                                    \/__/
> APPLICATION <

| Workflow |
DEMO_APP_____
             |_____PolygonGeometryProc_____
             |                             |_____generate_polygon_shape
             |                             |_____plot_polygon_shape
             |
             |_____ProjectileModelProc_____
                                           |_____simulate_projectile_motion
                                           |_____calculate_analytical_trajectory
                                           |_____compare_model_vs_analytical_trajectories
```

At this stage, **NUREMICS** also performs a structural check of each **Proc** by inspecting its `__call__` method. Specifically, it ensures that only functions defined within the **Proc** class itself are invoked during execution. This design choice enforces a clean and self-contained structure for each **Proc**, where all internal logic remains encapsulated.

Let’s consider a case where the developer does not adhere to this enforced structural rule, for instance, by injecting additional logic directly into the `__call__` method of a **Proc** (in this example, in the `ProjectileModelProc` class).

```python
    def __call__(self):
        super().__call__()

        some_parameter = 2 # <-- External logic added here

        self.simulate_projectile_motion()
        self.calculate_analytical_trajectory()
        self.compare_model_vs_analytical_trajectories()
```

In this situation, **NUREMICS** will immediately raise a structural validation error and halt execution.

👤🔄🖥️
```shell
| Workflow |
DEMO_APP_____
             |_____PolygonGeometryProc_____
             |                             |_____generate_polygon_shape
             |                             |_____plot_polygon_shape
             |
             |_____ProjectileModelProc_____(X)

(X) Each process must only call its internal function(s):

    def __call__(self):
        super().__call__()

        self.operation1()
        self.operation2()
        self.operation3()
        ...
```

**NUREMICS** is then expected to display a summary of all required input/output data for each **Proc**, along with their current mapping status within the **App**.

At this stage, the system automatically verifies whether every required input/output data has been properly mapped within the **App** configuration.

If any **input parameters** are missing, they are explicitly listed, and the developer is prompted to define them using either the `"user_params"` or `"hard_params"` key.

👤🔄🖥️
```shell
| PolygonGeometryProc |
> Input Parameter(s) :
(float) radius  -----||----- Not defined (X)
(int)   n_sides -----||----- Not defined (X)

(X) Please define all input parameters either in "user_params" or "hard_params".
```

The **input parameters** of the **Proc** `PolygonGeometryProc` can be properly mapped within the **App** by defining the `"user_params"` and/or `"hard_params"` keys in its corresponding dictionary entry inside the `workflow` list.

```python
from pathlib import Path
from nuremics import Application
from procs.general.PolygonGeometryProc.item import PolygonGeometryProc
from procs.general.ProjectileModelProc.item import ProjectileModelProc

APP_NAME = "DEMO_APP"

def main(
    working_dir: Path = None,
    studies: list = ["Default"],
):
    # --------------- #
    # Define workflow #
    # --------------- #
    workflow = [
        {
            "process": PolygonGeometryProc,
            "user_params": {
                "n_sides": "nb_sides",
            },
            "hard_params": {
                "radius": 0.5,
            },
        },
        {
            "process": ProjectileModelProc,
        },
    ]

    # ------------------ #
    # Define application #
    # ------------------ #
    app = Application(
        app_name=APP_NAME,
        working_dir=working_dir,
        workflow=workflow,
        studies=studies,
    )
    # Run it!
    app()

if __name__ == "__main__":

    # ------------------------ #
    # Define working directory #
    # ------------------------ #
    working_dir = Path(os.environ["WORKING_DIR"])

    # -------------- #
    # Define studies #
    # -------------- #
    studies = [
        "Study_Shape",
        "Study_Velocity",
    ]

    # --------------- #
    # Run application #
    # --------------- #
    main(
        working_dir=working_dir,
        studies=studies,
    )
```

When running the **App** again, **NUREMICS** detects that all required **input parameters** for `PolygonGeometryProc` have been successfully mapped.

However, it now reports that one or more **input paths** are missing. These are explicitly listed, and the developer is prompted to define them using either the `"user_paths"` or `"required_paths"` key.

👤🔄🖥️
```shell
| PolygonGeometryProc |
> Input Parameter(s) :
(float) radius  -----||----- 0.5      (hard_params)
(int)   n_sides -----||----- nb_sides (user_params)
> Input Path(s) :
title_file -----||----- Not defined (X)

(X) Please define all input paths either in "user_paths" or "required_paths".
```

The **input paths** of the **Proc** `PolygonGeometryProc` can be properly mapped within the **App** by defining the `"user_paths"` and/or `"required_paths"` keys in its corresponding dictionary entry inside the workflow list.

```python
from pathlib import Path
from nuremics import Application
from procs.general.PolygonGeometryProc.item import PolygonGeometryProc
from procs.general.ProjectileModelProc.item import ProjectileModelProc

APP_NAME = "DEMO_APP"

def main(
    working_dir: Path = None,
    studies: list = ["Default"],
):
    # --------------- #
    # Define workflow #
    # --------------- #
    workflow = [
        {
            "process": PolygonGeometryProc,
            "user_params": {
                "n_sides": "nb_sides",
            },
            "hard_params": {
                "radius": 0.5,
            },
            "user_paths": {
                "title_file": "plot_title.txt",
            },
        },
        {
            "process": ProjectileModelProc,
        },
    ]

    # ------------------ #
    # Define application #
    # ------------------ #
    app = Application(
        app_name=APP_NAME,
        working_dir=working_dir,
        workflow=workflow,
        studies=studies,
    )
    # Run it!
    app()

if __name__ == "__main__":

    # ------------------------ #
    # Define working directory #
    # ------------------------ #
    working_dir = Path(os.environ["WORKING_DIR"])

    # -------------- #
    # Define studies #
    # -------------- #
    studies = [
        "Study_Shape",
        "Study_Velocity",
    ]

    # --------------- #
    # Run application #
    # --------------- #
    main(
        working_dir=working_dir,
        studies=studies,
    )
```

When running the **App** again, **NUREMICS** detects that all required **input paths** for `PolygonGeometryProc` have been successfully mapped.

However, it now reports that one or more **output paths** are missing. These are explicitly listed, and the developer is prompted to define them using the `"output_paths"` key.

👤🔄🖥️
```shell
| PolygonGeometryProc |
> Input Parameter(s) :
(float) radius  -----||----- 0.5      (hard_params)
(int)   n_sides -----||----- nb_sides (user_params)
> Input Path(s) :
title_file -----||----- plot_title.txt (user_paths)
> Output Path(s) :
coords_file -----||----- Not defined (X)
fig_file    -----||----- Not defined (X)

(X) Please define all output paths in "output_paths".
```

The **output paths** of the **Proc** `PolygonGeometryProc` can be properly mapped within the **App** by defining the `"output_paths"` key in its corresponding dictionary entry inside the workflow list.

In the same way, we also complete the mapping for the **Proc** `ProjectileModelProc` by providing all required entries: `"user_params"` and/or `"hard_params"`, `"user_paths"` and/or `"required_paths"`, `"output_paths"`.

```python
from pathlib import Path
from nuremics import Application
from procs.general.PolygonGeometryProc.item import PolygonGeometryProc
from procs.general.ProjectileModelProc.item import ProjectileModelProc

APP_NAME = "DEMO_APP"

def main(
    working_dir: Path = None,
    studies: list = ["Default"],
):
    # --------------- #
    # Define workflow #
    # --------------- #
    workflow = [
        {
            "process": PolygonGeometryProc,
            "user_params": {
                "n_sides": "nb_sides",
            },
            "hard_params": {
                "radius": 0.5,
            },
            "user_paths": {
                "title_file": "plot_title.txt",
            },
            "output_paths": {
                "coords_file": "points_coordinates.csv",
                "fig_file": "polygon_shape.png",
            },
        },
        {
            "process": ProjectileModelProc,
            "user_params": {
                "gravity": "gravity",
                "mass": "mass",
            },
            "user_paths": {
                "velocity_file": "velocity.json",
                "configs_folder": "configs",
            },
            "required_paths": {
                "coords_file": "points_coordinates.csv",
            },
            "output_paths": {
                "comp_folder": "comparison",
            },
        },
    ]

    # ------------------ #
    # Define application #
    # ------------------ #
    app = Application(
        app_name=APP_NAME,
        working_dir=working_dir,
        workflow=workflow,
        studies=studies,
    )
    # Run it!
    app()

if __name__ == "__main__":

    # ------------------------ #
    # Define working directory #
    # ------------------------ #
    working_dir = Path(os.environ["WORKING_DIR"])

    # -------------- #
    # Define studies #
    # -------------- #
    studies = [
        "Study_Shape",
        "Study_Velocity",
    ]

    # --------------- #
    # Run application #
    # --------------- #
    main(
        working_dir=working_dir,
        studies=studies,
    )
```

With all required mappings now properly defined for each **Proc**, the **App** can be executed without raising any errors. **NUREMICS** confirms that the full mapping is complete by prompting a summary for each **Proc**, indicating that all **input parameters**, **input paths**, and **output paths** have been successfully resolved.

👤🔄🖥️
```shell
| PolygonGeometryProc |
> Input Parameter(s) :
(float) radius  -----||----- 0.5      (hard_params)
(int)   n_sides -----||----- nb_sides (user_params)
> Input Path(s) :
title_file -----||----- plot_title.txt (user_paths)
> Output Path(s) :
coords_file -----||----- points_coordinates.csv (output_paths)
fig_file    -----||----- polygon_shape.png      (output_paths)

| ProjectileModelProc |
> Input Parameter(s) :
(float) gravity -----||----- gravity (user_params)
(float) mass    -----||----- mass    (user_params)
> Input Path(s) :
velocity_file  -----||----- velocity.json          (user_paths)
configs_folder -----||----- configs                (user_paths)
coords_file    -----||----- points_coordinates.csv (required_paths)
> Output Path(s) :
comp_folder -----||----- comparison (output_paths)
```

As the **App** has now been fully assembled, **NUREMICS** displays a clean summary of its I/O interface, as it will appear to the end-user.

This summary includes all declared user parameters and user paths required as inputs, along with the corresponding output files and folders that the **App** will generate. It serves as an explicit interface contract, allowing end-users to clearly understand what data they need to provide and what results to expect.

👤🔄🖥️
```shell
> INPUTS <

| User Parameters |
> nb_sides (int)
> gravity (float)
> mass (float)

| User Paths |
> plot_title.txt
> velocity.json
> configs

> OUTPUTS <

> points_coordinates.csv
> polygon_shape.png
> comparison
```

With all **Procs** implemented and properly assembled within the **App**, the development work is now complete. The developer’s responsibility ends here (excluding, of course, the implementation of unit tests to ensure long-term maintainability, which falls outside the scope of this tutorial).

The **App** is now fully functional and ready to be operated by end-users. From this point, users can interact with the **App** through its declared I/O interface, without needing to modify or understand the underlying code structure.

## Use App

Now that the **App** has been fully implemented and assembled, this new section focuses on its usage from the end-user's perspective.

We will demonstrate how to interact with a ready-to-use **NUREMICS App**, including how to provide inputs, run the **App**, and retrieve the expected outputs, all without needing to understand its internal structure.

This section assumes the job of the developer is done, and shifts the focus to the operational phase of the **App**.

### Define Working Environment

Before running a **NUREMICS App**, the operator must first define the working environment in the `if __name__ == "__main__":` section of the **App**.

This setup step specifies two key elements:
- **Working directory (`working_dir`):** The root path where all input/output data, logs, and results will be stored.
- **Study names (`studies`):** A list of identifiers corresponding to the different studies the operator wants to run. Each study will be managed in its own dedicated folder under the working directory.

```python
if __name__ == "__main__":
    
    # ------------------------ #
    # Define working directory #
    # ------------------------ #
    working_dir = Path(os.environ["WORKING_DIR"])

    # -------------- #
    # Define studies #
    # -------------- #
    studies = [
        "Study_Shape",
        "Study_Velocity",
    ]

    # --------------- #
    # Run application #
    # --------------- #
    main(
        working_dir=working_dir,
        studies=studies,
    )
```

In this example:

- The `working_dir` is read from the environment variable `WORKING_DIR` previously introduced.
- The `studies` list contains the names of the studies you want to run. You can define as many studies as needed, each representing a self-contained parametric study.

As the **App** is executed, a new folder named after the **App** is automatically created under the specified `working_dir`. This folder acts as the root container for all the execution-related content generated by the **App**.

👤👁️💾
```bash
<working_dir>/
└── DEMO_APP/
    └── studies.json
```

📁 **App folder** (`DEMO_APP/`)<br>
The name of this folder is derived from the name of the application (`APP_NAME`) as defined during its construction.

📄 **Studies configuration file** (`studies.json`)<br>
This file serves as a centralized configuration hub for each study, allowing the operator to specify which input data remain fixed and which can vary across various experiments.

### Configure Studies

The **NUREMICS** terminal now provides feedback on the defined studies and halts execution, indicating that the first study `Study_Shape` requires configuration.

👤🔄🖥️
```shell
> STUDIES <

| Study_Shape |
(X) nb_sides not configured.
(X) gravity not configured.
(X) mass not configured.
(X) plot_title.txt not configured.
(X) velocity.json not configured.
(X) configs not configured.

(X) Please configure file :
> .../DEMO_APP/studies.json
```

Let's configure `Study_Shape` by allowing only `nb_sides` to vary (by assigning a `true` value in the `studies.json` file), and keeping all other input data fixed (by assigning `false` values in the `studies.json` file) across the study.

📄 `studies.json`
```json
{
    "Study_Shape": {
        "execute": true,
        "user_params": {
            "nb_sides": true,
            "gravity": false,
            "mass": false
        },
        "user_paths": {
            "plot_title.txt": false,
            "velocity.json": false,
            "configs": false
        }
    },
    "Study_Velocity": {
        "execute": true,
        "user_params": {
            "nb_sides": null,
            "gravity": null,
            "mass": null
        },
        "user_paths": {
            "plot_title.txt": null,
            "velocity.json": null,
            "configs": null
        }
    }
}
```

At the next execution of the **App**, the **NUREMICS** terminal now prompts that `Study_Shape` is properly configured, but halts indicating that the second study `Study_Velocity` still requires configuration.

👤🔄🖥️
```shell
> STUDIES <

| Study_Shape |
(V) nb_sides is variable.
(V) gravity is fixed.
(V) mass is fixed.
(V) plot_title.txt is fixed.
(V) velocity.json is fixed.
(V) configs is fixed.

| Study_Velocity |
(X) nb_sides not configured.
(X) gravity not configured.
(X) mass not configured.
(X) plot_title.txt not configured.
(X) velocity.json not configured.
(X) configs not configured.

(X) Please configure file :
> .../DEMO_APP/studies.json
```

Let's this time configure `Study_Velocity` by allowing only `velocity.json` to vary, and keeping all other input data fixed across the study.

📄 `studies.json`
```json
{
    "Study_Shape": {
        "execute": true,
        "user_params": {
            "nb_sides": true,
            "gravity": false,
            "mass": false
        },
        "user_paths": {
            "plot_title.txt": false,
            "velocity.json": false,
            "configs": false
        }
    },
    "Study_Velocity": {
        "execute": true,
        "user_params": {
            "nb_sides": false,
            "gravity": false,
            "mass": false
        },
        "user_paths": {
            "plot_title.txt": false,
            "velocity.json": true,
            "configs": false
        }
    }
}
```

At the next execution of the **App**, the **NUREMICS** terminal now prompts that both `Study_Shape` and `Study_Velocity` are properly configured.

👤🔄🖥️
```shell
> STUDIES <

| Study_Shape |
(V) nb_sides is variable.
(V) gravity is fixed.
(V) mass is fixed.
(V) plot_title.txt is fixed.
(V) velocity.json is fixed.
(V) configs is fixed.

| Study_Velocity |
(V) nb_sides is fixed.
(V) gravity is fixed.
(V) mass is fixed.
(V) plot_title.txt is fixed.
(V) velocity.json is variable.
(V) configs is fixed.
```

Going back to the data tree generated within the defined `working_dir`, we can see that a specific folder for each study has been created.

👤👁️💾
```bash
<working_dir>/
└── DEMO_APP/
    ├── studies.json
    ├── Study_Shape/
    └── Study_Velocity/
```

### Set Input Data

Each study directory within the data tree now contains an initialized input database that must be completed by the operator.

👤👁️💾
```bash
<working_dir>/
└── DEMO_APP/
    ├── studies.json
    ├── Study_Shape/
    │   ├── 0_inputs/
    │   ├── inputs.json
    │   └── inputs.csv
    └── Study_Velocity/
        ├── 0_inputs/
        ├── inputs.json
        └── inputs.csv
```

This input database contains:

- **`0_inputs`:** This folder must contain the input files and/or folders defined as `"user_paths"`.
- **`inputs.json`:** This file must contain the input parameters defined as _fixed_ `"user_params"`.
- **`inputs.csv`:** This file must contain the input parameters defined as _variable_ `"user_params"`.

At this stage of the **App** execution, the **NUREMICS** terminal halts by indicating that the _fixed_ input data must be set.

👤🔄🖥️
```shell
> SETTINGS <

| Study_Shape |
> Common : (X) gravity (X) mass (X) plot_title.txt (X) velocity.json (X) configs

(X) Please set inputs :
> .../DEMO_APP/Study_Shape/inputs.json
> .../DEMO_APP/Study_Shape/0_inputs/plot_title.txt
> .../DEMO_APP/Study_Shape/0_inputs/velocity.json
> .../DEMO_APP/Study_Shape/0_inputs/configs
```

For the `Study_Shape`, we are speaking about:

- `gravity` and `mass` within the `inputs.json` file.
- `plot_title.txt`, `velocity.json` and `configs` within the `0_inputs` folder.

<br>

📄 `inputs.json`
```json
{
    "gravity": -9.81,
    "mass": 0.1
}
```
<br>

📄 `0_inputs/plot_title.txt`
```
2D polygon shape
```
<br>

📄 `0_inputs/velocity.json`
```json
{
    "v0": 15.0,
    "angle": 45.0
}
```
<br>

📄 `0_inputs/configs/solver_config.json`
```json
{
    "timestep": 0.01
}
```
<br>

📄 `0_inputs/configs/display_config.json`
```json
{
    "fps": 60,
    "size": 700
}
```
<br>

All _fixed_ input data have now been completed within the `Study_Shape` input database.

👤👁️💾
```bash
<working_dir>/
└── DEMO_APP/
    ├── studies.json
    ├── Study_Shape/
    │   ├── 0_inputs/
    │   │   ├── plot_title.txt
    │   │   ├── velocity.json
    │   │   └── configs/
    │   │       ├── solver_config.json
    │   │       └── display_config.json
    │   ├── inputs.json
    │   └── inputs.csv
    └── Study_Velocity/
        ├── 0_inputs/
        ├── inputs.json
        └── inputs.csv
```

At this stage of the **App** execution, the **NUREMICS** terminal prompts that all _fixed_ input data are now properly set, but halts by indicating that datasets of _variable_ input data must be defined (to conduct various experiments).

👤🔄🖥️
```shell
> SETTINGS <

| Study_Shape |
> Common : (V) gravity (V) mass (V) plot_title.txt (V) velocity.json (V) configs

(X) Please define at least one dataset in file :
> .../DEMO_APP/Study_Shape/inputs.csv
```

Let's define three datasets identified as `Test1`, `Test2` and `Test3` within the `input.csv` file.

📄 `inputs.csv`

|  ID   | nb_sides |
|-------|----------|
| Test1 |          |
| Test2 |          |
| Test3 |          |

We can now see that **NUREMICS** has properly identified the three defined datasets, but is waiting for the _variable_ input data `nb_sides` to be set for each of them.

👤🔄🖥️
```shell
 > SETTINGS <

| Study_Shape |
> Common : (V) gravity (V) mass (V) plot_title.txt (V) velocity.json (V) configs
> Test1 : (X) nb_sides
> Test2 : (X) nb_sides
> Test3 : (X) nb_sides

(X) Please set inputs :
> .../DEMO_APP/Study_Shape/inputs.csv
```

Let's thus set some values of `nb_sides` for each defined dataset within the `input.csv` file.

📄 `inputs.csv`

|  ID   | nb_sides |
|-------|----------|
| Test1 |    3     |
| Test2 |    4     |
| Test3 |    5     |

**NUREMICS** finally prompts that all input data are properly set for `Study_Shape`.

👤🔄🖥️
```shell
> SETTINGS <

| Study_Shape |
> Common : (V) gravity (V) mass (V) plot_title.txt (V) velocity.json (V) configs
> Test1 : (V) nb_sides
> Test2 : (V) nb_sides
> Test3 : (V) nb_sides
```

The same job must be done to set the input data for `Study_Velocity`.

👤👁️💾
```bash
<working_dir>/
└── DEMO_APP/
    ├── studies.json
    ├── Study_Shape/
    │   ├── 0_inputs/
    │   │   ├── plot_title.txt
    │   │   ├── velocity.json
    │   │   └── configs/
    │   │       ├── solver_config.json
    │   │       └── display_config.json
    │   ├── inputs.json
    │   └── inputs.csv
    └── Study_Velocity/
        ├── 0_inputs/
        │   ├── 0_datasets/
        │   │   ├── Test1/
        │   │   │   └── velocity.json
        │   │   ├── Test2/
        │   │   │   └── velocity.json
        │   │   └── Test3/
        │   │       └── velocity.json
        │   ├── plot_title.txt
        │   └── configs/
        │       ├── solver_config.json
        │       └── display_config.json
        ├── inputs.json
        └── inputs.csv
```
<br>

Let's here consider the same _fixed_ input data `plot_title.txt` and `configs` as for the previous study `Study_Shape`.

<br>

📄 `inputs.json`
```json
{
    "nb_sides": 5,
    "gravity": -9.81
}
```
<br>

📄 `inputs.csv`

|  ID   |
|-------|
| Test1 |
| Test2 |
| Test3 |

<br>

📄 `0_inputs/0_datasets/Test1/velocity.json`
```json
{
    "v0": 15.0,
    "angle": 45.0
}
```
<br>

📄 `0_inputs/0_datasets/Test2/velocity.json`
```json
{
    "v0": 20.0,
    "angle": 45.0
}
```
<br>

📄 `0_inputs/0_datasets/Test3/velocity.json`
```json
{
    "v0": 20.0,
    "angle": 60.0
}
```
<br>

**NUREMICS** is now prompting that all input data are properly set for both `Study_Shape` and `Study_Velocity`.

👤🔄🖥️
```shell
> SETTINGS <

| Study_Shape |
> Common : (V) gravity (V) mass (V) plot_title.txt (V) velocity.json (V) configs
> Test1 : (V) nb_sides
> Test2 : (V) nb_sides
> Test3 : (V) nb_sides

| Study_Velocity |
> Common : (V) nb_sides (V) gravity (V) plot_title.txt (V) configs
> Test1 : (V) mass (V) velocity.json
> Test2 : (V) mass (V) velocity.json
> Test3 : (V) mass (V) velocity.json
```

### Get Results

At this stage, **NUREMICS** is ready to run all the defined studies and generate the corresponding results.

👤🔄🖥️
```shell
> RUNNING <

| Study_Shape | PolygonGeometryProc | Test1 |
> n_sides = 3
> radius = 0.5
> title_file = .../DEMO_APP/Study_Shape/0_inputs/plot_title.txt
>>> START
COMPLETED <<<

| Study_Shape | PolygonGeometryProc | Test2 |
> n_sides = 4
> radius = 0.5
> title_file = .../DEMO_APP/Study_Shape/0_inputs/plot_title.txt
>>> START
COMPLETED <<<

| Study_Shape | PolygonGeometryProc | Test3 |
> n_sides = 5
> radius = 0.5
> title_file = .../DEMO_APP/Study_Shape/0_inputs/plot_title.txt
>>> START
COMPLETED <<<

| Study_Shape | ProjectileModelProc | Test1 |
> gravity = -9.81
> mass = 0.1
> velocity_file = .../DEMO_APP/Study_Shape/0_inputs/velocity.json
> configs_folder = .../DEMO_APP/Study_Shape/0_inputs/configs
> coords_file = .../DEMO_APP/Study_Shape/1_PolygonGeometryProc/Test1/points_coordinates.csv
>>> START
COMPLETED <<<

| Study_Shape | ProjectileModelProc | Test2 |
> gravity = -9.81
> mass = 0.1
> velocity_file = .../DEMO_APP/Study_Shape/0_inputs/velocity.json
> configs_folder = .../DEMO_APP/Study_Shape/0_inputs/configs
> coords_file = .../DEMO_APP/Study_Shape/1_PolygonGeometryProc/Test2/points_coordinates.csv
>>> START
COMPLETED <<<

| Study_Shape | ProjectileModelProc | Test3 |
> gravity = -9.81
> mass = 0.1
> velocity_file = .../DEMO_APP/Study_Shape/0_inputs/velocity.json
> configs_folder = .../DEMO_APP/Study_Shape/0_inputs/configs
> coords_file = .../DEMO_APP/Study_Shape/1_PolygonGeometryProc/Test3/points_coordinates.csv
>>> START
COMPLETED <<<

| Study_Velocity | PolygonGeometryProc |
> n_sides = 5
> radius = 0.5
> title_file = .../DEMO_APP/Study_Velocity/0_inputs/plot_title.txt
>>> START
COMPLETED <<<

| Study_Velocity | ProjectileModelProc | Test1 |
> gravity = -9.81
> mass = 0.1
> configs_folder = .../DEMO_APP/Study_Velocity/0_inputs/configs
> velocity_file = .../DEMO_APP/Study_Velocity/0_inputs/0_datasets/Test1/velocity.json
> coords_file = .../DEMO_APP/Study_Velocity/1_PolygonGeometryProc/points_coordinates.csv
>>> START
COMPLETED <<<

| Study_Velocity | ProjectileModelProc | Test2 |
> gravity = -9.81
> mass = 0.1
> configs_folder = .../DEMO_APP/Study_Velocity/0_inputs/configs
> velocity_file = .../DEMO_APP/Study_Velocity/0_inputs/0_datasets/Test2/velocity.json
> coords_file = .../DEMO_APP/Study_Velocity/1_PolygonGeometryProc/points_coordinates.csv
>>> START
COMPLETED <<<

| Study_Velocity | ProjectileModelProc | Test3 |
> gravity = -9.81
> mass = 0.1
> configs_folder = .../DEMO_APP/Study_Velocity/0_inputs/configs
> velocity_file = .../DEMO_APP/Study_Velocity/0_inputs/0_datasets/Test3/velocity.json
> coords_file = .../DEMO_APP/Study_Velocity/1_PolygonGeometryProc/points_coordinates.csv
>>> START
COMPLETED <<<
```

The operator can then access the results in the output database generated by **NUREMICS** within the data tree. 

👤👁️💾
```bash
<working_dir>/
└── DEMO_APP/
    ├── studies.json
    ├── Study_Shape/
    │   ├── 1_PolygonGeometryProc/
    │   │   ├── Test1/
    │   │   │   ├── points_coordinates.csv
    │   │   │   └── polygon_shape.png
    │   │   ├── Test2/
    │   │   │   ├── points_coordinates.csv
    │   │   │   └── polygon_shape.png
    │   │   └── Test3/
    │   │       ├── points_coordinates.csv
    │   │       └── polygon_shape.png
    │   └── 2_ProjectileModelProc/
    │       ├── Test1/
    │       │   └── comparison/
    │       │       ├── results.xlsx
    │       │       └── model_vs_theory.png
    │       ├── Test2/
    │       │   └── comparison/
    │       │       ├── results.xlsx
    │       │       └── model_vs_theory.png
    │       └── Test3/
    │           └── comparison/
    │               ├── results.xlsx
    │               └── model_vs_theory.png
    └── Study_Velocity/
        ├── 1_PolygonGeometryProc/
        │   ├── points_coordinates.csv
        │   └── polygon_shape.png
        └── 2_ProjectileModelProc/
            ├── Test1/
            │   └── comparison/
            │       ├── results.xlsx
            │       └── model_vs_theory.png
            ├── Test2/
            │   └── comparison/
            │       ├── results.xlsx
            │       └── model_vs_theory.png
            └── Test3/
                └── comparison/
                    ├── results.xlsx
                    └── model_vs_theory.png
```