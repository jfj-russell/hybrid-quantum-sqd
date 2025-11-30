# hybrid-quantum-sqd

### Project Overview

This repository contains the implementation of a hybrid quantum–classical workflow designed for accurate molecular ground-state electronic structure simulation.

Our project focuses on demonstrating the sample-based quantum diagonalisation (SQD) algorithm, which effectively addresses the limitations of today's noisy quantum hardware by offloading complexity to classical post-processing. The notebook in this repository is largely based on the nitrogen implementation in the [qiskit-addon-sqd](https://github.com/Qiskit/qiskit-addon-sqd) repository.

We applied this method to the lithium hydride molecule to showcase how to achieve energy estimates approaching chemical accuracy for quantum chemistry problems.

---

### Why This Approach?

Molecular simulation is a core challenge in drug discovery, but exact quantum chemistry methods scale exponentially, limiting classical simulation capacity to about 40-50 electrons in a hugely wasteful trial-and-error approach.

Current quantum approaches (like VQE or QPE) are often limited by noise, optimisation difficulties, or the lack of fully fault-tolerant hardware.

SQD provides a practical bridge. It uses shallow quantum circuits to prepare a trial state and collect samples, then relies on classical computation to project the Hamiltonian onto a small, relevant basis, enabling accurate energy extraction. This approach is potentially scalable to systems larger than what the best supercomputers can handle today.

---

### What the Code Does

This repository executes a two-part workflow:

#### 1. Quantum Step (collect samples)

* **Initialisation:** We use the Coupled Cluster Singles and Doubles (CCSD) results to prepare an initial state using a LUCJ ansatz.
* **Execution:** This parameterised state is executed on IBM Quantum hardware.
* **Output:** The code collects measurement samples (electron configurations) needed for the next step.

#### 2. Classical Step (SQD)

* **Configuration recovery:** Impose selection rules and reweight amplitudes
* **Reconstruction:** The measurement samples are used to reconstruct an effective low-dimensional Hamiltonian matrix.
* **Diagonalisation:** This effective Hamiltonian is classically diagonalised and is fed back into the algorithm to iteratively refine the ground-state energy value and extract electron configuration information.
![Alt text](./state_refinement.png)

---

### How to Run the Code (Using WSL/Ubuntu)

To run this simulation, you will use your **WSL Ubuntu environment** to manage the Python virtual environment and execute the scripts.

#### Prerequisites

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/jfj-russell/hybrid-quantum-sqd.git
    cd hybrid-quantum-sqd
    ```

2.  **Create and Activate Virtual Environment:**
    If you haven't already, create and activate a virtual environment within your Ubuntu terminal.

    ```bash
    python3 -m venv .venv 
    source .venv/bin/activate 
    ```

3.  **Install Dependencies:**
    (Ensure you have a `requirements.txt` file listing `qiskit`, `numpy`, etc.)
    ```bash
    pip install -r requirements.txt
    ```
4.  **Set up IBM Quantum Access:**
    If running on actual hardware, configure your IBM Quantum API Token.

#### Execution Workflow

To activate your environment and start working within **WSL Ubuntu**:

**Launch WSL and Activate Environment (from PowerShell/CMD):**
```powershell
wsl
cd ~/your_folder
source .venv/bin/activate
