---
layout: default
title: "A Risk-Informed Qualification Framework for Robotic Remote Shutdown"
parent: Blogs
nav_exclude: false
has_toc: true
---
# **A Risk-Informed Qualification Framework for Robotic Remote Shutdown**  
**Ria Cheruvu** · Stanford University — AA228V · April 2026
(Written with the help of AI)
---

When a nuclear plant's main control room becomes inaccessible, a human operator has to walk into an irradiated corridor, find the right valve, and turn it. They're authorized to absorb up to 250 millisieverts (mSv) doing it—a hard physiological limit that defines exactly how much time they have before the mission becomes a medical emergency.

Robots are an obvious answer. They don't accumulate biological dose, don't fatigue, and don't need to be evacuated. But substituting a robot doesn't eliminate the hard problem—it transforms it. Ionizing radiation scrambles sensor electronics directly, causing random, high-magnitude position errors at exactly the moments when precise feedback matters most. And the closer the robot gets to the valve, the worse it gets.

The bigger question isn't whether a robot can physically reach the valve—in my experiments, it does so 99.2% of the time. The harder question is whether it can do so **safely**, within dose limits, without drifting into unshielded zones. That distinction—between task completion and safe task completion—is what this framework is built around.

The dominant safety paradigm in nuclear environments is **ALARA**: *As Low As Reasonably Achievable*. It's a sensible principle with a real limitation: it's qualitative. In practice, it produces conservative margins applied to worst-case scenarios, regardless of the actual mission profile. There's a growing body of work—including a pointed analysis of the Fukushima disaster response—showing that purely minimizing dose without quantifying the trade-offs of protective actions can produce outcomes that do more harm than good.

So the question becomes: **what would a more quantitative, decision-relevant alternative actually look like?**  
This framework is my attempt to offer something more rigorous: formal reachability bounds and statistically grounded failure probability estimates that can support a regulatory decision rather than just a heuristic one.

---

## Modeling Remote Shutdown

Traditional safety protocols for robotic manual valve actuation in high-dose nuclear environments rely on subjective margins, which routinely produce excessive mission downtime or dangerously unquantified tail risks. This paper presents a deterministic, risk-informed qualification framework that replaces those heuristic margins with formally verified reachability envelopes.

We model the remote shutdown task as a Partially Observable Markov Decision Process (POMDP) with a 7D augmented state space that explicitly encodes two-step communication lag and radiation-induced "salt-and-pepper" observation noise. To maintain tractability over long horizons ($T=100$), we present a linearized interval-arithmetic reachability approach to generate a formal safety certificate $\sigma^*$—a mathematical proof that the system remains within safe bounds under the modeled noise regime. The guarantee is stress-tested by a GPU-accelerated validation module using Defensive Mixture Importance Sampling (IS).

While the baseline policy exhibited unacceptable safety violations, tuning the controller to minimize residence time in the high-dose gradient allowed the framework to certify a strict mission success rate of **95.4% ± 0.5%**, bounding the residual catastrophic failure probability at **0.0463 ± 0.0025**.

---

## Problem Formulation

### Specifications ($\phi$)

The mission must satisfy two simultaneous specifications:

* **Safety Specification ($\phi_{safe}$):** The robot must not deviate more than $R_{safe} = 0.5m$ from the nominal corridor centerline and must not accumulate a radiation dose exceeding $D_{max} = 50 mSv$.
* **Task Specification ($\phi_{task}$):** The robot must reach the manual valve assembly within a success radius of $0.15m$, determined by the mechanical envelope of the valve actuator interface.

These specifications are in structural tension: reaching the valve requires approaching the radiation source, exponentially increasing dose accumulation. Making that tension explicit is essential for any meaningful safety guarantee.

---

## Approach

### Linearized Interval-Arithmetic Reachability

Full nonlinear reachability over $T=100$ steps produces interval blow-up. We instead propagate an axis-aligned *IntervalBox* $\Delta s_t$ through the linearized dynamics:

\[
\Delta s_{t+1} \approx A_t \Delta s_t + \omega
\]

where $A_t$ is the Jacobian of the nominal dynamics evaluated at time $t$, and $\omega$ is a Minkowski-sum noise bound scaled by $\sqrt{\Delta t}$ to accurately reflect random-walk accumulation.

This gives us a deterministic outer envelope—tight enough to be useful, conservative enough to be trusted.

### GPU-Accelerated Importance Sampling

Reachability provides the outer bound; Importance Sampling provides the empirical failure probability *within* it. Because safety failures are driven by rare, extreme radiation events, naive Monte Carlo is structurally unstable. We use a **70/30 Defensive Mixture IS scheme**:

\[
q(x) = 0.7 \cdot p(x) + 0.3 \cdot p_p(x)
\]

where $p_p(x)$ is a biased proposal with $\sigma_p = 2.2\sigma$. The 70% nominal component prevents weight collapse and preserves Effective Sample Size (ESS); the 30% biased component generates sufficient tail exposure to quantify rare cascade failures.

This combination ended up being the only configuration that produced both stability and tail sensitivity.

---

## Results

### Mission Performance

The first result surprised me in a way I needed. The robot reached the valve in 99.2% of trials. By a naive reading, that's a success. But radiation dose accumulates the entire time the robot is moving, and slow traversal through the final approach—where the inverse-square gradient is steepest—pushed it past the 50 mSv limit far more often than expected.

Task liveness and mission success are not the same thing, and designing for one without the other produces a system that looks good on the wrong metric.

![Validation Dashboard](https://raw.githubusercontent.com/riacheruvu/riacheruvu.github.io/refs/heads/main/assets/images/robotic_shutdown.png)

*Figure 1: Validation Dashboard over the 100-step horizon. Formal reachability bounds (blue boxes) track the nominal path, widening as uncertainty accumulates. Pink MC rollout traces reveal salt-and-pepper scramble signatures. The inverse-square radiation gradient is visible as the concentric heat map centered on the valve.*

### Sampling Strategy Ablation

| Strategy | P(fail) ± σ | ESS | Assessment |
|:---|:---|:---|:---|
| Naive Monte Carlo | 0.0433 ± 0.0020 | 10000 / 10000 | Baseline |
| **Defensive Mix (2.2σ)** | **0.0463 ± 0.0025** | **7000 / 10000** | **Best stability** |
| Aggressive Tail (3.2σ) | 0.0484 ± 0.0031 | 5000 / 10000 | High variance |

This is a good example of a broader point: in safety qualification, **statistical stability matters as much as the point estimate itself**. Underestimating rare, catastrophic tail events is the most dangerous kind of error, making the stabilized Defensive Mixture approach essential.

---

## Limitations and Future Work

Moving from qualitative heuristics to quantitative bounds shifts the burden of safety onto model accuracy. Our guarantees assume the 5% salt-and-pepper noise distribution accurately reflects real-world hardware degradation—if physical radiation faults exhibit different temporal correlations, the formal bounds will not hold during deployment.

A natural next step is integrating **Adaptive Stress Testing (AST)** to formulate falsification as a reinforcement learning optimization, enabling automated discovery of the worst-case spatial configurations of radiation-induced sensor faults. This would tighten the loop between modeling assumptions and real-world adversarial conditions.

---

## Links

**Full Paper:** [A Risk-Informed Qualification Framework for Robotic Remote Shutdown (PDF)](https://github.com/riacheruvu/riacheruvu.github.io/raw/main/assets/content/Risk_Informed_Robotic_Remote_Shutdown.pdf)

**Validation Dashboard Animation:**

<video width="100%" height="auto" controls autoplay loop muted>
  <source src="/assets/visual/shutdown_animation.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

---

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

*[← Back to Blogs](./blogs.html)*
```