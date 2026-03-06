# Usability

The **Apps** built with **nuRemics** come with a lean and pragmatic user interface by design. No flashy GUI, but instead, the focus is on simplicity and efficiency:

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

This streamlined approach prioritizes clarity, control, and reproducibility, making each **App** built with **nuRemics** well-suited for both direct interaction by end-users and seamless integration into larger software ecosystems. In such environments, **nuRemics** can operate as a backend computational engine, interacting programmatically with other tools (such as web applications) that provide their own user interfaces.

## Configuration

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

## Results

At the end of the execution, results are stored in a structured output tree, ready for review or further processing. The outputs are first organized by **Proc**, each of them writing its own result data. Within each **Proc**, the results are further subdivided by experiment _(Test1, Test2, ...)_, ensuring a clear separation and traceability of outcomes across the entire study.

This organization is automatically determined based on how the study is configured by the operator. **nuRemics** analyzes which input data are marked as _fixed_ or _variable_, and how they connect to the internal workflow of the **App**. If a **Proc** directly depends on _variable_ inputs, or indirectly through upstream dependencies, it will generate distinct outputs for each experiment. Otherwise, it will produce shared outputs only once.

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

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="../design/" class="md-button md-button--primary">
    Design
  </a>
</div>

---