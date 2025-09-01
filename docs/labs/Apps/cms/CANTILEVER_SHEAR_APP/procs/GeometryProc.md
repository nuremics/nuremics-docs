<p align="left">
  <img src="https://img.shields.io/badge/CadQuery-2.5.2+-2980b9" />
</p>

## Process

Create a geometric representation of a physical system.<br>
A/ **`create_geometry`:** Create and export a simple geometric entity (beam, plate, or block) in STEP or BREP format.

```mermaid
erDiagram
  **Parameters** ||--|| **Inputs** : provides
  **Paths** ||--|| **Inputs** : provides
  **Inputs** ||--|| **GeometryProc** : feeds
  **GeometryProc** ||--|| **Outputs** : generates

  **Parameters** {
    int dim
    float length
    float width
    float height
  }
  **Paths** {
    _ _ "_"
  }
  **GeometryProc** {
    op create_geometry
  }
  **Outputs** {
    file outfile "step/brep"
  }
```

## Input Parameter(s)

- **`dim`:** Dimension of the geometry: 1 for a line (beam), 2 for a rectangle (plate), 3 for a box (block).
- **`length`:** Length of the geometry along the X axis.
- **`width`:** Width of the geometry along the Y axis (only used if dim = 2|3).
- **`height`:** Height of the geometry along the Z axis (only used if dim = 3).

## Output Path(s)

- **`outfile`:** File containing the created geometry (in .step if `dim` = 3|2 or .brep if `dim` = 1).

---

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="https://github.com/nuremics/nuremics-labs/tree/cantilever-shear/src/labs/apps/cms/CANTILEVER_SHEAR_APP/procs/GeometryProc"
     target="_blank"
     rel="noopener noreferrer"
     class="md-button md-button--primary">
    View source code
  </a>
</div>

---