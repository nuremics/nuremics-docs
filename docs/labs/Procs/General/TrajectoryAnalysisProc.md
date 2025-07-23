## Process

Compare simulated (model) and theoretical trajectories of a projectile across all experiments.<br>
A/ **`plot_overall_model_vs_theory`:** Generate overall comparative plot of simulated (model) and theoritical trajectories.

```mermaid
erDiagram
  **Analysis** ||--|| **Inputs** : provides
  **Inputs** ||--|| **TrajectoryAnalysisProc** : feeds
  **TrajectoryAnalysisProc** ||--|| **Outputs** : generates

  **Analysis** {
    folder comp_folder "_"
  }
  **TrajectoryAnalysisProc** {
    op plot_overall_model_vs_theory
  }
  **Outputs** {
    file fig_file "png"
  }
```

## Input Analysis

- **`comp_folder/`**<br>
  **`results.xlsx`:** File containing simulated (model) and theoritical trajectories.

## Output Path(s)

- **`fig_file`:** Image comparing both trajectories across all experiments.

---

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="" class="md-button md-button--primary">
    View source code
  </a>
</div>