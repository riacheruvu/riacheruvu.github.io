---
title: Tech Projects
layout: default
---
# A selection of technical engineering projects

## Bringing Adventure Gaming to Life with Real-Time GenAI (Intel, 2024)
*Originally presented at ACM SIGGRAPH 2024*
[![Demo screen example of project](https://img.youtube.com/vi/6MxMCTWWEYQ/maxresdefault.jpg)](https://www.youtube.com/watch?v=6MxMCTWWEYQ)

**Project Overview:**
This project explores the frontier of "local AI" for immersive entertainment, demonstrating how large-scale Generative AI models can be optimized to run locally on consumer PCs to power real-time, interactive storytelling. Using Dungeons & Dragons as a primary use case, [the work](https://dl.acm.org/doi/10.1145/3641233.3664354) showcases how AI can serve as a "Digital Dungeon Master"—dynamically generating complex narratives, character dialogue, and environmental visuals without the latency or privacy concerns of cloud-based APIs.

**Key Contributions:**

  * **Local Inference Optimization:** Leveraged the OpenVINO toolkit to compress and accelerate LLMs and image generation models. The system utilizes the NPU for whisper transcription and stable diffusion, the GPU for Llama 3 inference, and the CPU for depth mapping and super-resolution [[03:49](http://www.youtube.com/watch?v=6MxMCTWWEYQ&t=229)].
  * **Multimodal Integration:** Developed a pipeline that synchronizes speech-to-text (Whisper) with narrative refinement (Llama 3) and real-time image synthesis (LCM Stable Diffusion) to create a cohesive visual experience with Parallax depth effects [[02:47](http://www.youtube.com/watch?v=6MxMCTWWEYQ&t=167)].
  * **Human-AI Collaboration:** Researched UI/UX frameworks that allow AI to augment, rather than replace, human creativity in tabletop gaming, which *Laptop Mag* noted as a [potential "revolution"](https://www.laptopmag.com/ai/bringing-adventure-gaming-to-life-could-revolutionize-your-next-dandd-session) for the next generation of D\&D sessions.

**Technical Demo:** [Watch the demo at IFS 2024 - "AI Brings Adventure Games to Life on Intel Core Ultra 200V Series | Talking Tech | Intel Technology
 on YouTube"](https://www.youtube.com/watch?v=6MxMCTWWEYQ)
 
## **Hybrid GenAI: Orchestrating Stable Diffusion with OpenVINO & Red Hat OpenShift (Intel, 2024)**

*Technical Case Study with Intel & Red Hat*

**Project Overview:**
As Generative AI shifts from research to enterprise production, the challenge is balancing the "limitless" compute of the cloud with the cost-efficiency and privacy of the edge. This project demonstrates a **Hybrid AI approach**, using the OpenVINO toolkit and Red Hat OpenShift Data Science (RHODS) to deploy Stable Diffusion across diverse hardware environments.

**Key Technical Contributions:**

  * **Cross-Stack Orchestration:** Implemented a seamless workflow to switch model inference between cloud GPUs and local Intel CPUs/GPUs without changing the underlying application logic.
  * **Extreme Optimization:** Leveraged the Hugging Face **Optimum-Intel** library to convert FP32 models to FP16/INT8, reducing the memory footprint of Stable Diffusion by over 50% (from 4.9GB to 2.4GB) while maintaining image quality.
  * **Enterprise Readiness:** Integrated Intel-certified drivers and plugins into the OpenShift lifecycle, enabling automated monitoring and management of high-throughput GenAI pipelines.

[Read the full technical deep-dive on Medium](https://medium.com/intel-tech/demo-generative-ai-with-intel-openvino-and-red-hat-open-shift-56264e94115c)

## **Responsible AI @ ONNX: Metadata, Model Cards, and Provenance (Intel, 2022)**

*Presented at the ONNX Steering Committee & Community Meeting*

[![Responsible AI @ ONNX](https://img.youtube.com/vi/U6L-bBKApA4/hqdefault.jpg)](https://www.youtube.com/watch?v=U6L-bBKApA4)

**Project Overview:**
This project addresses the critical need for machine-readable transparency in AI models. I led a proposal to extend the **ONNX (Open Neural Network Exchange)** specification to include standardized metadata for **Responsible AI**. By integrating Model Cards and provenance tracking directly into the model graph, we enable developers to automate compliance, track data lineage, and identify potential biases before deployment [[03:31](http://www.youtube.com/watch?v=U6L-bBKApA4&t=211)].

**Key Contributions:**

  * **Standardization:** Proposed an RDF-based machine-readable metadata format to allow for decentralized and extensible model characterization [[06:50](http://www.youtube.com/watch?v=U6L-bBKApA4&t=410)].
  * **Automated Governance:** Developed a pipeline for querying model metadata using Sparkle, allowing organizations to filter model zoos based on ethical constraints like carbon footprint or data privacy [[14:54](http://www.youtube.com/watch?v=U6L-bBKApA4&t=894)].
  * **Industry Collaboration:** Worked across the ONNX community to bridge the gap between human-readable documentation (Model Cards) and executable runtime checks [[19:18](http://www.youtube.com/watch?v=U6L-bBKApA4&t=1158)].

  
## Reinforcement Learning and Intuitive Physics (Intel, 2020)

Artificial intuitive physics explores computational models that simulate human-like physical reasoning. This project aimed to reverse-engineer "intuitive theories" of physics and implement them within AI algorithms. My work during a research collaboration with Intel Labs focused on building intuitive physics engines in simulated environments using reinforcement learning, enabling agents to interact with their surroundings in ways transferrable to real-world robotics. The research leveraged Pybullet and OpenAI Gym for physics simulation, along with other libraries, and included the implementation of reinforcement learning algorithms from scratch—such as Proximal Policy Optimization and World Models.

Three deliverables resulted from this project:

1. I authored a report titled Intuitive Physics and Simulation Tools for Active Agents (2020 draft) surveys AI-driven physical reasoning and simulation methods (e.g., FEM, SPH, IPEs) for robotics and scene understanding. Paused due to project reprioritization, with some incomplete sections (e.g., causality hardware), it showcases my interdisciplinary synthesis skills, informing my ongoing AI-driven simulation research. [Link](https://github.com/riacheruvu/riacheruvu.github.io/blob/main/2020%20Rough%20Draft%20Report%20-%20Simulation%20Tools%20and%20Intuitive%20Physics.pdf) 

2.	Developed ML engines that can perform “common sense reasoning” inspired by human reasoning at the intersection of computer vision, reinforcement learning (RL), computational neuroscience, and cognitive development. Implemented intuitive physics engines in a simulated environment using RL, enabling agents to interact with their surroundings applied to a real-life robot. Leveraged Pybullet, OpenAI Gym, and other libraries for physics simulation and different RL algorithms from scratch, such as Proximal Policy Optimization and World Models.
![Snapshot of RL experiments](https://raw.githubusercontent.com/riacheruvu/riacheruvu.github.io/refs/heads/main/rl_image.png)

3. I conducted research on modeling uncertainty in intuitive physics environments for active agents, exploring Bayesian neural networks and their alignment with hardware-level Instruction Set Architecture (ISA) customization. This work led to the identification of design pathways for AI models that effectively integrate physics reasoning with low-level architectural support.

# Featured Academic Projects
---
**Risk-informed remote robotic shutdown for nuclear environments (Stanford, 2026)**
![Snapshot of demo for robotic shutdown project](https://raw.githubusercontent.com/riacheruvu/riacheruvu.github.io/refs/heads/main/robotic_shutdown.png)
This project presents a **risk-informed qualification framework** for autonomous robotic remote shutdown in high-radiation nuclear environments. The system models the task as a **POMDP** with a 7D augmented state space encoding communication lag and radiation-induced sensor noise. A **linearized interval-arithmetic reachability analysis** generates a formal safety certificate, while GPU-accelerated **Defensive Mixture Importance Sampling** bounds failure probability. 

**Results:** Certified **99.2% ± 0.2% mission success rate** over 10,000 rollouts.

---

**Bayesian Deep Learning (Harvard, 2019):**

![Snapshot of aleatoric uncertainty for a computer vision dataset](https://raw.githubusercontent.com/riacheruvu/riacheruvu.github.io/refs/heads/main/aleatoric_uncertainty.png)
- **Project Summary:**
Research on epistemic and aleatoric uncertainties for Bayesian Deep Learning (2019). For this project, we extend the research by Alex Kendall and Yarin Gal from the paper "What Uncertainties Do We Need in Bayesian Deep Learning for Computer Vision?" We provide a synthesis of this paper and evaluate the claims made by the authors. We implemented pedagogical examples of neural networks with and without epistemic and aleatoric uncertainty and evaluated these models on toy regression and classification tasks, in addition to the MNIST and CIFAR-10 datasets. We also implement experiments such as the effect of model complexity on the performance of a model incorporating epistemic uncertainty, and the effect of data transformations on epistemic and aleatoric uncertainty.

[Colab notebook](https://colab.research.google.com/github/onefishy/am207_fall19_projects/blob/master/what_uncertainties/what_uncertainties_3/cheruvuria_136145_9127626_Final_Project_Submission.ipynb)
---

**Automated Data Engineering Workflow (Harvard, 2019)**

- **Project Summary:**
Automated Data Engineering Using Dask and Luigi (2019). This project implements an automated data engineering pipeline - Given large input dataset CSV files and user-defined preferences, the data is cleaned, data types are automatically determined, and the workflow outputs a visualization HTML report (which is atomically written to the user's local file system) for the categorical and numerical variables in the dataset.
---

**Healthcare Streaming Data Visualization Pipeline (Harvard, 2019)**

![Streaming Data Pipeline](https://raw.githubusercontent.com/riacheruvu/riacheruvu.github.io/refs/heads/main/project_streaming_data.png)
- **Project Summary:**
Healthcare Streaming Data Visualization Pipeline (2019). Can we predict and visualize the percent of adults who have an overweight classification in a particular region in the US based on factors such as income, gender, or race? The goal of this project was to simulate a real-life streaming application using Kibana, ElasticSearch, Kafka, and Spark for a potential use case where health care service providers and insurance organizations are consistently providing data and querying a system on the patterns in the obesity epidemic across the United States.
---

**Neural Cryptography (Harvard, 2016):**

![Cryptography pipeline](https://raw.githubusercontent.com/riacheruvu/neural-cryptography/refs/heads/master/Crypto-Pipeline.png)
- **Project Summary:**
Applying Convolutional Neural Networks to Cryptography (2016). As part of this project, I have successfully demonstrated an efficient neural cryptography algorithm (NCA) through tuning and implemented NCA through socket communication. I have also developed a signature/verification capability calculated based on the hidden states of the neural networks, which has not been accomplished before at the time of the project. The adopted and modified code presented in this project serves as a foundation to further research and improvements to the neural cryptography work.
