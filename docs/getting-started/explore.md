# Explore

Let's dive deeper by conducting our own scientific exploration. We now analyze how gravity affects the trajectory of the projectile by configuring a custom study that simulates the same projectile on different planets (the Earth, the Moon and Mars):

1. **Declare a new study** <br>
Create a new study named `Study_Gravity`.

2. **Configure the variable and fixed inputs** <br>
Set the `gravity` input parameter as variable and keep all other inputs fixed.

3. **Define the input datasets** <br>
Create three distinct input datasets to test different gravity values:
    - `Earth`: Set the parameter to the gravity of the Earth (`-9.81` m/s², which is already the default value).
    - `Moon`: Set the parameter to the gravity of the Moon (`-1.62` m/s²).
    - `Mars`: Set the parameter to the gravity of Mars (`-3.72` m/s²).

4. **Manage the execution flow** <br>
To focus strictly on our new exploration, we can deactivate the execution of `Study_Shape` and `Study_Velocity`.

5. **Enable silent mode** <br>
Configure the software processes (`PolygonGeometryProc`, `ProjectileModelProc`, and `TrajectoryAnalysisProc`) to run in _silent mode_. This prevents interactive windows from popping up, allowing the application to execute all computations quietly in the background.

6. **Run and review** <br>
Run the application. Once the computations are complete, access the results for `Study_Gravity` to compare the various trajectories of the projectile across the three different environments.

7. **Refine the analysis** <br>
By default, all curves in the `overall_comparisons.png` output are plotted using the same style (red circular markers). To better distinguish them, we can customize the style of each curve in the results settings of the `TrajectoryAnalysisProc` (the software process that generates this specific output). Let's then adjust the settings as follows:
    - For the `Earth`:
        - `marker`: `D`
        - `markersize`: `6`
        - `markevery`: `40`
    - For the `Moon`:
        - `color`: `limegreen`
        - `marker`: `v`
        - `markevery`: `50`
    - For `Mars`:
        - `color`: `dodgerblue`
        - `marker`: `X`
        - `markevery`: `50`

8. **Restart the analysis** <br>
To update the `overall_comparisons.png` plot without re-running the entire study, we can deactivate the execution of `PolygonGeometryProc` and `ProjectileModelProc` and relaunch the application to execute only the `TrajectoryAnalysisProc`. The results section will instantly refresh with our new visual styles.

9. **Run your own experiment** <br>
It is now your turn to expand this exploration. Add a fourth input dataset to the `Study_Gravity` by choosing a planet of your choice (e.g., Jupiter, Venus, or even the Sun) and look up its gravity value:
    - To optimize the execution, deactivate the previous datasets (`Earth`, `Moon`, `Mars`) within the study. This ensures the application will only process your new dataset.
    - Customize the appearance of your new curve in the `TrajectoryAnalysisProc` results settings. You can refer to the [Matplotlib](https://matplotlib.org/stable/){:target="_blank"} conventions for the [colors](https://matplotlib.org/stable/gallery/color/named_colors.html){:target="_blank"}, [markers](https://matplotlib.org/stable/api/markers_api.html){:target="_blank"} and [linestyles](https://matplotlib.org/stable/gallery/lines_bars_and_markers/linestyles.html).

---

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="https://www.suffisciens.com/labsvision"
     target="_blank"
     rel="noopener noreferrer"
     class="md-button md-button--primary">
    Onboarding
  </a>
</div>

---