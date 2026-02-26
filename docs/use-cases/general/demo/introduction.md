# Introduction

The present use case addresses a classical problem in **Solid Mechanics**, namely the simulation of projectile kinematics launched with an initial velocity and subjected to gravitational acceleration (see Fig. 1).

<figure class="wide-caption">
  <img src="../../../../images/projectile_motion.png" width="25%"/>
  <figcaption>Figure 1: Schematic representation of a polygonal projectile launched from its center of mass with an initial velocity (green vector) at an angle &alpha; relative to the horizontal. The projectile is subjected to a constant downward gravitational acceleration (red vector).
  </figcaption>
</figure>

The projectile is modeled as a 2D polygonal shape defined by a set of vertices (see Fig. 2). This geometric representation explicitly defines the spatial boundaries of the object, moving beyond simplified point-mass model. The geometry is established by a sequence of $(x, y)$ coordinates where each vertex serves as a node for the boundary edges. By utilizing a discrete set of vertices, it provides a versatile polygonal model for defining arbitrary rigid body shapes.

<figure class="wide-caption">
  <img src="../../../../images/2D_polygon_shape.png" width="45%"/>
  <figcaption>Figure 2: Spatial discretization of the 2D polygon shape. The plot illustrates the vertices (blue markers) and connecting edges that define the geometry of the projectile within a local (x, y) coordinate system (in units of meters).
  </figcaption>
</figure>

To simulate the motion of the projectile, a physics engine is employed to numerically simulate its trajectory. To verify the accuracy of this numerical model, the simulation results are compared against the analytical solution derived from classical point-mass mechanics.

The theoretical trajectory is governed by the following kinematic equations, assuming a constant gravitational acceleration $g$ and neglecting air resistance:

$$
\left\{
\begin{aligned}
x(t) &= v_0 \cos(\alpha)\, t \\
y(t) &= y_0 + v_0 \sin(\alpha)\, t + \frac{1}{2} g t^2
\end{aligned}
\right.
$$

Where:

- $v_0$ is the initial velocity magnitude.
- $\alpha$ is the launch angle relative to the horizontal.
- $y_0$ is the initial height.
- $t$ is the elapsed time.

The comparison between the numerical simulation and this theoretical solution is illustrated in Figure 3, showing the spatial evolution of the projectile from the launch point to its impact zone.

<figure class="wide-caption">
  <img src="../../../../images/projectile_motion_model_vs_theory.png" width="60%"/>
  <figcaption>Figure 3: (x, y) trajectory of the projectile. Comparison between the numerical simulation (dashed red line) and the theoretical solution (solid black line).
  </figcaption>
</figure>

---

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="../materials-and-methods/"
     class="md-button md-button--primary">
    Materials & Methods
  </a>
</div>

---