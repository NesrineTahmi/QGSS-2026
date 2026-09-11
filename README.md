# QGSS 2026 - My Lab Solutions

My solutions to the labs from the [Qiskit Global Summer School 2026](https://github.com/qiskit-community/qgss-2026), IBM Quantum's annual summer school on quantum computing with Qiskit. Labs are Jupyter notebooks combining short lectures with graded coding exercises, run partly on simulators and partly on real IBM quantum hardware.

## Repository structure

```
QGSS-2026/
├── lab0/   # Setup: Qiskit patterns, statevectors, visualization 
├── lab1/   # Building quantum circuits for real hardware: GHZ states, depth, connectivity
├── lab2/   # Noise, backends, and benchmarking
├── lab3/   # Structure-aware error mitigation (Samplomatic, NoiseLearnerV3, PNA, PEC, SLC)
└── README.md
```

## Lab summaries

### Lab 0 
Introduces the four-step Qiskit pattern (**Map → Transpile → Execute → Post-process**) through a first circuit built from basic gates, run via `Sampler`/`Estimator`, and optionally on real hardware. Also covers statevectors and the QSphere visualization for multi-qubit states, plus an optional chapter building a circuit in C and reading the resulting state back in Python.

### Lab 1 
Starts from Bell states and works up to large GHZ states while tracking **circuit depth** and **hardware connectivity** as first-class constraints, not afterthoughts:

- **Part 1 : Bell states & entanglement.** Building `|00⟩+|11⟩` and other Bell states with H/CX.
- **Part 2 : GHZ states at scale.** Comparing a linear-depth GHZ construction to a depth-reducing "start from the middle" / recursive fan-out approach, culminating in a 16-qubit GHZ state at depth 5.
- **Part 3 : Respecting hardware topology.** Introduces the **bridge gate identity** to turn a non-local CX into nearest-neighbor gates, then applies it to build increasingly large GHZ states directly on `FakeTorino`'s heavy-hex connectivity, from a 5-qubit line, to a 7-qubit T-shaped subgraph, up to a 64-qubit GHZ state built over a BFS spanning tree of the whole device.

### Lab 2 
Moves from ideal circuits to realistic, noisy hardware behavior:

- **Chapter 1 : Noise-aware simulation.** What a `BackendV2` actually encodes (coupling map, basis gates, error rates), building a custom noisy simulator, and comparing ideal vs. noisy GHZ state results through the full Qiskit pattern.
- **Chapter 2 : Noise models.** Depolarizing error, Pauli error (with a bit-flip-sensitive experiment), and thermal relaxation (T1 decay, T2 dephasing), building intuition for what each toy noise model does to a circuit's output.
- **Chapter 3 : Heron vs. Nighthawk.** Comparing layouts and benchmarking IBM's current and previous QPU architectures against each other.
- **Chapter 4 : Dynamic circuits.** Builds a dynamic GHZ state step-by-step using mid-circuit measurement and feed-forward (measure a bridge qubit, conditionally correct, reset, and continue), plus an optional bonus section running the same idea on real hardware with a custom initial layout.

### Lab 3
Moves from whole-circuit error mitigation switches (`EstimatorOptions`: dynamical decoupling, Pauli twirling, TREX, ZNE, PEC) to **per-box, structure-aware** techniques:

- **Chapter 2 : Samplomatic & NoiseLearnerV3.** Circuits are split into annotated *boxes* (`Twirl`, `InjectNoise`, `ChangeBasis`), turned into a `template` + `samplex` pair via `build()`, characterized layer-by-layer with `NoiseLearnerV3`, and executed with the box-aware `Executor` primitive. Demonstrated on a toy 2-qubit circuit, then a mirrored 1D transverse-field Ising chain.
- **Chapter 3 : PNA, PEC, and SLC.** Three ways to spend a learned noise model:
  - **PNA (Propagated Noise Absorption):** rewrite the *observable* to absorb noise instead of correcting the circuit.
  - **PEC (Probabilistic Error Cancellation):** insert anti-noise directly into the circuit.
  - **SLC (Shaded Lightcones):** prune PEC's noise generators to only those inside an observable's propagated lightcone, cutting sampling overhead for local observables.

## Requirements

- Python 3.10+
- `qiskit`, `qiskit-ibm-runtime`
- An [IBM Quantum](https://quantum.ibm.com/) account with API access (for hardware-execution cells)
- Lab-specific packages where noted in each notebook (e.g. `samplomatic`, `qiskit-addon-pna`, `qiskit-addon-slc` for Lab 3)

Install with:

```bash
pip install qiskit qiskit-ibm-runtime
```
