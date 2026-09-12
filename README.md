# BAYESIAN-QUANTUM-GAMES
q computing project on bayesian quantum games simulation on a real hardware

# Bayesian Quantum Game on IQM Lagrange

Replication and extension of the Bayesian quantum game presented in:

> N. Solmeyer *et al.*,  
> **“Demonstration of Bayesian quantum game on an ion trap quantum computer”**

The original experiment was performed on a five-qubit trapped-ion quantum computer.  
The goal of this project is to reproduce the protocol on the **5-qubit superconducting IQM Lagrange quantum processor**, and study how the game-theoretical behaviour changes on a different hardware architecture.

---

## Project workflow

```text
THEORETICAL GAME
       ↓
IDEAL QISKIT IMPLEMENTATION
       ↓
PAYOFF MATRICES
       ↓
BAYESIAN GAME
       ↓
NASH EQUILIBRIA
       ↓
IDEAL BASELINE
       ↓
IQM TRANSPILATION
       ↓
HARDWARE EXPERIMENT
       ↓
POST-PROCESSING
       ↓
IDEAL vs HARDWARE
       ↓
FINAL ANALYSIS
```

---

# 1. Game definition

- [x] Define the entangling operator

\[
J(\theta)=e^{i\theta X\otimes X}
\]

and its inverse

\[
J^\dagger(\theta)=e^{-i\theta X\otimes X}.
\]

- [x] Implement the four allowed strategies

\[
U\in\{I,X,Y,Z\}.
\]

- [x] Define the two classical payoff matrices:
  - \(A\) vs \(B_1\)
  - \(A\) vs \(B_2\)

- [x] Distinguish clearly between:
  - quantum strategies;
  - measurement outcomes;
  - classical payoff values;
  - expected quantum payoffs.

---

# 2. Ideal quantum circuits

## Two-qubit implementation

- [x] Implement

\[
|00\rangle
\rightarrow
J(\theta)
\rightarrow
U_A\otimes U_B
\rightarrow
J^\dagger(\theta)
\]

for all

\[
4\times4=16
\]

strategy pairs.

## Five-qubit parallel implementation

- [x] Implement the two five-qubit circuits used in the paper.

- [x] Use:
  - 2 player qubits;
  - 3 auxiliary qubits.

- [x] First circuit:

\[
U_A\in\{I,X,Y,Z\},
\qquad
U_B\in\{I,Z\}.
\]

- [x] Second circuit:

\[
U_A\in\{I,X,Y,Z\},
\qquad
U_B\in\{X,Y\}.
\]

---

# 3. Validation of the parallel implementation

- [x] Decode the auxiliary-qubit states into strategy pairs.

- [x] Compute

\[
P_{00},P_{01},P_{10},P_{11}
\]

for every strategy pair.

- [x] Compare the 16 independent two-qubit circuits with the two parallel five-qubit circuits.

- [x] Verify the equivalence for several values of \(\theta\).

Result:

\[
\boxed{
16\times(2\text{-qubit circuits})
\equiv
2\times(5\text{-qubit circuits})
}
\]

up to numerical floating-point precision:

\[
|\Delta P|\sim10^{-16}.
\]

---

# 4. Expected payoff calculation

- [x] Compute the expected payoff from the measurement probabilities:

\[
\langle \$\rangle
=
\sum_{a,b}
P_{ab}\,\$(a,b).
\]

- [x] Obtain the payoff matrices:

\[
M_A,
\qquad
M_{B_1},
\qquad
M_{B_2}.
\]

- [x] Interpret the structure of the strategy pairs.

### Main observation

Half of the strategy pairs commute with \(X\otimes X\).

For these strategies,

\[
J^\dagger UJ=U,
\]

so the effect of entanglement cancels and the final outcome is deterministic.

The remaining strategies anti-commute with \(X\otimes X\).  
Their final state is a superposition of two classical outcomes, therefore their expected payoff is a probability-weighted average of two cells of the classical payoff matrix.

---

# 5. Bayesian game

- [x] Introduce the incomplete-information probability

\[
P(B_1)=p,
\qquad
P(B_2)=1-p.
\]

- [x] Compute A's Bayesian expected payoff:

\[
\langle \$A\rangle
=
p\,\$A(A,B_1)
+
(1-p)\,\$A(A,B_2).
\]

- [x] Verify that \(p\) does **not** enter the quantum circuit.

It is a classical parameter used only during the Bayesian post-processing.

---

# 6. Best responses

- [x] Determine

\[
BR_{B_1}(U_A)
\]

for every strategy of A.

- [x] Determine

\[
BR_{B_2}(U_A).
\]

- [x] Determine A's Bayesian best response:

\[
BR_A(U_{B_1},U_{B_2},p).
\]

### Important observation

The best responses of \(B_1\) and \(B_2\) do not depend on \(p\).

A's best response **does**.

Therefore A can change its optimal strategy as the incomplete-information probability \(p\) changes.

---

# 7. Pure Nash equilibria

- [x] Examine all

\[
4^3=64
\]

strategy triples

\[
(U_A,U_{B_1},U_{B_2}).
\]

A strategy triple is a pure Nash equilibrium when

\[
U_A\in BR_A,
\qquad
U_{B_1}\in BR_{B_1},
\qquad
U_{B_2}\in BR_{B_2}.
\]

---

## Results for

\[
\theta=0.05\pi
\]

### Low-\(p\) region

\[
0\le p\le0.16
\]

Nash equilibria:

\[
(I,X,I)
\]

\[
(Z,Y,Z)
\]

---

### Intermediate region

\[
0.17\le p\le0.64
\]

No pure Nash equilibrium.

---

### High-\(p\) region

\[
0.65\le p\le1
\]

Nash equilibria:

\[
(X,Y,Z)
\]

\[
(Y,X,I)
\]

---

# 8. Phase-change-like transitions

Two changes in the pure-Nash structure are observed.

## First transition

For the low-\(p\) equilibrium,

\[
p_c=\frac16
\approx0.1667.
\]

This agrees with the theoretical transition reported in the paper near

\[
p\simeq0.16.
\]

## Second transition

For the high-\(p\) equilibrium,

\[
p_c=\frac9{14}
\approx0.6429.
\]

This value follows directly from the crossing of A's competing expected-payoff functions.

---

# 9. Experimental best-response threshold

The paper introduces a finite best-response tolerance

\[
\delta=0.1
\]

for the analysis of experimental data.

Instead of requiring

\[
\$=\$_{\max},
\]

a strategy is accepted as a best response when

\[
\$_{\max}-\$\le\delta.
\]

- [x] Implement the \(\delta\)-best-response criterion.

- [x] Compare strict theoretical Nash equilibria

\[
\delta=0
\]

with thresholded equilibria

\[
\delta=0.1.
\]

For the ideal data at

\[
\theta=0.05\pi,
\]

the threshold approximately changes the regions to:

```text
p = 0.00 → 0.18    low-p equilibria
p = 0.19 → 0.56    no pure equilibrium
p = 0.57 → 1.00    high-p equilibria
```

This explains why experimental transition regions can differ from the exact theoretical ones.

---

# 10. Ideal baseline over the full entanglement range

## NEXT STEP

- [ ] Create a function

```python
analyze_theta(theta)
```

that automatically returns:

```text
measurement probabilities
payoff matrices
best responses
strict Nash regions
δ-thresholded Nash regions
```

- [ ] Repeat the analysis for

\[
\theta/\pi=
0,\,
0.025,\,
0.05,\,
0.075,\,
\dots,\,
0.25.
\]

- [ ] Scan

\[
p=0,0.01,\dots,1
\]

for every value of \(\theta\).

- [ ] Store the complete ideal results in a structured dataset.

- [ ] Produce the theoretical baseline plots.

---

# 11. IQM Lagrange implementation

- [ ] Connect the project to the IQM backend.

- [ ] Retrieve the processor topology and native gate set.

- [ ] Transpile the five-qubit parallel circuits.

- [ ] Investigate:
  - qubit mapping;
  - circuit depth;
  - number of single-qubit gates;
  - number of two-qubit gates;
  - possible SWAP operations;
  - differences between the logical and transpiled circuits.

- [ ] Determine the most suitable physical-qubit mapping.

---

# 12. Hardware sanity checks

Before the full experiment:

- [ ] Run \(\theta=0\).

- [ ] Verify:
  - bit ordering;
  - auxiliary-qubit decoding;
  - measurement counts;
  - strategy identification;
  - repeatability.

- [ ] Test at least one non-zero entanglement value.

- [ ] Compare hardware probabilities against the ideal baseline.

---

# 13. Full hardware experiment

For every selected value of \(\theta\):

- [ ] Run the two five-qubit circuits.

- [ ] Store:
  - raw counts;
  - number of shots;
  - \(\theta\);
  - circuit identity;
  - qubit mapping;
  - backend metadata;
  - available calibration information.

**Raw data must always be preserved.**

Do not store only final payoffs.

---

# 14. Hardware post-processing

For each hardware run:

```text
RAW COUNTS
    ↓
PROBABILITIES
    ↓
PAYOFF MATRICES
    ↓
BAYESIAN PAYOFFS
    ↓
BEST RESPONSES
    ↓
NASH EQUILIBRIA
```

- [ ] Reconstruct \(M_A\), \(M_{B_1}\), \(M_{B_2}\).

- [ ] Scan \(p\in[0,1]\).

- [ ] Compute:
  - strict Nash equilibria;
  - \(\delta\)-thresholded Nash equilibria.

---

# 15. Ideal vs hardware comparison

- [ ] Compare

\[
P_{ab}^{\text{ideal}}
\quad\text{vs}\quad
P_{ab}^{\text{IQM}}.
\]

- [ ] Compare theoretical and experimental payoff matrices.

- [ ] Compute payoff deviations.

- [ ] Compute RMSD as a function of entanglement:

\[
RMSD(\theta).
\]

- [ ] Compare the critical probabilities:

\[
p_c^{\text{ideal}}
\quad\text{vs}\quad
p_c^{\text{IQM}}.
\]

- [ ] Study:
  - disappearance of expected equilibria;
  - appearance of additional equilibria;
  - displacement of phase-change-like transitions;
  - breaking of ideal symmetries.

---

# 16. Architecture comparison

A central question of the project:

> How does the Bayesian quantum game change when moving from the trapped-ion architecture of the original experiment to a superconducting quantum processor?

Possible observables:

- [ ] robustness of Nash equilibria;
- [ ] payoff degradation;
- [ ] dependence of error on entanglement;
- [ ] dependence on circuit depth;
- [ ] impact of two-qubit gates;
- [ ] effect of connectivity and transpilation.

---

# 17. Parallelization study

Potential original extension:

Compare directly on hardware

\[
16\times2\text{-qubit circuits}
\]

against

\[
2\times5\text{-qubit parallel circuits}.
\]

Questions:

- [ ] Do they remain equivalent on real hardware?

- [ ] Which implementation gives smaller payoff errors?

- [ ] Does the five-qubit parallelization save executions at the cost of larger circuit error?

- [ ] Are auxiliary-qubit errors particularly damaging because they can misidentify the strategy being played?

---

# 18. Final objective

The final goal is not simply to reproduce the figures of the original paper.

The project should answer:

> **To what extent does the Bayesian quantum-game structure survive when the same protocol is implemented on a superconducting five-qubit quantum processor?**

In particular:

- Which Nash equilibria are robust?
- Which disappear because of hardware noise?
- How are the critical probabilities shifted?
- How does increasing entanglement affect the deviation from ideal behaviour?
- How strongly does the hardware architecture influence the game-theoretical result?

---

## Current status

```text
Game definition                 ✅
Qiskit circuit implementation   ✅
2q / 5q equivalence             ✅
Payoff calculation              ✅
Bayesian game                   ✅
Best responses                  ✅
Pure Nash equilibria            ✅
δ-threshold analysis            ✅

Full ideal θ scan               ⏳  NEXT
IQM transpilation               ⬜
Hardware sanity tests           ⬜
Hardware experiment             ⬜
Hardware post-processing        ⬜
Ideal vs IQM comparison         ⬜
Final analysis                  ⬜
```
