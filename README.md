# Ketscope

**Step-through visual inspector for quantum circuits.** Scrub through gates, diff state vectors, and inspect entanglement, with OpenQASM 3 import/export.

> **Status:** Early planning. Nothing is usable yet. The roadmap below describes what is being built.

---

## Why Ketscope?

Existing circuit tools are built for **exploring**: you drop gates on a canvas and see the final state. Ketscope is built for **understanding a circuit you already have**, and eventually for finding out why it misbehaves.

| | Typical circuit tools | Ketscope |
|---|---|---|
| Main question answered | "What does this circuit output?" | "What happens at every step, and where does it go wrong?" |
| State inspection | Final state, or a few inline displays | Full state at any layer, with changes highlighted |
| Parameterized gates | Often unsupported | Live sliders for rotation angles |
| Entanglement | Hard to see from per-qubit views | Pairwise qubit correlation view |
| Workflow | Self-contained | OpenQASM 3 in and out, so it fits alongside Qiskit, Cirq and others |
| Account required | Sometimes | Never; runs entirely in the browser |

---

## Planned features

### Inspector (MVP)
- **Timeline scrubber**: move through the circuit layer by layer and watch the state evolve
- **State diff**: highlight exactly which amplitudes changed between two layers
- **Amplitude and probability views**: magnitude, phase and measurement probability for every basis state
- **Bloch spheres**: per-qubit reduced states, including mixed states for entangled qubits
- **Parameter sliders**: sweep rotation angles and see every view update live
- **OpenQASM 3 import/export**: load a `.qasm` file, edit it visually, export clean code

### Debugger (later milestone)
- **Breakpoints** at specific gates or layers
- **State assertions**, such as "qubits 0 and 1 are entangled here" or "this register is in uniform superposition", flagged when they fail
- **Expected vs. actual**: provide a target state and jump to the first layer where the simulation diverges

---

## Roadmap

| Version | Milestone | Scope |
|---|---|---|
| **v0.1** | Simulation core | State-vector simulator, standard gate set (H, X, Y, Z, S, T, RX/RY/RZ, CX, CZ, SWAP, Toffoli), unit tests against known results |
| **v0.2** | Circuit editor | Drag-and-drop gate placement, qubit wires, layer layout, undo/redo, shareable URLs |
| **v0.3** | Inspector | Timeline scrubber, amplitude/probability views, state diff between layers |
| **v0.4** | OpenQASM 3 | Parser and exporter for the supported gate subset, with clear errors for unsupported constructs |
| **v0.5** | Deeper views | Bloch spheres, parameter sliders, pairwise entanglement view |
| **v0.6** | Measurement | Mid-circuit measurement, classical bits, classically controlled gates |
| **v1.0** | Debugger | Breakpoints, state assertions, expected vs. actual divergence |

Each milestone ships as a usable release before the next one starts.

---

## Planned tech stack

| Layer | Choice | Reason |
|---|---|---|
| Language | **TypeScript** | One language across the simulator and UI; strong typing for the math core |
| UI | **React** + **Vite** | Mature ecosystem, fast development server |
| Rendering | **SVG** for circuits, **Three.js** for Bloch spheres | SVG stays crisp and accessible; WebGL only where 3D is needed |
| Simulation | Pure TypeScript state-vector engine | Simple to test and debug; a **Rust → WebAssembly** core is a possible later optimization |
| Testing | **Vitest** | Fast, Vite-native |
| Hosting | **GitHub Pages** | Free, static, no backend needed |

### Qubit limit

Ketscope simulates on a classical machine, so memory grows as 2ⁿ. The initial target is **up to 16 qubits** with every layer cached, with recomputation from the nearest cached layer as a later strategy for larger circuits.

---

## Non-goals

- **Running on real quantum hardware.** Inspecting intermediate states requires simulation; measuring real hardware destroys them.
- **Large-scale simulation.** Tools like Qiskit Aer and qsim already do this well.
- **Circuit optimization or transpilation.** Use Qiskit, Cirq or t|ket⟩.
- **Courses, exercises or challenges.** Ketscope is a tool, not a learning platform.

---

## Contributing

Ketscope is currently developed by a single maintainer, and **outside pull requests are not being accepted yet**. Bug reports and feature ideas are welcome as issues.

---

## License

Licensed under the [Apache License 2.0](LICENSE). "Ketscope" refers to this project; forks should use a different name.
