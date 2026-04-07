---
layout: default
title: "Risk-Informed Qualification Framework for Robotic Remote Shutdown"
nav_exclude: true
---

# A Risk-Informed Qualification Framework for Robotic Remote Shutdown

**Ria Cheruvu** · Stanford University — AA228V · April 2026

---

## Abstract

Traditional safety protocols for robotic manual valve actuation in high-dose nuclear environments rely on subjective margins, which routinely produce excessive mission downtime or dangerously unquantified tail risks. This paper presents a deterministic, risk-informed qualification framework that replaces those heuristic margins with formally verified reachability envelopes.

We model the remote shutdown task as a Partially Observable Markov Decision Process (POMDP) with a 7D augmented state space that explicitly encodes two-step communication lag and radiation-induced "salt-and-pepper" observation noise. To maintain tractability over long horizons (T=100), we present a linearized interval-arithmetic reachability approach to generate a formal safety certificate σ\*—a mathematical proof that the system remains within safe bounds under the modeled noise regime. The guarantee is stress-tested by a GPU-accelerated validation module using Defensive Mixture Importance Sampling (IS).

While the baseline policy exhibited unacceptable safety violations, tuning the controller to minimize residence time in the high-dose gradient allowed the framework to certify a strict mission success rate of **95.4% ± 0.5%**, bounding the residual catastrophic failure probability at **0.0463 ± 0.0025**.

---

## I. Introduction

In nuclear power plants, the remote shutdown panel (RSP) exists to bring a reactor to a safe, cold-shutdown state when the main control room is inaccessible. Manual actuation of high-pressure coolant valves requires navigating irradiated corridors to apply precise mechanical force. Historically, this falls to human operators authorized to absorb up to 250 millisieverts (mSv) in emergencies—a hard physiological limit that defines the clock on every shutdown operation.

The introduction of robotic agents into this workflow removes the most dangerous element from the human operator. However, ionizing radiation disrupts sensor electronics directly. Gamma-ray strikes cause transient bit-flips in vision and LIDAR sensors, producing sudden, random "salt-and-pepper" corruption of observed position data. Furthermore, as the robot approaches the valve, the dose rate increases by the inverse square of proximity, meaning precise sensor feedback is most likely to be degraded exactly when it is most critical.

Safety engineers have traditionally relied on ALARA (As Low As Reasonably Achievable), a qualitative principle directing that dose be minimized without specifying formal bounds. Recent industry advocacy has critiqued the blind application of ALARA-driven conservatism, establishing an urgent need for qualification frameworks that rely on rigorous failure probability bounds rather than heuristic margin-setting.

We propose a qualification framework that models three primary uncertainty sources: (1) radiation-induced observation noise, (2) surface-slippage transition uncertainty, and (3) communication latency—producing both a formal reachability certificate and a statistically rigorous failure probability estimate.

---

## II. Problem Formulation

### Specifications (φ)

The mission must satisfy two simultaneous specifications.

**Safety Specification (φ_safe):** The robot must not deviate more than R_safe = 0.5m from the nominal corridor centerline and must not accumulate a radiation dose exceeding D_max = 50 mSv.

**Task Specification (φ_task):** The robot must reach the manual valve assembly within a success radius of 0.15m, determined by the mechanical envelope of the valve actuator interface.

These specifications are in structural tension: reaching the valve requires approaching the radiation source, exponentially increasing dose accumulation.

### Augmented State Space

To handle communication latency, we encode a 2-step temporal lag directly in the state vector rather than modeling stochastic delay distributions (which cause interval bounds to branch exponentially):
````
s = [x, y, θ, v_{t-1}, ω_{t-1}, v_{t-2}, ω_{t-2}]
````

Transition dynamics at time t are driven by the commands in [v_{t-2}, ω_{t-2}]. By treating the delay as a deterministic FIFO buffer, the system maintains differentiable transition dynamics for Jacobian computation. Sensor corruption is modeled as a 5% probability per-step of a full position scramble (±2m error).

---

## III. Approach

### Linearized Interval-Arithmetic Reachability

Full nonlinear reachability over T=100 steps produces interval blow-up. We instead propagate an axis-aligned *IntervalBox* Δs_t through the linearized dynamics:
````
Δs_{t+1} ≈ A_t Δs_t + ω
````

where A_t is the Jacobian of the nominal dynamics evaluated at time t, and ω is a Minkowski-sum noise bound scaled by √Δt to accurately reflect random-walk accumulation. A critical implementation detail: sensor uncertainty perturbs the control signal computed from a corrupted observation, which enters through the lag buffer—not directly into heading θ. Failing to respect this structure produces a physically incorrect certificate.

The certificate σ\* applies specifically to the closed-loop dynamics of the active proportional controller, not as a blanket guarantee for any arbitrary policy.

### GPU-Accelerated Importance Sampling

Reachability provides a deterministic outer bound; Importance Sampling provides the empirical failure probability within it. Because safety failures are driven by rare, extreme radiation events, Naive Monte Carlo is structurally unstable. We use a **70/30 Defensive Mixture IS scheme**:
````
q(x) = 0.7 · p(x) + 0.3 · p_p(x)
````

where p_p(x) is a biased proposal with σ_p = 2.2σ. The 70% nominal component prevents weight collapse and preserves Effective Sample Size (ESS); the 30% biased component generates sufficient tail exposure to quantify rare cascade failures.

---

## IV. Results

### Mission Performance

Initial evaluations revealed structural tension between task completion and radiation safety. While the robot physically reached the valve in **99.2% of trials** (Task Liveness), the strict safety compliance rate was much lower—slow traversal through the final inverse-square gradient caused the robot to exceed the 50 mSv limit.

After tuning the proportional controller for higher transit velocity (0.5 m/s) and aggressive heading recovery (K_p = 2.5), the framework achieved a **strict mission success rate of 95.4% ± 0.5%**, comfortably exceeding the 90% qualification threshold. Remaining failures were exclusively attributable to cascading sensor scrambles—multiple consecutive displacements pushing the robot beyond R_safe before the controller could recover.

### Validation Dashboard

![Validation Dashboard](https://raw.githubusercontent.com/riacheruvu/riacheruvu.github.io/refs/heads/main/robotic_shutdown.png)

*Figure 1: Validation Dashboard over the 100-step horizon. Formal reachability bounds (blue boxes) track the nominal path, widening as uncertainty accumulates. Pink MC rollout traces reveal salt-and-pepper scramble signatures. The inverse-square radiation gradient is visible as the concentric heat map centered on the valve.*

### Sampling Strategy Ablation

| Strategy | P(fail) ± σ | ESS | Assessment |
|:---|:---|:---|:---|
| Naive Monte Carlo | 0.0433 ± 0.0020 | 10000 / 10000 | Baseline |
| **Defensive Mix (2.2σ)** | **0.0463 ± 0.0025** | **7000 / 10000** | **Best stability** |
| Aggressive Tail (3.2σ) | 0.0484 ± 0.0031 | 5000 / 10000 | High variance |

In a regulatory context, the naive MC estimate of 0.0433 is not just wrong—it actively *understates* the true risk. Underestimating rare, catastrophic tail events is the most dangerous kind of error in safety qualification, making the stabilized Defensive Mixture approach essential.

---

## V. Limitations and Future Work

Moving from qualitative heuristics to quantitative bounds shifts the burden of safety onto model accuracy (the sim-to-real gap). Our guarantees assume the 5% salt-and-pepper noise distribution accurately reflects real-world hardware degradation—if physical radiation faults exhibit different temporal correlations or higher frequencies, the formal bounds will not hold during deployment. Additionally, radiation-induced visual degradation was abstracted as a full-position scramble; closing the perception-action loop in physical deployment requires mapping raw pixel-corruption directly into the POMDP observation matrix.

Future work will integrate **Adaptive Stress Testing (AST)** to formulate falsification as a reinforcement learning optimization, enabling automated discovery of the worst-case spatial configurations of radiation-induced sensor faults.

---
## Resources

**Full Paper:** [A Risk-Informed Qualification Framework for Robotic Remote Shutdown (PDF)](https://github.com/riacheruvu/riacheruvu.github.io/blob/main/Risk_Informed_Robotic_Remote_Shutdown.pdf)

**Validation Dashboard Animation:**

<video width="100%" height="auto" controls autoplay loop muted>
  <source src="/assets/shutdown_animation.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

## Citation
```bibtex
@article{cheruvu2026nuclear,
  title={A Risk-Informed Qualification Framework for Robotic Remote Shutdown},
  author={Cheruvu, Ria},
  journal={Stanford University - AA228V},
  year={2026}
}
```
---
*[← Back to Projects](./projects.html)*