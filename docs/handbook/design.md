# Design

Let's introduce the core design patterns behind **Procs** and **Apps** in **nuRemics**.

## Proc

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

## App

A final end-user **App** can be built by plugging together previously implemented **Procs**, and specifying their sequential order of execution within the workflow.

```mermaid
flowchart RL
  Proc1[<b>PolygonGeometryProc<b>] e1@--1--o App[<b>DEMO_APP<b>]
  Proc2[<b>ProjectileModelProc<b>] e2@--2--o App
  Proc3[<b>TrajectoryAnalysisProc<b>] e3@--3--o App
  Op11[<b>generate_polygon_shape<b>] e4@--A--o Proc1
  Op12[<b>plot_polygon_shape<b>] e5@--B--o Proc1
  Op21[<b>simulate_projectile_motion<b>] e6@--A--o Proc2
  Op22[<b>calculate_analytical_trajectory<b>] e7@--B--o Proc2
  Op23[<b>compare_model_vs_analytical_trajectories<b>] e8@--C--o Proc2
  Op31[<b>plot_overall_model_vs_theory<b>] e9@--A--o Proc3
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
  subgraph Inputs[<b>INPUTS<b>]
    direction TB

    subgraph Paths[<b>Paths<b>]
      direction LR
      path1["plot_title.txt <i>(file)<i>"]
      path2["velocity.json <i>(file)<i>"]
      path3["configs <i>(folder)<i>"]
    end

    subgraph Parameters[<b>Parameters<b>]
      direction LR
      param1["nb_sides <i>(int)<i>"]
      param2["gravity <i>(float)<i>"]
      param3["mass <i>(float)<i>"]
    end
  end

  subgraph App[<b>DEMO_APP<b>]
    direction RL
    proc1["PolygonGeometryProc"]
    proc2["ProjectileModelProc"]
  end

  subgraph Outputs[<b>OUTPUTS<b>]
    direction RL
    out1["points_coordinates.csv <i>(file)<i>"]
    out2["polygon_shape.png <i>(file)<i>"]
    out3["comparison <i>(folder)<i>"]
  end

  Inputs --> App
  App --> Outputs
```

It is also insightful for the end-user to present this I/O interface by showing which **INPUTS** are used by each **Proc** of the **App**, and which **OUTPUTS** are written by each of them.

```mermaid
flowchart LR
  subgraph Inputs[<b>INPUTS<b>]
    direction TB

    subgraph Paths[<b>Paths<b>]
      direction LR
      path1["plot_title.txt <i>(file)<i>"]
    end

    subgraph Parameters[<b>Parameters<b>]
      direction LR
      param1["nb_sides <i>(int)<i>"]
    end
  end

  subgraph App[<b>DEMO_APP<b>]
    direction RL
    proc1["PolygonGeometryProc"]
  end

  subgraph Outputs[<b>OUTPUTS<b>]
    direction RL
    out1["points_coordinates.csv <i>(file)<i>"]
    out2["polygon_shape.png <i>(file)<i>"]
  end

  Inputs --> proc1
  proc1 --> Outputs
```

```mermaid
flowchart LR
  subgraph Inputs[<b>INPUTS<b>]
    direction TB

    subgraph Paths[<b>Paths<b>]
      direction LR
      path2["velocity.json <i>(file)<i>"]
      path3["configs <i>(folder)<i>"]
      out1["points_coordinates.csv <i>(file)<i>"]
    end

    subgraph Parameters[<b>Parameters<b>]
      direction LR
      param2["gravity <i>(float)<i>"]
      param3["mass <i>(float)<i>"]
    end
  end

  subgraph App[<b>DEMO_APP<b>]
    direction RL
    proc2["ProjectileModelProc"]
  end

  subgraph Outputs[<b>OUTPUTS<b>]
    direction RL
    out3["comparison <i>(folder)<i>"]
  end

  Inputs --> proc2
  proc2 --> Outputs

  classDef blueBox fill:#d0e6ff,stroke:#339,stroke-width:1.5px;
  class out1 blueBox;
```

---

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="../usability/" class="md-button md-button--primary">
    Usability
  </a>
</div>

---