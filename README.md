# 🌀 Iterative Phase Retrieval Using the HIO Algorithm

This repository contains my implementation of the **Hybrid Input-Output (HIO)** algorithm for **phase retrieval** — recovering phase information from Fourier magnitude data, a key problem in computational imaging and coherent diffractive imaging (CDI).

Developed as a project following a 3-month course in **Fourier optics and computational imaging**, this simulation reconstructs a phase-only object using both **plane wave** and **vortex beam** illumination setups. The repository consists of all components including the report, code, data, figures, a Jupyter notebook appendix and the final presentation for the same.

---

## 🧠 Overview

In CDI, only the intensity (magnitude squared) of the Fourier transform is measured, and the phase is lost. Iterative algorithms like HIO are used to recover this lost phase.

The HIO update rule used is:

$$O^{(k+1)}(x,y) = \begin{cases} 
O'^{(k)}(x,y) & \text{if } (x,y) \in \text{Support} \\
O^{(k)}(x,y) - \beta\left(O^{(k)}(x,y) - O'^{(k)}(x,y)\right) & \text{otherwise}
\end{cases}$$

**where:**
- $O^{(k+1)}(x,y)$ is the updated object estimate at iteration $k+1$
- $O'^{(k)}(x,y)$ is the result after applying Fourier constraints at iteration $k$
- $O^{(k)}(x,y)$ is the object estimate from the previous iteration
- $\beta$ is the relaxation parameter (typically 0.8)
- **Support** refers to the known region where the object exists (non-zero values expected)

**Physical Interpretation:**
- **Inside support region**: Accept the Fourier-constrained solution directly
- **Outside support region**: Apply feedback with relaxation to gradually suppress artifacts
- The $\beta$ parameter controls the strength of the correction, preventing oscillations while ensuring convergence

  
## Project Overview
Phase retrieval seeks to recover the lost **Fourier phase** of an object given only its intensity (magnitude) measurements.  
We implement:

* **Gerchberg–Saxton (GS)** – baseline for two-plane intensity data  
* **Hybrid Input-Output (HIO)** – single-plane magnitude plus support/positivity constraints  
* **Vortex-beam illumination** – embeds a spiral phase to break twin-image symmetry and accelerate convergence  

Both plane-wave and vortex runs reconstruct a 256 × 256 **Lena/Cameraman phase object** padded to enforce support.  
Quantitative comparisons (iteration count) and qualitative figures are used to show reconstruction quality.


## 🧪 Project Workflow

### Phase 1: Phase Object Creation
**Objective:** Transform a standard grayscale test image into a complex-valued phase object

- Use a standard grayscale image (e.g., Cameraman, Lena)
- Load and normalize intensity to [0, 1] range  
- Assign normalized intensity as phase: $$O(x,y) = e^{i\phi(x,y)}$$
- Apply zero-padding for support constraints (critical for HIO convergence)

### Phase 2: Fourier Magnitude Generation  
**Objective:** Simulate realistic measurement conditions where only intensity information is available

- Simulate forward propagation using Fast Fourier Transform (FFT)
- Compute: $$F(u,v) = \mathcal{F}\{O(x,y)\}$$
- Retain only the Fourier magnitude: $$|F(u,v)|$$
- Discard phase information to simulate detector limitations

### Phase 3: HIO Algorithm Implementation
**Objective:** Iteratively recover lost phase information through constraint application

- **Initialize:** Random phase guess combined with known Fourier magnitude
- **Iterative Process:** Apply the HIO update rule for 400 iterations:

$$O^{(k+1)}(x,y) = \begin{cases} 
O'^{(k)}(x,y) & \text{if } (x,y) \in \text{Support} \\
O^{(k)}(x,y) - \beta\left(O^{(k)}(x,y) - O'^{(k)}(x,y)\right) & \text{otherwise}
\end{cases}$$

- **Parameters:** β = 0.8 (relaxation parameter), 400 iterations for convergence
- **Constraints:** Non-negativity and support region enforcement

### Phase 4: Dual Illumination Modes
**Objective:** Compare reconstruction performance under different illumination conditions

#### Method A: Plane Wave Illumination (Baseline)
- **Illumination function:** Multiply object by unity: $$O_{\text{plane}}(x,y) = O(x,y) \times 1$$
- **Characteristics:** Uniform illumination, standard imaging condition
- **Expected artifacts:** Twin-image stagnation possible

#### Method B: Vortex Beam Illumination (Enhanced)  
- **Illumination function:** Apply spiral phase: $$O_{\text{vortex}}(x,y) = O(x,y) \times e^{i\arctan(y/x)}$$
- **Phase correction:** After reconstruction, remove vortex phase: $$O_{\text{recovered}}(x,y) = O_{\text{reconstructed}}(x,y) \times e^{-i\arctan(y/x)}$$
- **Advantages:** Breaks twin-image symmetry, faster convergence

### Phase 5: Reconstruction Analysis & Comparison
**Objective:** Quantitative and qualitative assessment of both illumination methods

#### Convergence Tracking
- Monitor reconstruction quality over 400 iterations
- Save intermediate results every 100 iterations for progress visualization
- Track structural similarity index (SSIM) and mean squared error (MSE)

#### Comparative Metrics
| **Illumination Method** | **Convergence** | **Artifacts** | **Reconstruction Quality** |
|------------------------|----------------|---------------|---------------------------|
| **Plane Wave**         | 400 iterations | Twin-image present | Good (some stagnation) |
| **Vortex Beam**        | 400 iterations | Artifacts suppressed | Excellent (stable) |

#### Visualization Pipeline
- **Side-by-side comparison:** Original vs. reconstructed images
- **Difference mapping:** Error analysis between methods  
- **Progress tracking:** Convergence evolution over iterations
- **Phase pattern display:** Vortex illumination structure visualization

**Key Finding:** Vortex illumination demonstrates superior reconstruction quality with complete twin-image artifact suppression, validating the effectiveness of structured illumination in iterative phase retrieval algorithms.


## Key Features
* Pure-Python, NumPy & FFT-based – no external optics libraries required  
* Mask-based support & non-negativity constraints  
* β-relaxation parameter configurable (default **0.8**)  
* Automatic saving of intermediate reconstructions every N iterations  
* Jupyter notebooks for step-by-step explanation and interactive plots  
* Unit tests for numerical sanity (energy conservation, convergence)  
* MIT-licensed for academic & commercial reuse

---

## License
This project is licensed under the **MIT License**.
See the [LICENSE](LICENSE) file for details.


## 👤 Author

This project code is solely developed by:

**Ansal Kanu**  
B.Sc. (Hons.) Physics, Ramjas College, University of Delhi  
Email: kanuansal06@gmail.com  
GitHub: [github.com/<your-username>](https://github.com/<your-username>)

If you use or adapt any part of this work, please cite the repository or credit the author.


## Citation
If you use this code in academic work, please cite:

```bibtex
@misc{kanu2025hio,
  title  = {Iterative Phase Retrieval Using the Hybrid Input-Output Algorithm},
  author = {Ansal Kanu},
  year   = {2025},
  howpublished = {\url{https://github.com/Ansal06/Iterative-Phase-Retrieval}}
}
```
---

## Acknowledgements
Based on foundational work by **Gerchberg & Saxton (1972)**, **Fienup (1978, 1982)**, and recent advances in vortex-beam phase retrieval (**Kularia et al., JOSA A 2024**).
```
-Gerchberg & Saxton (1972) – A practical algorithm for the determination of phase from image and diffraction plane pictures, Optik 35(2), 237–246.
-Fienup (1978, 1982) – Reconstruction of an object from the modulus of its Fourier transform, Opt. Lett. 3, 27–29 (1978); Phase retrieval algorithms: a comparison, Appl. Opt. 21(15), 2758–2769 (1982).
-Kularia et al. (2024) – Twin-stagnation-free phase retrieval with vortex phase illumination, JOSA A 41, 1166–1174 (2024).
```

Happy reconstructing!
