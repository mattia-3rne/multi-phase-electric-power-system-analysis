# Multi-Phase Electric Power System Analysis

## ⚡ Project Overview

The goal of this project is to analyze and simulate the behavior of **Multi-Phase Alternating Current Systems**. It specifically focuses on the calculation of neutral currents, resistive losses, and instantaneous power flows in systems with an arbitrary number of phases $N$.

This project uses Vector algebra and Time-Domain simulation to demonstrate the mathematical efficiency of balanced polyphase systems and the consequences of load imbalances in a **Star Connection**.

---

## 🧠 Theoretical Background

### The Star Connection

In a Star topology, $N$ distinct phases meet at a common central node called the **Star Point**. This creates a circuit where the return path for the current is provided by a shared **Neutral Conductor**.

#### Kirchhoff's Current Law
The fundamental calculation for this system is based on Kirchhoff's Current Law. For a system with $N$ phases, the instantaneous current flowing into the neutral line $i_N(t)$ is the algebraic sum of all individual phase currents:

$$
i_N(t) = \sum_{k=1}^{N} i_k(t)
$$

If we define the current for the $k$-th phase with amplitude $i_{p,k}$ and phase shift $\varphi_k$:

$$
i_k(t) = i_{p,k} \cdot \sin(\omega t + \varphi_k)
$$

Then the neutral current is the superposition of these $N$ sine waves. In a **perfectly balanced system**, where all amplitudes $i_p$ are equal and phases are shifted equidistantly by $2\pi/N$, this sum is exactly zero:

$$
i_N(t) = 0 \quad \forall t
$$

---

### The Phenomenon of Constant Power Transfer

A fundamental and unique property of symmetric polyphase systems is that the total instantaneous power delivered to a resistive or inductive load remains strictly constant over time. This stands in stark contrast to single-phase systems, where the power pulsates at double the grid frequency, causing mechanical vibrations and torque ripple in motors.

To prove this for a general system with $N$ phases (where $N \geq 3$), we assume a perfectly symmetric supply. Let the voltage $u(t)$ and current $i(t)$ for the $k$-th phase be shifted by an angle of $\frac{2\pi k}{N}$, with a load power factor angle of $\phi$.

The instantaneous power $p_k(t)$ for a single phase $k$ is defined as the product of its instantaneous voltage and current:

$$
p_k(t) = u_k(t) \cdot i_k(t) = \left[ u_p \cos\left(\omega t - \frac{2\pi k}{N}\right) \right] \cdot \left[ i_p \cos\left(\omega t - \frac{2\pi k}{N} - \phi\right) \right]
$$

To separate the constant power component from the oscillating component, we apply the trigonometric product-to-sum identity:

$$
\cos(A) \cos(B) = \frac{1}{2}\left[\cos(A-B) + \cos(A+B)\right]
$$

Applying this identity to our power equation, where $A$ represents the voltage phase and $B$ represents the current phase, we obtain two distinct terms for each phase:

$$
p_k(t) = \frac{u_p i_p}{2} \left[ \cos(\phi) + \cos\left(2\omega t - \frac{4\pi k}{N} - \phi\right) \right]
$$

The total instantaneous power $P_{total}(t)$ of the system is obtained by summing the contributions of all $N$ phases simultaneously:

$$
P_{total}(t) = \sum_{k=0}^{N-1} p_k(t)
$$

This summation can be split into two separate parts:

$$
P_{total}(t) = \sum_{k=0}^{N-1} \frac{u_p i_p}{2} \cos(\phi) + \sum_{k=0}^{N-1} \frac{u_p i_p}{2} \cos\left(2\omega t - \frac{4\pi k}{N} - \phi\right)
$$

**The Analysis of the Sums:**

1.  **The Constant Term:** Since $\cos(\phi)$ does not depend on the index $k$, summing it $N$ times simply multiplies the value by $N$.
2.  **The Oscillating Term:** This term represents the sum of $N$ sinusoidal waves (or phasors), each with the same amplitude and frequency ($2\omega$), but spaced equally apart by a phase angle of $\frac{4\pi}{N}$. In any symmetric system where $N \geq 3$, the sum of these vectors forms a closed polygon in the complex plane, meaning their vector sum is exactly zero.

Consequently, the oscillating term vanishes completely, leaving only the constant term:

$$
P_{total}(t) = \frac{N}{2} u_p i_p \cos(\phi) = \text{Constant}
$$

This mathematical result explains why large-scale industrial motors and generators operate smoothly: the mechanical torque is constant, reducing wear and vibration compared to single-phase machinery.


---

### Resistive Line Losses

In real-world applications, conductors are not ideal superconductors; they possess internal electrical resistance ($R$). As current flows through these conductors, a portion of the electrical energy is irreversibly converted into heat via Joule Heating. This project calculates these conduction losses to quantify system efficiency.

The total resistive loss in the system is the sum of losses in the active phase lines and the return neutral line.

#### A. Phase Conductor Losses
Current must flow through the phase lines ($L_1, L_2, \dots, L_N$) to deliver power to the load. Assuming each phase conductor has a resistance $R_L$, the instantaneous power loss for a specific phase $k$ is:

$$
P_{loss, k}(t) = i_k(t)^2 \cdot R_L
$$

The total loss across all phase conductors is the sum of these individual losses:

$$
\sum_{k=1}^{N} P_{loss, k}(t) = \sum_{k=1}^{N} \left[ i_{p,k} \sin(\omega t + \varphi_k) \right]^2 \cdot R_L
$$

#### B. Neutral Conductor Loss
In a Star connection, the neutral wire carries the vector sum of the phase currents. If the system is perfectly balanced, this current is zero, and the neutral loss is zero. However, in an unbalanced system, the neutral current $i_N \neq 0$, leading to additional heating.

With a neutral line resistance $R_N$, the instantaneous neutral power loss is:

$$
P_{loss, N}(t) = i_N(t)^2 \cdot R_N = \left( \sum_{k=1}^{N} i_{p,k} \sin(\omega t + \varphi_k) \right)^2 \cdot R_N
$$

---

### Total System Loss

To quantify the thermal stress and energy efficiency of the system, we move from instantaneous values to time-averaged metrics. The standard for engineering reporting is Average Power Loss, which relies on the Root Mean Square current.

#### 1. RMS Current Calculation
The RMS value represents the effective current and it is the equivalent DC current that would produce the same amount of heat in a resistor. For any periodic current $i(t)$ with period $T$:

$$
I_{rms} = \sqrt{\frac{1}{T} \int_{0}^{T} i(t)^2 dt}
$$

For the pure sinusoidal currents used in this simulation, this integration simplifies to a constant relationship with the peak amplitude $i_p$:

$$
I_{rms} = \frac{i_p}{\sqrt{2}} \approx 0.707 \cdot i_p
$$

#### 2. Deriving Average Power Loss
The average power loss is the mean of the instantaneous power over one full cycle. For a resistive line $R$:

$$
P_{loss} = \frac{1}{T} \int_{0}^{T} i(t)^2 \cdot R dt = R \cdot \left( \frac{1}{T} \int_{0}^{T} i(t)^2 dt \right)
$$

Therefore, the calculation simplifies to the standard formula:

$$
P_{loss} = I_{rms}^2 \cdot R
$$

#### 3. Total System Power Loss
The project calculates the total average loss by summing the losses in all $N$ phases plus the loss in the neutral return path:

$$
P_{loss, tot} = \sum_{k=1}^{K} (I_{rms, k}^2 \cdot R_L) + (I_{rms, N}^2 \cdot R_N)
$$

The term $P_{loss, N}$ represents the inefficiency of the system configuration. It is energy dissipated in the return path that performs no useful work for the load. Minimizing $i_N$ (by balancing amplitudes and phases) directly reduces this loss component to zero.

#### 4. System Efficiency

The system efficiency defines how much of the input energy is successfully delivered to the load versus how much is wasted as heat in the lines.
* **$P_{load}$:** The useful active power consumed by the load.
* **$P_{in}$:** The total input power supplied by the source.

The efficiency is calculated as:

$$
\eta = \frac{P_{load}}{P_{in}} = \frac{P_{load}}{P_{load} + P_{loss, tot}}
$$

---

## ⚠️ Limitations

| Limitation | Description |
| :--- | :--- |
| **Harmonics** | The derivation assumes pure sine waves. Non-linear loads (like LEDs) introduce harmonic frequencies that do not sum to zero in the neutral line, even in balanced systems. |
| **Impedance Symmetry** | The constant power calculation relies on perfect symmetry. In reality, asymmetry, from factors like cable lengths or component tolerances, means the total power is never truly constant, and minor oscillations are unavoidable. |

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
---

## 📂 Project Structure

* `main.ipynb`: The main analysis notebook.
* `requirements.txt`: Python package dependencies.
* `README.md`: Project documentation.
