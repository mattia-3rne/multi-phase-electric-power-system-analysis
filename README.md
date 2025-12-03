# Multi-Phase Electric Power System Analysis

## ⚡ Project Overview

The goal of this project is to analyze and simulate the behavior of **Multi-Phase Alternating Current (AC) Systems**. It specifically focuses on the calculation of neutral currents, resistive losses, and instantaneous power flows in systems with an arbitrary number of phases ($N$).

This project uses Vector algebra and Time-Domain simulation to demonstrate the mathematical efficiency of balanced polyphase systems and the consequences of load imbalances in a **Star Connection**.

---

## 🧠 Theoretical Background

### 1. The Star Connection

In a Star topology, $N$ distinct phases meet at a common central node called the **Star Point** (*Sternpunkt*). This creates a circuit where the return path for the current is provided by a shared **Neutral Conductor**.

#### Kirchhoff's Node Rule
The fundamental calculation for this system is based on Kirchhoff's Current Law (KCL). For a system with $N$ phases, the instantaneous current flowing into the neutral line $i_N(t)$ is the algebraic sum of all individual phase currents:

$$
i_N(t) = \sum_{k=1}^{N} i_k(t)
$$

If we define the current for the $k$-th phase with amplitude $\hat{i}_k$ and phase shift $\varphi_k$:

$$
i_k(t) = \hat{i}_k \cdot \sin(\omega t + \varphi_k)
$$

Then the neutral current is the superposition of these $N$ sine waves. In a **perfectly balanced system**, where all amplitudes $\hat{i}$ are equal and phases are shifted equidistantly by $2\pi/N$, this sum is exactly zero.

$$
\text{Balanced Case:} \quad i_N(t) = 0 \quad \forall t
$$

---

### 2. The Phenomenon of Constant Power Transfer

A unique property of symmetric polyphase systems is that the **total instantaneous power** delivered to a resistive load is constant over time, unlike single-phase systems where power pulsates.

For a general system with $N$ phases ($N \geq 3$), assuming a symmetric voltage $u(t)$ and current $i(t)$ with a power factor angle $\phi$:

The instantaneous power $p_k(t)$ of the $k$-th phase is:

$$
p_k(t) = u_k(t) \cdot i_k(t) = \left[ \hat{u} \cos\left(\omega t - \frac{2\pi k}{N}\right) \right] \cdot \left[ \hat{i} \cos\left(\omega t - \frac{2\pi k}{N} - \phi\right) \right]
$$

Using the trigonometric identity $\cos A \cos B = \frac{1}{2}[\cos(A-B) + \cos(A+B)]$:

$$
p_k(t) = \frac{\hat{u}\hat{i}}{2} \left[ \cos(\phi) + \cos\left(2\omega t - \frac{4\pi k}{N} - \phi\right) \right]
$$

The **Total Power** $P_{total}(t)$ is the sum over all $N$ phases:

$$
P_{total}(t) = \sum_{k=0}^{N-1} p_k(t) = \underbrace{\sum_{k=0}^{N-1} \frac{\hat{u}\hat{i}}{2} \cos(\phi)}_{\text{Constant Term}} + \underbrace{\sum_{k=0}^{N-1} \frac{\hat{u}\hat{i}}{2} \cos\left(2\omega t - \frac{4\pi k}{N} - \phi\right)}_{\text{Oscillating Term}}
$$

**The Result:**
The "Oscillating Term" represents the sum of $N$ vectors spaced equally around a circle (phasors). Geometrically, these vectors always sum to zero for $N \geq 3$. Therefore, the oscillating component vanishes, leaving only the constant term:

$$
P_{total} = \frac{N}{2} \hat{u} \hat{i} \cos(\phi) = \text{Constant}
$$

This mathematically proves why multi-phase motors run smoothly with constant torque and why generators experience a constant mechanical load.

---

### 3. Resistive Line Losses

In real-world applications, the conductors have internal resistance. This project calculates the Conduction Losses.

#### Neutral Loss Calculation
In an unbalanced Star connection, the neutral current $i_N \neq 0$. The instantaneous power dissipated as heat in the neutral wire (resistance $R_N$) is:

$$
P_{loss, N}(t) = i_N(t)^2 \cdot R_N = \left( \sum_{k=1}^{N} \hat{i}_k \sin(\omega t + \varphi_k) \right)^2 \cdot R_N
$$

This highlights the inefficiency of unbalanced loads: they generate heat in the return path without performing useful work.

---

## ⚠️ Limitations

| Limitation | Description |
| :--- | :--- |
| **Harmonics** | The derivation assumes pure sine waves. Non-linear loads (like LEDs or rectifiers) introduce harmonic frequencies (3rd, 5th, etc.) that do not sum to zero in the neutral line, even in balanced systems. |
| **Impedance Symmetry** | The "Constant Power" proof assumes the grid impedance is perfectly symmetric. In reality, slight differences in cable lengths can introduce oscillations. |
| **Steady State** | This analysis models steady-state operation. Transient spikes during switching events are not covered by these equations. |

---

## 🚀 Getting Started

### Prerequisites
* Python 3.8+
* Jupyter Notebook

### Installation

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/your-username/multi-phase-electric-power-system-analysis.git
    ```

2.  **Install dependencies**:
    ```bash
    pip install -r requirements.txt
    ```

3.  **Launch Jupyter Notebook**:
    ```bash
    jupyter notebook
    ```
