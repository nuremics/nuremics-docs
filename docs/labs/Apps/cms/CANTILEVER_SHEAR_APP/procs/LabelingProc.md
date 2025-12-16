<p align="left">
  <img src="https://img.shields.io/badge/PyQt6-6.9.1-000000" />
</p>

## Process

Define and label the entities of a physical system from its geometric representation.<br>
A/ **`label_boundaries`:** Assign labels to the boundaries of a geometric model.

```mermaid
erDiagram
  **Parameters** ||--|| **Inputs** : provides
  **Paths** ||--|| **Inputs** : provides
  **Inputs** ||--|| **GeometryProc** : feeds
  **GeometryProc** ||--|| **Outputs** : generates

  **Parameters** {
    int dim
  }
  **Paths** {
    file infile "step/brep"
  }
  **GeometryProc** {
    op create_geometry
  }
  **Outputs** {
    file outfile "json"
  }
```

## Input Parameter(s)

- **`dim`:** Dimension of the geometry: 1 for a line (beam), 2 for a rectangle (plate), 3 for a box (block).

## Input Path(s)

- **`infile`:** File containing the geometric model (in .step if `dim` = 3|2 or .brep if `dim` = 1).

## Output Path(s)

- **`outfile`:** File containing the labeled geometric entities.

---

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="https://github.com/nuremics/nuremics-labs/tree/cantilever-shear/src/labs/apps/cms/CANTILEVER_SHEAR_APP/procs/LabelingProc"
     target="_blank"
     rel="noopener noreferrer"
     class="md-button md-button--primary">
    View source code
  </a>
</div>

---