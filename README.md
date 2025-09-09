# PID-Controller-Design-for-the-Helium-Synchrotron-Medical-Accelerator

This repository contains the code and documentation for developing a feedback controller for the Radio-Frequency Knock-Out (RFKO) method in slow beam extraction, implemented in simulation with Xsuite. The project was carried out as part of the CERN NIMMS Summer Student Program 2023.

The broader context is the NIMMS (Next Ion Medical Machine Study) Group’s effort to design a new medical accelerator, the Helium-Synchrotron, for radiotherapy applications. A key challenge in this larger project is to provide a uniform, stable beam spill for precise tumor treatment, which is achieved through slow extraction using the RFKO method. This work focuses on the feedback controller design that supports this overall accelerator development.

---

## Contents

* **RFKO\_feedback\_controll.ipynb**
  Jupyter Notebook with the implementation of the RFKO feedback controller, simulation workflow, and plots.

```
 * **Figures/** *(optional)*
   Directory for extracted plots/diagrams from the notebook.
```
---

## Project Overview

The project’s goal is to design and simulate a feedback-based control system that stabilizes particle beam extraction in a Helium Synchrotron for medical applications such as radiotherapy.
A stable, uniform beam spill is critical to ensure precise patient treatment.

Key aspects:

* Implementation of PI/PID controllers for amplitude modulation (AM).
* Exploration of frequency modulation (FM) methods: random, modulator signal, and chirp.
* Demonstration that chirp-based FM produces the most uniform extraction spill.
* Statistical validation using Poisson distributions and duty factor analysis.

---

## Note for Users

This notebook represents the original student implementation from 2023. It is primarily provided as an archival record of the work and may not run end-to-end without adjustments. Users should be aware that some variables and file paths may need to be corrected, dependencies installed, and environment-specific details updated. Since the project has since been continued and extended (including a C++ implementation), this notebook is best viewed as documentation of the original approach rather than production-ready code.

---

## Getting Started

1. **Clone this repository:**

   ```bash
   git clone https://github.com/<your-username>/RFKO-Feedback-Control.git
   cd RFKO-Feedback-Control
   ```

2. **Install requirements (if not already available):**

   ```bash
   pip install numpy pandas matplotlib jupyter
   ```

3. **Run the notebook:**

   ```bash
   jupyter notebook RFKO_feedback_controll.ipynb
   ```

4. Inspect the simulation results and plots.

---

## Code Notes

The notebook implements a feedback controller class (`RFKOFeedback`).
Here are important details and caveats:

* **Controller Logic**

  * Uses PI control with optional D-term.
  * Control signal is clipped between `min_limit` and `max_limit` to avoid instability.
  * Error is computed as the difference between the setpoint extraction rate and the observed rate.

* **Frequency Modulation Modes**

  * FM=1 → Random uniform signal.
  * FM=2/3 → Resonant sinusoidal kicks (third-order resonance).
  * FM=4 → Gaussian band — note: code currently uses `np.random.normal`, which may not match intended “between \[left, right]” sampling.
  * FM=6 → Chirp modulation — should ideally use phase accumulation for correctness (see future work).

* **Known Issues**

  * Some undefined variables (`particle_df`, `pi`, `nturns`) need cleanup.
  * Current file writing (`Values.txt`) is inefficient; buffering is recommended.
  * No integral anti-windup yet; the I-term can drift if output saturates.
  * Some frequency modulation methods need mathematical refinement (especially chirp and Gaussian FM).
  * Heavy per-turn Pandas operations could slow down long runs.

* **Future Work (from report and continuation)**

  * Add a D component to the PID controller or implement a feed-forward system to reduce oscillations around the set-point.
  * Rewrite the tool in C for performance improvements, as planned in the report.
  * The project was continued by Rebecca Taylor, who cited this work in her PhD thesis, further developing the controller approach.

---

## Continuation and Impact

This project was extended after its completion. Rebecca Taylor continued the work, translated the controller implementation from Python to C++ and added the D component to the PID controller, and cited this project in her PhD thesis. This ensured both performance improvements and the incorporation of the controller into subsequent accelerator physics research.

---

## Reference and Citation

The official CERN project report is available here:
[https://cds.cern.ch/record/2879791](https://cds.cern.ch/record/2879791)

If you use this code or report, please cite:
**Stamatina Detsi**, *A Feedback Controller for the RFKO Method for Slow Extraction for NIMMS Helium Synchrotron*, CERN-STUDENTS-Note-2023-232, 2023.
[CERN Document Server](https://cds.cern.ch/record/2879791)

---

## Acknowledgements

* CERN NIMMS Group (Next Ion Medical Machine Study)
* Supervisors: R. Taylor, E. Benedetto, M. Vretenar
* Summer Student Program 2023

---
