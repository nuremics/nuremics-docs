# Theory

## Design Patterns

Let’s introduce the core design patterns behind **Procs** and **Apps** in **NUREMICS**.

### Proc

A **Proc** can be seen as an algorithmic box which processes some input data and produces corresponding output data.

The input data typically fall into two main categories:

- **Input parameters**: Scalar values such as `float`, `int`, `bool`, or `str`.

- **Input paths**: Files or folders provided as `Path` objects (from Python's `pathlib` module), pointing to structured data on disk.

```mermaid
erDiagram
  **Parameters** ||--|| **Inputs** : provides
  **Paths** ||--|| **Inputs** : provides

  **Parameters** {
    float radius
    int n_sides
  }
  **Paths** {
    file title_file "txt"
  }
```

As mentioned in the [Handbook Overview](index.md){:target="_blank"}, the algorithmic box of the **Proc** is a class composed of functions (**Op**) called sequentially within its `__call__` method.

```mermaid
erDiagram
  **Parameters** ||--|| **Inputs** : provides
  **Paths** ||--|| **Inputs** : provides
  **Inputs** ||--|| **PolygonGeometryProc** : feeds

  **Parameters** {
    float radius
    int n_sides
  }
  **Paths** {
    file title_file "txt"
  }
  **PolygonGeometryProc** {
    op generate_polygon_shape
    op plot_polygon_shape
  }
```

Output data are typically expressed as `Path` objects as well, corresponding to files or folders written to disk during the execution of the **Proc**.

```mermaid
erDiagram
  **Parameters** ||--|| **Inputs** : provides
  **Paths** ||--|| **Inputs** : provides
  **Inputs** ||--|| **PolygonGeometryProc** : feeds
  **PolygonGeometryProc** ||--|| **Outputs** : generates

  **Parameters** {
    float radius
    int n_sides
  }
  **Paths** {
    file title_file "txt"
  }
  **PolygonGeometryProc** {
    op generate_polygon_shape
    op plot_polygon_shape
  }
  **Outputs** {
    file coords_file "csv"
    file fig_file "png"
  }
```

For the sake of example, let's define another **Proc** considering the same structure.

```mermaid
erDiagram
  **Parameters** ||--|| **Inputs** : provides
  **Paths** ||--|| **Inputs** : provides
  **Inputs** ||--|| **ProjectileModelProc** : feeds
  **ProjectileModelProc** ||--|| **Outputs** : generates

  **Parameters** {
    float gravity
    float mass
  }
  **Paths** {
    file velocity_file "json"
    folder configs_folder "_"
    file coords_file "csv"
  }
  **ProjectileModelProc** {
    op simulate_projectile_motion
    op calculate_analytical_trajectory
    op compare_model_vs_analytical_trajectories
  }
  **Outputs** {
    folder comp_folder "_"
  }
```

### App

A final end-user **App** can be built by plugging together previously implemented **Procs**, and specifying their sequential order of execution within the workflow.

```mermaid
flowchart RL
  **PolygonGeometryProc** e1@--1--o **DEMO_APP**
  **ProjectileModelProc** e2@--2--o **DEMO_APP**
  **TrajectoryAnalysisProc** e3@--3--o **DEMO_APP**
  **generate_polygon_shape** e4@--A--o **PolygonGeometryProc**
  **plot_polygon_shape** e5@--B--o **PolygonGeometryProc**
  **simulate_projectile_motion** e6@--A--o **ProjectileModelProc**
  **calculate_analytical_trajectory** e7@--B--o **ProjectileModelProc**
  **compare_model_vs_analytical_trajectories** e8@--C--o **ProjectileModelProc**
  **plot_overall_model_vs_theory** e9@--A--o **TrajectoryAnalysisProc**
  e1@{ animate: true }
  e2@{ animate: true }
  e3@{ animate: true }
  e4@{ animate: true }
  e5@{ animate: true }
  e6@{ animate: true }
  e7@{ animate: true }
  e8@{ animate: true }
  e9@{ animate: true }
```

Each **Proc** integrated into the **App** defines its own set of inputs and outputs, specific to its internal algorithmic logic. When these **Procs** are assembled into a workflow, the **App** itself exposes a higher-level set of inputs and outputs. These define the I/O interface presented to the end-user, who provides the necessary input data and retrieves the final results upon execution.

The assembly step is performed through a mapping between the internal I/O data of each **Proc** and the global I/O interface of the **App**. This mapping mechanism serves multiple purposes:

- It defines which data are exposed to the end-user (and how they are displayed) and which remain internal to the workflow.

- It manages the data dependencies between **Procs**, when the output of one **Proc** is used as input for another.

This notably ensures a coherent and seamless management of data across the workflow, while delivering a clean and focused I/O interface tailored to the user's needs.

The mapping between a **Proc** and the **App** starts by specifying which **Proc** input parameters are exposed to the end-user, and how they are labeled in the **App** input interface.

```mermaid
erDiagram
  **DEMO_APP** ||--|| **user_params** : mapping
  **user_params** ||--|| **PolygonGeometryProc** : mapping

  **user_params** {
    int n_sides "nb_sides"
  }
```

The **Proc** input parameters that remain internal to the workflow are assigned fixed values directly within the mapping definition, without being exposed to the end-user.

```mermaid
erDiagram
  **DEMO_APP** ||--|| **user_params** : mapping
  **DEMO_APP** ||--|| **hard_params** : mapping
  **user_params** ||--|| **PolygonGeometryProc** : mapping
  **hard_params** ||--|| **PolygonGeometryProc** : mapping

  **user_params** {
    int n_sides "nb_sides"
  }
  **hard_params** {
    float radius "0.5"
  }
```

The **Proc** input paths that need to be provided by the end-user are specified by defining the expected file or folder names within the **App** input interface.

```mermaid
erDiagram
  **DEMO_APP** ||--|| **user_params** : mapping
  **DEMO_APP** ||--|| **hard_params** : mapping
  **DEMO_APP** ||--|| **user_paths** : mapping
  **user_params** ||--|| **PolygonGeometryProc** : mapping
  **hard_params** ||--|| **PolygonGeometryProc** : mapping
  **user_paths** ||--|| **PolygonGeometryProc** : mapping

  **user_params** {
    int n_sides "nb_sides"
  }
  **hard_params** {
    float radius "0.5"
  }
  **user_paths** {
    file title_file "plot_title.txt"
  }
```

The **Proc** input paths can also be mapped to output paths produced by a previous **Proc** within the workflow (although this does not apply here, as we are currently focusing on the first **Proc** in the workflow).

```mermaid
erDiagram
  **DEMO_APP** ||--|| **user_params** : mapping
  **DEMO_APP** ||--|| **hard_params** : mapping
  **DEMO_APP** ||--|| **user_paths** : mapping
  **DEMO_APP** ||--|| **required_paths** : mapping
  **user_params** ||--|| **PolygonGeometryProc** : mapping
  **hard_params** ||--|| **PolygonGeometryProc** : mapping
  **user_paths** ||--|| **PolygonGeometryProc** : mapping
  **required_paths** ||--|| **PolygonGeometryProc** : mapping

  **user_params** {
    int n_sides "nb_sides"
  }
  **hard_params** {
    float radius "0.5"
  }
  **user_paths** {
    file title_file "plot_title.txt"
  }
  **required_paths** {
    _ _ "_"
  }
```

Finally, the **Proc** output paths are specified by indicating the name of the file(s) or folder(s) that will be written by the **Proc** during the workflow execution.

```mermaid
erDiagram
  **DEMO_APP** ||--|| **user_params** : mapping
  **DEMO_APP** ||--|| **hard_params** : mapping
  **DEMO_APP** ||--|| **user_paths** : mapping
  **DEMO_APP** ||--|| **required_paths** : mapping
  **DEMO_APP** ||--|| **output_paths** : mapping
  **user_params** ||--|| **PolygonGeometryProc** : mapping
  **hard_params** ||--|| **PolygonGeometryProc** : mapping
  **user_paths** ||--|| **PolygonGeometryProc** : mapping
  **required_paths** ||--|| **PolygonGeometryProc** : mapping
  **output_paths** ||--|| **PolygonGeometryProc** : mapping

  **user_params** {
    int n_sides "nb_sides"
  }
  **hard_params** {
    float radius "0.5"
  }
  **user_paths** {
    file title_file "plot_title.txt"
  }
  **required_paths** {
    _ _ "_"
  }
  **output_paths** {
    file coords_file "points_coordinates.csv"
    file fig_file "polygon_shape.png"
  }
```

Let's now assemble the second **Proc** to be executed by the **App** within the workflow, by establishing a dependency: the output data produced by the first **Proc** will serve as input data for this second one.

```mermaid
erDiagram
  **DEMO_APP** ||--|| **user_params** : mapping
  **DEMO_APP** ||--|| **hard_params** : mapping
  **DEMO_APP** ||--|| **user_paths** : mapping
  **DEMO_APP** ||--|| **required_paths** : mapping
  **DEMO_APP** ||--|| **output_paths** : mapping
  **user_params** ||--|| **ProjectileModelProc** : mapping
  **hard_params** ||--|| **ProjectileModelProc** : mapping
  **user_paths** ||--|| **ProjectileModelProc** : mapping
  **required_paths** ||--|| **ProjectileModelProc** : mapping
  **output_paths** ||--|| **ProjectileModelProc** : mapping

  **user_params** {
    float gravity "gravity"   
    float mass "mass"
  }
  **hard_params** {
    _ _ "_"
  }
  **user_paths** {
    file velocity_file "velocity.json"
    folder configs_folder "configs"
  }
  **required_paths** {
    file coords_file "points_coordinates.csv"
  }
  **output_paths** {
    folder comp_folder "comparison"
  }
```

Once all **Procs** have been assembled into the **App**, the final I/O interface presented to the end-user emerges.

```mermaid
flowchart LR
  subgraph **INPUTS**
    direction TB

    subgraph **Paths**
      direction LR
      path1["plot_title.txt _(file)_"]
      path2["velocity.json _(file)_"]
      path3["configs _(folder)_"]
    end

    subgraph **Parameters**
      direction LR
      param1["nb_sides _(int)_"]
      param2["gravity _(float)_"]
      param3["mass _(float)_"]
    end
  end

  subgraph **DEMO_APP**
    direction RL
    proc1["PolygonGeometryProc"]
    proc2["ProjectileModelProc"]
  end

  subgraph **OUTPUTS**
    direction RL
    out1["points_coordinates.csv _(file)_"]
    out2["polygon_shape.png _(file)_"]
    out3["comparison _(folder)_"]
  end

  **INPUTS** --> **DEMO_APP**
  **DEMO_APP** --> **OUTPUTS**
```

It is also insightful for the end-user to present this I/O interface by showing which **INPUTS** are used by each **Proc** of the **App**, and which **OUTPUTS** are written by each of them.

```mermaid
flowchart LR
  subgraph **INPUTS**
    direction TB

    subgraph **Paths**
      direction LR
      path1["plot_title.txt _(file)_"]
    end

    subgraph **Parameters**
      direction LR
      param1["nb_sides _(int)_"]
    end
  end

  subgraph **DEMO_APP**
    direction RL
    proc1["PolygonGeometryProc"]
  end

  subgraph **OUTPUTS**
    direction RL
    out1["points_coordinates.csv _(file)_"]
    out2["polygon_shape.png _(file)_"]
  end

  **INPUTS** --> proc1
  proc1 --> **OUTPUTS**
```

```mermaid
flowchart LR
  subgraph **INPUTS**
    direction TB

    subgraph **Paths**
      direction LR
      path2["velocity.json _(file)_"]
      path3["configs _(folder)_"]
      out1["points_coordinates.csv _(file)_"]
    end

    subgraph **Parameters**
      direction LR
      param2["gravity _(float)_"]
      param3["mass _(float)_"]
    end
  end

  subgraph **DEMO_APP**
    direction RL
    proc2["ProjectileModelProc"]
  end

  subgraph **OUTPUTS**
    direction RL
    out3["comparison _(folder)_"]
  end

  **INPUTS** --> proc2
  proc2 --> **OUTPUTS**

  classDef blueBox fill:#d0e6ff,stroke:#339,stroke-width:1.5px;
  class out1 blueBox;
```

## Usability

The **Apps** built with **NUREMICS** come with a lean and pragmatic user interface by design. No flashy GUI, but instead, the focus is on simplicity and efficiency:

- An input database that the operator completes by editing configuration files and uploading the required input files and folders.

- A terminal interface that provides informative feedback at each execution, clearly indicating what the **App** is doing and what actions are expected from the operator.

- An output database that stores all results in a well-structured and traceable folder hierarchy.

```mermaid
sequenceDiagram
    actor Operator
    Operator->>DEMO_APP: Execution
    DEMO_APP->>INPUTS: Initialize database
    DEMO_APP->>Operator: Terminal feedback
    Operator->>INPUTS: Complete database
    Operator->>DEMO_APP: Execution
    DEMO_APP->>INPUTS: Read database
    DEMO_APP->>OUTPUTS: Write database
    DEMO_APP->>Operator: Terminal feedback
    Operator->>OUTPUTS: Access results
```

This streamlined approach prioritizes clarity, control, and reproducibility, making each **App** built with **NUREMICS** well-suited for both direct interaction by end-users and seamless integration into larger software ecosystems. In such environments, **NUREMICS** can operate as a backend computational engine, interacting programmatically with other tools (such as web applications) that provide their own user interfaces.

### Configuration

When running an **App**, the operator first defines a set of studies aimed at exploring the **INPUTS** space and analyzing the outcomes in the **OUTPUTS** space.

```mermaid
flowchart LR
    Study_Shape
    Study_Velocity
```

The operator then configures each study by selecting which inputs stay constant _(Fixed)_ and which ones change _(Variable)_ across the various experiments.

```mermaid
flowchart LR

  subgraph Fixed1["**Fixed**"]
    direction TB

    subgraph Paths_Fixed1["**Paths**"]
      direction LR
      path1_1["plot_title.txt"]
      path2_1["velocity.json"]
      path3_1["configs"]
    end

    subgraph Parameter_Fixed1["**Parameters**"]
      direction LR
      param2_1["gravity"]
      param3_1["mass"]
    end
  end

  subgraph Variable1["**Variable**"]
    direction TB

    subgraph Paths_Variable1["**Paths**"]
      direction LR
      no_path["_"]
    end

    subgraph Parameter_Variable1["**Parameters**"]
      direction LR
      param1_1["nb_sides"]
    end
  end

  Study_Shape --> Fixed1
  Study_Shape --> Variable1

  subgraph Fixed2["**Fixed**"]
    direction TB

    subgraph Paths_Fixed2["**Paths**"]
      direction LR
      path1_2["plot_title.txt"]
      path3_2["configs"]
    end

    subgraph Parameter_Fixed2["**Parameters**"]
      direction LR
      param1_2["nb_sides"]
      param2_2["gravity"]
      param3_2["mass"]
    end
  end

  subgraph Variable2["**Variable**"]
    direction TB

    subgraph Paths_Variable2["**Paths**"]
      direction LR
      path2_2["velocity.json"]
    end

    subgraph Parameter_Variable2["**Parameters**"]
      direction LR
      no_param["_"]
    end
  end

  Study_Velocity --> Fixed2
  Study_Velocity --> Variable2
```

### Settings

To conduct experiments, the operator assigns values for both fixed and variable inputs: fixed inputs remain constant across all experiments _(Common)_, while variable inputs are adjusted from one experiment to another _(Test1, Test2, ...)_.

```mermaid
flowchart LR
    Study_Shape --> Study1_Common["Common"]
    Study_Shape --> Study1_Test1["Test1"]
    Study_Shape --> Study1_Test2["Test2"]
    Study_Shape --> Study1_Test3["..."]
    
    Study1_Common --> common1_param2["gravity = ..."]
    Study1_Common --> common1_param3["mass = ..."]
    Study1_Common --> common1_input1["plot_title.txt _(uploaded)_"]
    Study1_Common --> common1_input2["velocity.json _(uploaded)_"]
    Study1_Common --> common1_input3["configs _(uploaded)_"]

    Study1_Test1 --> test1_param1["nb_sides = ..."]
    Study1_Test2 --> test2_param1["nb_sides = ..."]
    Study1_Test3 --> test3_param1["nb_sides = ..."]

    Study_Velocity --> Study2_Common["Common"]
    Study_Velocity --> Study2_Test1["Test1"]
    Study_Velocity --> Study2_Test2["Test2"]
    Study_Velocity --> Study2_Test3["..."]
    
    Study2_Common --> common2_param1["nb_sides = ..."]
    Study2_Common --> common2_param2["gravity = ..."]
    Study2_Common --> common2_param3["mass = ..."]
    Study2_Common --> common2_input1["plot_title.txt _(uploaded)_"]
    Study2_Common --> common2_input3["configs _(uploaded)_"]

    Study2_Test1 --> test1_path2["velocity.json _(uploaded)_"]
    Study2_Test2 --> test2_path2["velocity.json _(uploaded)_"]
    Study2_Test3 --> test3_path2["velocity.json _(uploaded)_"]
```

### Results

At the end of the execution, results are stored in a structured output tree, ready for review or further processing. The outputs are first organized by **Proc**, each of them writing its own result data. Within each **Proc**, the results are further subdivided by experiment _(Test1, Test2, ...)_, ensuring a clear separation and traceability of outcomes across the entire study.

This organization is automatically determined based on how the study is configured by the operator. **NUREMICS** analyzes which input data are marked as _fixed_ or _variable_, and how they connect to the internal workflow of the **App**. If a **Proc** directly depends on _variable_ inputs, or indirectly through upstream dependencies, it will generate distinct outputs for each experiment. Otherwise, it will produce shared outputs only once.

This logic ensures that only the necessary parts of the workflow are repeated through experimentations, and that the output structure faithfully reflects the configuration of the study along with the internal dependencies within the workflow.

```mermaid
flowchart LR
    Study_Shape --> Study1_PolygonGeometryProc["PolygonGeometryProc"]
    Study_Shape --> Study1_ProjectileModelProc["ProjectileModelProc"]

    Study1_PolygonGeometryProc --> Study1_PolygonGeometryProc_Test1["Test1"]
    Study1_PolygonGeometryProc --> Study1_PolygonGeometryProc_Test2["Test2"]
    Study1_PolygonGeometryProc --> Study1_PolygonGeometryProc_Test3["..."]

    Study1_PolygonGeometryProc_Test1 --> Study1_PolygonGeometryProc_Test1_output1["points_coordinates.csv"]
    Study1_PolygonGeometryProc_Test1 --> Study1_PolygonGeometryProc_Test1_output2["polygon_shape.png"]
    Study1_PolygonGeometryProc_Test2 --> Study1_PolygonGeometryProc_Test2_output1["points_coordinates.csv"]
    Study1_PolygonGeometryProc_Test2 --> Study1_PolygonGeometryProc_Test2_output2["polygon_shape.png"]
    Study1_PolygonGeometryProc_Test3 --> Study1_PolygonGeometryProc_Test3_output1["points_coordinates.csv"]
    Study1_PolygonGeometryProc_Test3 --> Study1_PolygonGeometryProc_Test3_output2["polygon_shape.png"]

    Study1_ProjectileModelProc --> Study1_ProjectileModelProc_Test1["Test1"]
    Study1_ProjectileModelProc --> Study1_ProjectileModelProc_Test2["Test2"]
    Study1_ProjectileModelProc --> Study1_ProjectileModelProc_Test3["..."]

    Study1_ProjectileModelProc_Test1 --> Study1_ProjectileModelProc_Test1_output3["comparison"]
    Study1_ProjectileModelProc_Test2 --> Study1_ProjectileModelProc_Test2_output3["comparison"]
    Study1_ProjectileModelProc_Test3 --> Study1_ProjectileModelProc_Test3_output3["comparison"]
    
    Study_Velocity --> Study2_PolygonGeometryProc["PolygonGeometryProc"]
    Study_Velocity --> Study2_ProjectileModelProc["ProjectileModelProc"]

    Study2_PolygonGeometryProc --> Study2_PolygonGeometryProc_Common_output1["points_coordinates.csv"]
    Study2_PolygonGeometryProc --> Study2_PolygonGeometryProc_Common_output2["polygon_shape.png"]

    Study2_ProjectileModelProc --> Study2_ProjectileModelProc_Test1["Test1"]
    Study2_ProjectileModelProc --> Study2_ProjectileModelProc_Test2["Test2"]
    Study2_ProjectileModelProc --> Study2_ProjectileModelProc_Test3["..."]

    Study2_ProjectileModelProc_Test1 --> Study2_ProjectileModelProc_Test1_output3["comparison"]
    Study2_ProjectileModelProc_Test2 --> Study2_ProjectileModelProc_Test2_output3["comparison"]
    Study2_ProjectileModelProc_Test3 --> Study2_ProjectileModelProc_Test3_output3["comparison"]
```

---

<div align="center" style="font-weight: bold; font-size: 1.0rem;">
Explore NUREMICS in:
</div>

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="../practice/" class="md-button md-button--primary">
    Practice
  </a>
</div>

---