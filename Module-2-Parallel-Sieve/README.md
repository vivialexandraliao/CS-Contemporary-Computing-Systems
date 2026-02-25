# CS-Contemporary-Computing-Systems
A respository for DS/CS work
# Parallel Sieve of Eratosthenes ($N=10^9$)
## Contemporary Computing System Modeling - Module 2

### 🔬 Project Overview
A high-performance implementation of the Sieve of Eratosthenes targeting $N=10^9$. This project explores the transition from serial algorithmic logic to parallel execution using shared-memory (OpenMP/Multiprocessing) and GPGPU (CUDA) models.

### 🛠 Tech Stack
* **Language:** Python 3.11+
* **Parallelism:** Numba (CUDA), Multiprocessing (OpenMP-style)
* **Analysis:** NumPy, Matplotlib, LaTeX

### 📐 Parallel Design (PCAM Model)
| Phase | Strategy |
| :--- | :--- |
| **Partitioning** | Segmented Domain Decomposition |
| **Communication** | Broadcast of small primes ($\sqrt{N}$) |
| **Agglomeration** | One small prime per GPU thread/CPU worker |
| **Mapping** | Static (CPU) and Hardware-Scheduled (GPU) |

### 📈 Performance Highlights
* **Peak Speedup:** 45x (CUDA vs. Serial Baseline)
* **Memory Optimization:** Bit-packed array reducing footprint by 87.5%.

### 🚀 Youtube Video link
https://youtu.be/rZe7Wdfvapg
