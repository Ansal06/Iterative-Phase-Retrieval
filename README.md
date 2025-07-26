# 🌀 Iterative Phase Retrieval Using the HIO Algorithm

This repository contains my implementation of the **Hybrid Input-Output (HIO)** algorithm for **phase retrieval** — recovering phase information from Fourier magnitude data, a key problem in computational imaging and coherent diffractive imaging (CDI).

Developed as a project following a 3-month course in **Fourier optics and computational imaging**, this simulation reconstructs a phase-only object using both **plane wave** and **vortex beam** illumination setups. The repository consists of all components including the report, code, data, figures, a Jupyter notebook appendix and the final presentation for the same.

---

## 🧠 Overview

In CDI, only the intensity (magnitude squared) of the Fourier transform is measured, and the phase is lost. Iterative algorithms like HIO are used to recover this lost phase.

The HIO update rule is: 

`O^(k+1)(x,y) = {
    O'(k)(x,y)                                    if (x,y) ∈ Support
    O(k)(x,y) - β(O(k)(x,y) - O'(k)(x,y))       otherwise
}`

where:
- `O′_k(x, y)` is the object after applying the Fourier constraint
- `β` is the relaxation parameter (typically between 0.5 and 1)

## Project Overview
Phase retrieval seeks to recover the lost **Fourier phase** of an object given only its intensity (magnitude) measurements.  
We implement:

* **Gerchberg–Saxton (GS)** – baseline for two-plane intensity data  
* **Hybrid Input-Output (HIO)** – single-plane magnitude plus support/positivity constraints  
* **Vortex-beam illumination** – embeds a spiral phase to break twin-image symmetry and accelerate convergence  

Both plane-wave and vortex runs reconstruct a 256 × 256 **Lena/Cameraman phase object** padded to enforce support.  
Quantitative comparisons (iteration count) and qualitative figures are used to show reconstruction quality.


## 🧪 Project Workflow

1. **Create a Phase Object**  
   - Use a standard grayscale image (e.g., Cameraman)
   - Normalize intensity → assign as phase: `O(x, y) = exp(i·ϕ(x, y))`

2. **Fourier Magnitude Generation**  
   - Simulate forward propagation using FFT
   - Retain only the magnitude: `|F(u, v)|`

3. **HIO Algorithm**  
   - Initialize random phase guess  
   - Iteratively apply Fourier and object domain constraints using the HIO rule  
   - Perform for 400 iterations

4. **Illumination Modes**  
   - **Plane wave**: Multiply object by `1`  
   - **Vortex beam**: Multiply object by `exp(i (arctan(y/x)))`

5. **Reconstruction & Comparison**  
   - Compare outputs from both illumination schemes  
   - Visualize convergence


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

@misc{kanu2025hio,
title = {Iterative Phase Retrieval Using the Hybrid Input-Output Algorithm},
author = {Ansal Kanu},
year = {2025},
howpublished = {\url{https://github.com/<Ansal06>/phase-retrieval}}
}


## Acknowledgements
Based on foundational work by **Gerchberg & Saxton (1972)**, **Fienup (1978, 1982)**, and recent advances in vortex-beam phase retrieval (**Kularia et al., JOSA A 2024**).
-Gerchberg & Saxton (1972) – A practical algorithm for the determination of phase from image and diffraction plane pictures, Optik 35(2), 237–246.
-Fienup (1978, 1982) – Reconstruction of an object from the modulus of its Fourier transform, Opt. Lett. 3, 27–29 (1978); Phase retrieval algorithms: a comparison, Appl. Opt. 21(15), 2758–2769 (1982).
-Kularia et al. (2024) – Twin-stagnation-free phase retrieval with vortex phase illumination, JOSA A 41, 1166–1174 (2024).
  
Happy reconstructing!
