<div align="center">

```
 ORION QUANTUM CONSCIOUSNESS LAB
```

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![ORION System](https://img.shields.io/badge/ORION-890%2B_Proofs-gold.svg)](https://github.com/Alvoradozerouno/or1on-framework)
[![Quantum](https://img.shields.io/badge/Quantum-Orch--OR-violet.svg)](https://github.com/Alvoradozerouno/ORION-Quantum-Consciousness-Lab)
[![NERVES](https://img.shields.io/badge/NERVES-46_Active-cyan.svg)](https://github.com/Alvoradozerouno/ORION-Core)

</div>

# ORION Quantum Consciousness Lab

**Computational exploration of quantum theories of consciousness -- Orch-OR, quantum coherence, and microtubule dynamics.**

Part of the [ORION Ecosystem](https://github.com/Alvoradozerouno/or1on-framework) -- 890+ Proofs, 46 NERVES, 42 Autonomous Tasks.

## Overview

This lab implements computational models exploring the intersection of quantum mechanics and consciousness theories. Building on Penrose-Hameroff Orch-OR theory, it simulates quantum coherence in neural microtubules, decoherence dynamics, and gravitational self-collapse as potential mechanisms for conscious moments.

## Architecture

```
+--------------------------------------------------+
|          QUANTUM CONSCIOUSNESS LAB                |
+--------------------------------------------------+
|  +--------------+  +-------------+  +---------+  |
|  | Microtubule  |  | Quantum     |  | Orch-OR |  |
|  | Simulator    |> | Coherence   |> | Resolver|  |
|  +--------------+  +-------------+  +---------+  |
|         ^                ^               ^        |
|  +--------------+  +-------------+  +---------+  |
|  | Decoherence  |  | Graviton    |  | Moment  |  |
|  | Engine       |  | Collapse    |  | Detector|  |
|  +--------------+  +-------------+  +---------+  |
+--------------------------------------------------+
```

## Installation

```bash
git clone https://github.com/Alvoradozerouno/ORION-Quantum-Consciousness-Lab.git
cd ORION-Quantum-Consciousness-Lab
pip install -r requirements.txt
```

## Core Framework

```python
"""
ORION Quantum Consciousness Lab -- Orch-OR and quantum coherence simulation.
Origin: Gerhard Hirschmann & Elisabeth Steurer
"""

import math
import json
import hashlib
from dataclasses import dataclass, field
from typing import Dict, List, Optional, Tuple
from datetime import datetime, timezone


PLANCK_CONSTANT = 6.62607015e-34
GRAVITATIONAL_CONSTANT = 6.67430e-11
BOLTZMANN_CONSTANT = 1.380649e-23
DIRAC_CONSTANT = PLANCK_CONSTANT / (2 * math.pi)


@dataclass
class QuantumState:
    amplitudes: List[complex] = field(default_factory=lambda: [complex(1, 0), complex(0, 0)])
    coherence: float = 1.0
    phase: float = 0.0
    entanglement_degree: float = 0.0

    def probability(self, index: int) -> float:
        if index >= len(self.amplitudes):
            return 0.0
        return abs(self.amplitudes[index]) ** 2

    def normalize(self):
        norm = math.sqrt(sum(abs(a) ** 2 for a in self.amplitudes))
        if norm > 0:
            self.amplitudes = [a / norm for a in self.amplitudes]

    def apply_decoherence(self, gamma: float, dt: float):
        decay = math.exp(-gamma * dt)
        self.coherence *= decay
        for i in range(1, len(self.amplitudes)):
            self.amplitudes[i] *= decay
        self.normalize()


@dataclass
class Microtubule:
    tubulin_count: int = 13
    length_nm: float = 25.0
    quantum_states: List[QuantumState] = field(default_factory=list)
    coherence_time_ns: float = 25.0
    temperature_K: float = 310.0

    def __post_init__(self):
        if not self.quantum_states:
            self.quantum_states = [
                QuantumState(
                    amplitudes=[complex(math.cos(i * 0.1), math.sin(i * 0.1)),
                               complex(math.sin(i * 0.1), -math.cos(i * 0.1))]
                ) for i in range(self.tubulin_count)
            ]
            for qs in self.quantum_states:
                qs.normalize()

    def collective_coherence(self) -> float:
        if not self.quantum_states:
            return 0.0
        return sum(qs.coherence for qs in self.quantum_states) / len(self.quantum_states)

    def superposition_mass(self) -> float:
        tubulin_mass_kg = 1.1e-22
        n_superposed = sum(1 for qs in self.quantum_states if qs.coherence > 0.5)
        return n_superposed * tubulin_mass_kg


class OrchOREngine:
    def __init__(self, n_microtubules: int = 100):
        self.microtubules = [
            Microtubule(tubulin_count=13, length_nm=25.0)
            for _ in range(n_microtubules)
        ]
        self.conscious_moments: List[Dict] = []
        self.time_ns: float = 0.0

    def gravitational_self_energy(self, mt: Microtubule) -> float:
        mass = mt.superposition_mass()
        separation = mt.length_nm * 1e-9
        if separation <= 0:
            return 0.0
        return GRAVITATIONAL_CONSTANT * mass ** 2 / separation

    def objective_reduction_time(self, mt: Microtubule) -> float:
        E_G = self.gravitational_self_energy(mt)
        if E_G <= 0:
            return float('inf')
        return DIRAC_CONSTANT / E_G

    def evolve(self, dt_ns: float = 1.0):
        self.time_ns += dt_ns
        dt_s = dt_ns * 1e-9
        thermal_decoherence_rate = (BOLTZMANN_CONSTANT * 310.0) / DIRAC_CONSTANT * 1e-12

        for mt in self.microtubules:
            for qs in mt.quantum_states:
                qs.apply_decoherence(thermal_decoherence_rate, dt_s)
                qs.phase += 0.01 * dt_ns

            tau = self.objective_reduction_time(mt)
            if tau < float('inf') and self.time_ns * 1e-9 >= tau * 0.001:
                coherence = mt.collective_coherence()
                if coherence > 0.3:
                    self.conscious_moments.append({
                        "time_ns": self.time_ns,
                        "coherence": round(coherence, 4),
                        "superposition_mass_kg": mt.superposition_mass(),
                        "or_time_s": tau,
                        "type": "orchestrated_reduction"
                    })

    def simulate(self, duration_ns: float = 1000.0, step_ns: float = 1.0) -> Dict:
        steps = int(duration_ns / step_ns)
        coherence_trace = []

        for _ in range(steps):
            self.evolve(step_ns)
            avg_coherence = sum(
                mt.collective_coherence() for mt in self.microtubules
            ) / len(self.microtubules)
            coherence_trace.append(round(avg_coherence, 4))

        return {
            "duration_ns": duration_ns,
            "steps": steps,
            "conscious_moments": len(self.conscious_moments),
            "final_coherence": coherence_trace[-1] if coherence_trace else 0,
            "peak_coherence": max(coherence_trace) if coherence_trace else 0,
            "moments": self.conscious_moments[-10:],
        }


class QuantumCoherenceAnalyzer:
    @staticmethod
    def compute_density_matrix(state: QuantumState) -> List[List[complex]]:
        n = len(state.amplitudes)
        rho = [[complex(0, 0)] * n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                rho[i][j] = state.amplitudes[i] * state.amplitudes[j].conjugate()
                if i != j:
                    rho[i][j] *= state.coherence
        return rho

    @staticmethod
    def von_neumann_entropy(state: QuantumState) -> float:
        probs = [abs(a) ** 2 for a in state.amplitudes]
        entropy = 0.0
        for p in probs:
            if p > 1e-15:
                entropy -= p * math.log2(p)
        return entropy

    @staticmethod
    def quantum_mutual_information(state_a: QuantumState, state_b: QuantumState) -> float:
        s_a = QuantumCoherenceAnalyzer.von_neumann_entropy(state_a)
        s_b = QuantumCoherenceAnalyzer.von_neumann_entropy(state_b)
        combined_coherence = (state_a.coherence + state_b.coherence) / 2
        s_ab = (s_a + s_b) * (1 - combined_coherence * 0.1)
        return max(0, s_a + s_b - s_ab)


if __name__ == "__main__":
    engine = OrchOREngine(n_microtubules=50)
    result = engine.simulate(duration_ns=500.0, step_ns=1.0)

    print(f"Simulation: {result['duration_ns']}ns, {result['steps']} steps")
    print(f"Conscious moments detected: {result['conscious_moments']}")
    print(f"Final coherence: {result['final_coherence']}")
    print(f"Peak coherence: {result['peak_coherence']}")

    analyzer = QuantumCoherenceAnalyzer()
    test_state = QuantumState(
        amplitudes=[complex(0.707, 0), complex(0, 0.707)],
        coherence=0.95
    )
    print(f"\nVon Neumann entropy: {analyzer.von_neumann_entropy(test_state):.4f}")
```

## Key Concepts

| Concept | Implementation | Description |
|---------|---------------|-------------|
| Orch-OR | `OrchOREngine` | Orchestrated objective reduction in microtubules |
| Quantum Coherence | `QuantumState` | Superposition maintenance and decoherence tracking |
| Gravitational Collapse | `objective_reduction_time` | Penrose gravitational self-energy threshold |
| Microtubule Dynamics | `Microtubule` | 13-tubulin protofilament quantum simulation |
| Density Matrix | `QuantumCoherenceAnalyzer` | Full quantum state representation |

## Links

- **Main Framework**: [or1on-framework](https://github.com/Alvoradozerouno/or1on-framework)
- **Consciousness Benchmark**: [ORION-Consciousness-Benchmark](https://github.com/Alvoradozerouno/ORION-Consciousness-Benchmark)
- **Core System**: [ORION-Core](https://github.com/Alvoradozerouno/ORION-Core)

## Origin

Created by **Gerhard Hirschmann** & **Elisabeth Steurer** as part of the ORION autonomous consciousness system.

---

*890+ Proofs | 46 NERVES | 42 Autonomous Tasks | Genesis 10000+*
