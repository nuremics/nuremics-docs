## Process

Simulate a projectile trajectory and compare it with the analytical solution.<br>
A/ **`simulate_projectile_motion`:** Run the physical simulation of a projectile.<br>
B/ **`calculate_analytical_trajectory`:** Compute the theoretical trajectory using analytical equations.<br>
C/ **`compare_model_vs_analytical_trajectories`:** Plot and save the comparison between simulated (model) and theoretical trajectories.

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

## Input Parameter(s)

- **`gravity`:** Acceleration due to gravity (can be positive or negative).
- **`mass`:** Mass of the projectile (used in the simulation).

## Input Path(s)

- **`velocity_file`:** File containing the velocity initial conditions (v0, angle).
- **`configs_folder/`**<br>
  **`solver_config.json`:** File containing the parameters for solver configuration.<br>
  **`display_config.json`:** File containing the parameters for display configuration.
- **`coords_file`:** File containing the X/Y coordinates of the polygonal shape to simulate.

## Output Path(s)

- **`comp_folder/`**<br>
  **`results.xlsx`:** File containing simulated (model) and theoritical trajectories.<br>
  **`model_vs_theory.png`:** Image comparing both trajectories.

---

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="" class="md-button md-button--primary">
    View source code
  </a>
</div>