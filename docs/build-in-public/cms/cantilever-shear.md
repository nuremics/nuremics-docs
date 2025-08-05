# Cantilever subjected to end shear force

## Use Case

This is a classical benchmark in **Computational Structural Mechanics (CSM)**, focusing on the simulation of a beam structure fixed at one end and loaded by a shear force at the other end.

This test case is extracted from the scientific article by [Sze *et al.* 2004](https://doi.org/10.1016/j.finel.2003.11.001){:target="_blank"}, which compiles and tabulates detailed reference solutions for a series of well-known non-linear benchmark problems in shell finite element analysis. The reference solution of the present test case was obtained using the commercial solver [*Abaqus*](https://www.3ds.com/products/simulia/abaqus){:target="_blank"} and its S4R four-node shell elements, with reduced integration and hourglass control.

<figure class="wide-caption">
  <img src="../../../images/cantilever_shear_figure.png" width="70%"/>
  <figcaption>Figure 1 (extracted from 
    <a href="https://doi.org/10.1016/j.finel.2003.11.001" target="_blank">Sze et al. 2004</a>):
    (a) Cantilever subjected to end shear force. (b) Load–deflection curves for cantilever subjected to end shear force. (c) The deformed 16 × 1 mesh under the maximum force.
  </figcaption>
</figure>

<figure class="wide-caption">
  <img src="../../../images/cantilever_shear_table.png" width="100%"/>
  <figcaption>Table 1 (extracted from 
    <a href="https://doi.org/10.1016/j.finel.2003.11.001" target="_blank">Sze et al. 2004</a>):
    Horizontal and vertical tip deflections for the cantilever loaded with end shear force.
  </figcaption>
</figure>

The purpose of the present use case is thus to implement a custom **NUREMICS App** specifically tailored to reproduce this benchmark simulation and to compare the obtained results with the published reference solution.

<!-- ## NUREMICS App

- **Branch name:** [`cantilever-shear`](https://github.com/nuremics/nuremics-labs/tree/cantilever-shear){:target="_blank"}
- **App name:** `CANTILEVER_SHEAR_APP` -->