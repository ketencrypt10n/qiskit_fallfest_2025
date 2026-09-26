# Quantum Key Distribution (QKD) Simulations — QuantaNova

Simulations of the two foundational Quantum Key Distribution protocols, **BB84** and **E91**, built with [Qiskit](https://qiskit.org/). Both protocols let two parties (Alice and Bob) establish a shared secret key whose security rests on the laws of quantum mechanics rather than computational hardness — any eavesdropper (Eve) attempting to intercept the qubits introduces detectable errors.

## Repository Contents

| File | Description |
|---|---|
| `QKD_BB84_QuantaNova.ipynb` | BB84 protocol implementation: noiseless simulation, a realistic noisy simulation (IBM `FakeSherbrooke` device noise model), optional eavesdropper, and error-rate comparison plots. |
| `QuantaNova_E91.ipynb` | E91 (Ekert) protocol implementation: entangled-pair generation, Alice/Bob/Eve as classes, CHSH Bell-inequality test, intercept-resend attack simulation, and noise-level analysis. |
| `Quantum_Key_Distribution.pdf` | Reference slide deck covering the theory: motivation for QKD, the BB84 protocol steps (quantum communication, sifting, QBER estimation, error reconciliation, privacy amplification), and related background. |

## Protocols

### BB84 (`QKD_BB84_QuantaNova.ipynb`)
1. **Alice** generates random bits and random encoding bases (Z / X) and prepares qubits accordingly.
2. **(Optional) Eve** intercepts and measures the qubits in randomly chosen bases before forwarding them (intercept-resend attack).
3. **Bob** measures each incoming qubit in a randomly chosen basis.
4. **Sifting**: Alice and Bob publicly compare bases and keep only the bits where their bases matched.
5. **QBER estimation**: a sample of the sifted key is compared to estimate the quantum bit error rate.
6. The notebook repeats the protocol under a **realistic noise model** (derived from IBM's `FakeSherbrooke` backend via `qiskit_aer.noise`) and plots the error rate for the noiseless vs. noisy runs, with and without an eavesdropper.

### E91 (`QuantaNova_E91.ipynb`)
1. Generate **entangled Bell pairs** shared between Alice and Bob.
2. Alice and Bob each independently choose measurement angles from their own sets and measure their halves of the entangled pairs.
3. Basis/angle information is exchanged so matching-basis results can be used for key generation, while mismatched-basis results are used for a **CHSH Bell-inequality test**.
4. A Bell violation confirms the correlations are genuinely quantum (ruling out a classical intercept-resend attack), after which the key is extracted.
5. An **Eve** class simulates an intercept-resend attack with configurable attack probability, and a **noise model** (configurable 1-qubit/2-qubit gate error and readout error) lets the protocol be tested under realistic hardware conditions.

## Requirements

- Python 3.9+
- [Qiskit](https://pypi.org/project/qiskit/)
- [qiskit-aer](https://pypi.org/project/qiskit-aer/)
- [qiskit-ibm-runtime](https://pypi.org/project/qiskit-ibm-runtime/)
- `numpy`, `matplotlib`

Install everything with:

```bash
pip install qiskit qiskit-aer qiskit-ibm-runtime numpy matplotlib
```

(The first cell of each notebook also runs this install command directly.)

## Usage

1. Clone this repository and open either notebook in Jupyter, JupyterLab, Google Colab, or VS Code.
2. Run all cells from top to bottom.
3. In `QKD_BB84_QuantaNova.ipynb`, adjust `n_qubits`, `test_fraction`, and `with_eve` to explore different scenarios.
4. In `QuantaNova_E91.ipynb`, adjust the parameters of `run_e91_protocol()` (`n_pairs`, `eve_attack`, `eve_probability`, `noise_enabled`, gate/readout error rates) to explore different scenarios.
5. Review the printed summaries (sifted key length, estimated QBER, CHSH value, Bell-violation status) and generated plots.

## Reference

`Quantum_Key_Distribution.pdf` — background slides on QKD as a response to the threat quantum computers pose to classical cryptography, and a walkthrough of the BB84 protocol stages: quantum communication, sifting, QBER estimation, error reconciliation, and privacy amplification.

## License

Add a license of your choice (e.g., MIT) before publishing this repository publicly.
