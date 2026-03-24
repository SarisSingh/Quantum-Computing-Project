# Grover's Search Algorithm — Qiskit Implementation

**Course Project | Quantum Computing**

---

## Overview

This project implements **Grover's Search Algorithm** from scratch using Qiskit, without using any built-in `qiskit.algorithms.Grover` helper. The algorithm searches a **10-qubit (1024-state)** space and finds two marked states:

- `0110011010`
- `1101010001`

---

## Files

| File | Description |
|------|-------------|
| `Grover_Algorithm_Qiskit.ipynb` | Complete Google Colab notebook with implementation |
| `README.md` | This file |

---

## How to Run

### Option A: Google Colab (Recommended)

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Upload `Grover_Algorithm_Qiskit.ipynb`
3. Run **Cell 1** (Installation) — uninstalls conflicting versions and installs the correct compatible pair
4. **Restart the runtime** (`Runtime → Restart runtime`) — this step is mandatory
5. Run all remaining cells in order (`Runtime → Run all`)

> ⚠️ **Do not skip the runtime restart.** Colab caches old package versions in memory; without a restart, the `ImportError` for `ProviderV1` will persist even after reinstallation.

### Option B: Local Jupyter

```bash
pip install qiskit==1.1.2 qiskit-aer==0.15.1 pylatexenc matplotlib
jupyter notebook Grover_Algorithm_Qiskit.ipynb
```

Then run all cells from top to bottom.

---

## Dependency & Version Notes

### Compatible Version Pair

| Package | Required Version | Notes |
|---------|-----------------|-------|
| `qiskit` | `1.1.2` | Qiskit 1.0 removed the legacy `ProviderV1` API |
| `qiskit-aer` | `0.15.1` | First release ported to the Qiskit 1.x API |
| `pylatexenc` | latest | Required for circuit diagram rendering |
| `matplotlib` | latest | Required for histogram plots |

### Known Issue — `ImportError: cannot import name 'ProviderV1'`

If you see this error:
```
ImportError: cannot import name 'ProviderV1' from 'qiskit.providers'
```

This means `qiskit-aer 0.14.x` (which depends on the removed `ProviderV1` API) is installed instead of `0.15.x`. **Cell 1 in the notebook fixes this automatically** by uninstalling old versions before reinstalling the correct ones. Make sure to restart the runtime afterwards.

**Version compatibility reference:**

| `qiskit` | Compatible `qiskit-aer` |
|----------|------------------------|
| `0.44 – 0.45` | `0.12 – 0.13.3` |
| `1.0 – 1.1` | `0.15.1` |
| `1.2+` | `0.15.1+` |

---

## Expected Output

When the notebook runs successfully:

1. ✅ All packages install without errors
2. ✅ The Grover circuit is built and verified
3. ✅ Histograms appear for 1, 3, 5, and 10 iterations
4. ✅ The two marked states (`0110011010`, `1101010001`) appear with the **highest probability** in each histogram
5. ✅ A probability-vs-iterations chart shows how amplification increases toward the optimal k ≈ 17

---

## Algorithm Structure

```
|0⟩^10 ──── H^10 ──── [Oracle → Diffusion]^k ──── Measure
```

| Component | Implementation |
|-----------|---------------|
| Superposition | Hadamard on all 10 qubits |
| Oracle | X gates + Multi-controlled Z (MCX with H trick) |
| Diffusion | H → X → MCZ → X → H (inversion about mean) |
| Simulation | AerSimulator, 4096 shots, transpile() |

---

## Optimal Iterations

For N = 1024 states and M = 2 marked states:

$$k_{\text{opt}} = \left\lfloor \frac{\pi}{4} \sqrt{\frac{N}{M}} \right\rfloor = 17$$

The notebook runs k = 1, 3, 5, 10 to show how probability evolves.

---

## References

- Grover, L. K. (1996). *A fast quantum mechanical algorithm for database search.*
- IBM Quantum Learning: https://quantum.cloud.ibm.com/learning/en/modules/computer-science/grovers
- Qiskit Docs: https://docs.quantum.ibm.com
