# Cantilever subjected to end shear force

- Branch: [`cantilever-shear`](https://github.com/nuremics/nuremics-labs/tree/cantilever-shear){:target="_blank"}
- App: [`CANTILEVER_SHEAR_APP`](../../labs/apps/cms/CANTILEVER_SHEAR_APP/app.md){:target="_blank"}

---

<div align="center" style="font-weight: bold; font-size: 1.0rem;">
Take part of the discussions on the Discord channel <code>#cantilever-shear</code>
</div>

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="https://www.suffisciens.com/nuremics/discord"
     target="_blank"
     rel="noopener noreferrer"
     class="md-button md-button--primary">
    Join the Community
  </a>
</div>

---

## Introduction

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

## Materials & Methods

This benchmark simulation is thus reproduced using a custom computational model, and the obtained results are compared with the published reference solution.

_To be continued..._