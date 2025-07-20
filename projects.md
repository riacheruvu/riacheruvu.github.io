---
title: Tech Projects
layout: default
---
# A selection of technical engineering projects

## Reinforcement Learning and Intuitive Physics

Artificial intuitive physics explores computational models that simulate human-like physical reasoning. This project aimed to reverse-engineer "intuitive theories" of physics and implement them within AI algorithms. My work during a research collaboration with Intel Labs focused on building intuitive physics engines in simulated environments using reinforcement learning, enabling agents to interact with their surroundings in ways transferrable to real-world robotics. The research leveraged Pybullet and OpenAI Gym for physics simulation, along with other libraries, and included the implementation of reinforcement learning algorithms from scratch—such as Proximal Policy Optimization and World Models.

Three deliverables resulted from this project:

1. I authored a report titled Intuitive Physics and Simulation Tools for Active Agents (2020 draft) surveys AI-driven physical reasoning and simulation methods (e.g., FEM, SPH, IPEs) for robotics and scene understanding. Paused due to project reprioritization, with some incomplete sections (e.g., causality hardware), it showcases my interdisciplinary synthesis skills, informing my ongoing AI-driven simulation research. [Link](https://github.com/riacheruvu/riacheruvu.github.io/blob/main/2020%20Rough%20Draft%20Report%20-%20Simulation%20Tools%20and%20Intuitive%20Physics.pdf) 

2.	Developed ML engines that can perform “common sense reasoning” inspired by human reasoning at the intersection of computer vision, reinforcement learning (RL), computational neuroscience, and cognitive development. Implemented intuitive physics engines in a simulated environment using RL, enabling agents to interact with their surroundings applied to a real-life robot. Leveraged Pybullet, OpenAI Gym, and other libraries for physics simulation and different RL algorithms from scratch, such as Proximal Policy Optimization and World Models.
![Snapshot of RL experiments](https://raw.githubusercontent.com/riacheruvu/riacheruvu.github.io/refs/heads/main/rl_image.png)

3. I conducted research on modeling uncertainty in intuitive physics environments for active agents, exploring Bayesian neural networks and their alignment with hardware-level Instruction Set Architecture (ISA) customization. This work led to the identification of design pathways for AI models that effectively integrate physics reasoning with low-level architectural support.

# Featured Academic Projects

**Bayesian Deep Learning:**
![Snapshot of aleatoric uncertainty for a computer vision dataset](https://raw.githubusercontent.com/riacheruvu/riacheruvu.github.io/refs/heads/main/aleatoric_uncertainty.png)
- **Project Summary:**
Research on epistemic and aleatoric uncertainties for Bayesian Deep Learning (2019). For this project, we extend the research by Alex Kendall and Yarin Gal from the paper "What Uncertainties Do We Need in Bayesian Deep Learning for Computer Vision?" We provide a synthesis of this paper and evaluate the claims made by the authors. We implemented pedagogical examples of neural networks with and without epistemic and aleatoric uncertainty and evaluated these models on toy regression and classification tasks, in addition to the MNIST and CIFAR-10 datasets. We also implement experiments such as the effect of model complexity on the performance of a model incorporating epistemic uncertainty, and the effect of data transformations on epistemic and aleatoric uncertainty.

[Colab notebook](https://colab.research.google.com/github/onefishy/am207_fall19_projects/blob/master/what_uncertainties/what_uncertainties_3/cheruvuria_136145_9127626_Final_Project_Submission.ipynb)

**Automated Data Engineering Workflow:**
- **Project Summary:**
Automated Data Engineering Using Dask and Luigi (2019). This project implements an automated data engineering pipeline - Given large input dataset CSV files and user-defined preferences, the data is cleaned, data types are automatically determined, and the workflow outputs a visualization HTML report (which is atomically written to the user's local file system) for the categorical and numerical variables in the dataset.

**Healthcare Streaming Data Visualization Pipeline:**
![Streaming Data Pipeline](https://raw.githubusercontent.com/riacheruvu/riacheruvu.github.io/refs/heads/main/project_streaming_data.png)
- **Project Summary:**
Healthcare Streaming Data Visualization Pipeline (2019). Can we predict and visualize the percent of adults who have an overweight classification in a particular region in the US based on factors such as income, gender, or race? The goal of this project was to simulate a real-life streaming application using Kibana, ElasticSearch, Kafka, and Spark for a potential use case where health care service providers and insurance organizations are consistently providing data and querying a system on the patterns in the obesity epidemic across the United States.

**Neural Cryptography:**
![Cryptography pipeline](https://raw.githubusercontent.com/riacheruvu/neural-cryptography/refs/heads/master/Crypto-Pipeline.png)
- **Project Summary:**
Applying Convolutional Neural Networks to Cryptography (2016). As part of this project, I have successfully demonstrated an efficient neural cryptography algorithm (NCA) through tuning and implemented NCA through socket communication. I have also developed a signature/verification capability calculated based on the hidden states of the neural networks, which has not been accomplished before at the time of the project. The adopted and modified code presented in this project serves as a foundation to further research and improvements to the neural cryptography work.