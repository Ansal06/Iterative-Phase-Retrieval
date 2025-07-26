# 🌀 Iterative Phase Retrieval Using the HIO Algorithm

This repository contains my implementation of the **Hybrid Input-Output (HIO)** algorithm for **phase retrieval** — recovering phase information from Fourier magnitude data, a key problem in computational imaging and coherent diffractive imaging (CDI).

Developed as a project following a 3-month course in **Fourier optics and computational imaging**, this simulation reconstructs a phase-only object using both **plane wave** and **vortex beam** illumination setups.

---

## 🧠 Overview

In CDI, only the intensity (magnitude squared) of the Fourier transform is measured, and the phase is lost. Iterative algorithms like HIO are used to recover this lost phase.

The HIO update rule is:
O_{k+1}(x, y) = O′_k(x, y),                            if (x, y) ∈ support
O_k(x, y) - β [O_k(x, y) - O′_k(x, y)], otherwise
Where:
- `O′_k(x, y)` is the object after applying the Fourier constraint
- `β` is the relaxation parameter (typically between 0.5 and 1)


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
   - **Vortex beam**: Multiply object by `exp(i·arctan(y/x))`

5. **Reconstruction & Comparison**  
   - Compare outputs from both illumination schemes  
   - Visualize convergence

  
