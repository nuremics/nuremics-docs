# Cantilever subjected to end shear force

- Branch: [`cantilever-shear`](https://github.com/nuremics/nuremics-labs/tree/cantilever-shear){:target="_blank"}
- App: [`CANTILEVER_SHEAR_APP`](../../labs/apps/cms/CANTILEVER_SHEAR_APP/app.md){:target="_blank"}

<p style="text-align: right;">
  <a href="../../../development-chart/" target="_blank"><em>Check Development Chart</em> </a>
</p>
---

<div class="timeline-container">
  <div class="timeline-line"></div>
  <div class="timeline-arrow"></div>

  <div class="timeline-items">
    <div class="timeline-item">
      <div class="phase">Use Case</div>
      <div class="marker"></div>
      <div class="role">Requirements Scientist</div>
      <div class="contributors">
        <div class="contributor">
          <a href="https://github.com/julien-siguenza" target="_blank">
            <img src="https://github.com/julien-siguenza.png" alt="Julien SIGUENZA" />
            <p class="name">Julien SIGUENZA</p>
          </a>
        </div>
      </div>
    </div>

    <div class="timeline-item">
      <div class="phase">Architecture</div>
      <div class="marker"></div>
      <div class="role">Workflow Architect</div>
      <div class="contributors">
        <div class="contributor">
          <a href="https://github.com/julien-siguenza" target="_blank">
            <img src="https://github.com/julien-siguenza.png" alt="Julien SIGUENZA" />
            <p class="name">Julien SIGUENZA</p>
          </a>
        </div>
      </div>
    </div>

    <div class="timeline-item">
      <div class="phase">Implementation</div>
      <div class="marker"></div>
      <div class="role">Scientific Developer</div>
      <div class="contributors">
        <div class="contributor">
          <a href="https://github.com/julien-siguenza" target="_blank">
            <img src="https://github.com/julien-siguenza.png" alt="Julien SIGUENZA" />
            <p class="name">Julien SIGUENZA</p>
          </a>
        </div>
      </div>
    </div>

    <div class="timeline-item">
      <div class="phase">Integration</div>
      <div class="marker"></div>
      <div class="role">Software Developer</div>
      <div class="contributors">
        <div class="contributor">
          <a href="https://github.com/emmanuelHa" target="_blank">
            <img src="https://github.com/emmanuelHa.png" alt="Emmanuel HAREL" />
            <p class="name">Emmanuel HAREL</p>
          </a>
        </div>
      </div>
    </div>

    <div class="timeline-item">
      <div class="phase">Production</div>
      <div class="marker"></div>
      <div class="role">End-User Scientist</div>
      <div class="contributors">
        <div class="contributor">
          <a href="https://github.com/julien-siguenza" target="_blank">
            <img src="https://github.com/julien-siguenza.png" alt="Julien SIGUENZA" />
            <p class="name">Julien SIGUENZA</p>
          </a>
        </div>
      </div>
    </div>

  </div>
</div>

<style>
.timeline-container {
  position: relative;
  margin: 60px 0;
  width: 100%;
  height: 200px;
}

/* Ligne principale centrée sur les marqueurs */
.timeline-line {
  position: relative;
  top: 0;
  bottom: 0;
  left: 0;
  width: calc(100% - 12px); /* espace pour la flèche */
  height: 2px;
  background: #444;
  margin: auto 0; /* centre verticalement */
  z-index: 0;
}

/* Flèche finale */
.timeline-arrow {
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  transform: translate(0px, -5px);
  width: 0;
  height: 0;
  border-left: 12px solid #444;
  border-top: 6px solid transparent;
  border-bottom: 6px solid transparent;
  z-index: 1;
}

.timeline-items {
  display: flex;
  justify-content: space-between;
  width: 100%;
  position: relative;
  z-index: 2;
}

.timeline-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Marqueur centré sur la ligne */
.marker {
  width: 12px;
  height: 12px;
  background-color: #444;
  border-radius: 50%;
  margin: 0;
  position: absolute;
  transform: translate(0px, -7px);
  z-index: 3;
}

/* Phase au-dessus */
.phase {
  font-weight: bold;
  margin-bottom: 5px;
  text-align: center;
  transform: translate(0px, -40px);
}

/* Role en dessous */
.role {
  font-style: normal;
  margin-top: 0px;
  margin-bottom: 0px;
  text-align: center;
  color: #666;
  transform: translate(0px, -20px);
}

/* Contributeurs sous le rôle */
.contributors {
  display: flex;
  gap: 8px;
  justify-content: center;
  flex-wrap: wrap;
  margin-top: 5px;
}

.contributor img {
  display: block;   /* prend toute la largeur du conteneur */
  margin: 0 auto;   /* centre horizontalement */
  margin-bottom: 10px;
  width: 110px;
  height: 110px;
  border-radius: 50%;
  box-shadow: 0 2px 6px rgba(0,0,0,0.2);
  transition: transform 0.2s ease;
}

.contributor .name {
  font-size: 1.0em;
  text-align: center;
  margin: 2px 0 0 0;
}

.contributor img:hover {
  transform: scale(1.1);
}

</style>

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

The present use case addresses a classical benchmark problem in **Computational Structural Mechanics (CSM)**, namely the simulation of a beam structure fixed at one end and loaded by a shear force at the other end (see Fig. 1). This type of problem is widely used in the literature as a reference for evaluating the accuracy and robustness of numerical methods in **CSM**.

The benchmark considered here was originally reported by [Sze *et al.* 2004](https://doi.org/10.1016/j.finel.2003.11.001){:target="_blank"}, who compiled a comprehensive set of non-linear test cases for shell **Finite-Element (FE)** analysis. In their work, the reference solution (summarized in Tab. 1) for this specific problem was obtained using the commercial **FE** solver [*Abaqus*](https://www.3ds.com/products/simulia/abaqus){:target="_blank"}, relying on the S4R four-node shell elements with reduced integration and hourglass control.

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

This test case is particularly interesting because it exhibits non-linear elastic behavior, which provides a meaningful challenge for numerical methods. Additionally, the simple geometric shape of the structure allows for different modeling approaches: fully resolved 3D solid elements, 2D shell elements, or simplified 1D beam elements. Each approach introduces different levels of approximation and computational cost, offering a clear perspective on the trade-offs inherent in structural modeling.

The main objective of this use case is therefore to simulate the benchmark problem using three different modeling strategies [3D solid elements | 2D shell elements | 1D beam elements] within the **nuRemics** framework, and to compare their respective capabilities in capturing the non-linear response of the structure.

<figure class="wide-caption">
  <img src="../../../images/ASME_VV40.png" width="80%"/>
  <figcaption>Figure 2:
    The ASME V&V40 Standard provides structured guidance for assessing credibility and applicability of Computational Modeling & Simulation in the context of Medical Devices. For more information, see the <a href="https://www.asme.org/codes-standards/find-codes-standards/assessing-credibility-of-computational-modeling-through-verification-and-validation-application-to-medical-devices" target="_blank">official ASME V&V40 page</a>.
  </figcaption>
</figure>

Importantly, this study is performed in application of the **ASME V&V40 standard (V&V40)** (see Fig. 2), which provides guidance for the development of **Computational Modeling & Simulation (CM&S)** technologies, ensuring their credibility and reliability through rigorous **Verification & Validation (V&V)** activities. The standard establishes a structured framework to guarantee that CM&S methods meet the highest standards of safety and effectiveness, particularly in the context of **Medical Device (MD)** development. By adhering to the **V&V40**, this use case not only serves as a benchmark for numerical accuracy but also demonstrates how **nuRemics** can be applied within a scientifically rigorous and regulatory-aware workflow.

It is nonetheless important to note that this methodology could be broadened to contexts beyond **MD**, making the **V&V40** framework relevant for a wide range of industrial applications where the **V&V** of computational models is critical for design, safety, and performance assessment.

In the present use case, the benchmark problem is employed to establish the credibility factor associated with **Numerical Code Verification (NCV)**. The objective of **NCV** is to demonstrate the correct implementation and functioning of the numerical algorithms within the **CM&S** framework. This involves a careful investigation of key numerical aspects, including spatial and temporal convergence rates, independence from coordinate transformations, and symmetry tests under various system conditions.

**NCV** is typically conducted by comparing numerical solutions to exact benchmark solutions, which may be analytical or semi-analytical, or generated using techniques such as the methods of manufactured solutions. In this use case, the cantilever beam benchmark provides such a reference solution, enabling a rigorous assessment of the numerical fidelity of the **nuRemics CM&S software system** across multiple modeling strategies (3D solids, 2D shells, and 1D beams).