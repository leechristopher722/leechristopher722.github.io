---
title: 'PacmanAI'
date: 2022-05-01
draft: false
description: 'PacmanAI Project Page'
slug: 'pacmanAI'
---

A series of AI engines for the PacMan game, utilizing multiagent search algorithms, reinforcement learning, and more.

This project consists of 4 parts:

- **Single/Multiagent Search**
- **Logic**
- **Bayes Nets and HMMs**
- **Reinforcement Learning**

## Single/Multiagent Search

Depth-first, breadth-first, uniform cost, and A\* search. Minimax and expectimax algorithms along with designing evaluation functions for multi-agent search.

- **Conditions & Algorithms Used**:
  1. Locating Fixed Food: Depth First, Breath First, A\*, Suboptimal Search

## Logic

Applying Propositional Logic in a pacman world represented with booleans to solve planning tasks as well as localization, mapping, and SLAM.

**Goals:**

1. Create logical expressions that represent Pacman physics and locate Pacman agent's position given its actions and sensor readings.
2. Plan sequence of actions to the goal position.
3. Plan sequence of actions to eat all the food on the board.
4. Locate Pacman agent's position at each timestep with **known map**, **unknown starting position**, and sensors.
5. Map the entire board with **known starting location** and sensors.
6. Locate Pacman agent's position and Map the entire board with **unknown map**, **unknown starting position**, and sensors (Simultaneous Localization and Mapping).

**Results:**

1.
2.
3.
4.
5.
6.

## Bayes Nets and HMMs

Bayes Nets and the forward algorithm, employing particle sampling in Hidden Markov Models to locate ghosts based on noisy distance readings.

**Goal:** Locate and eat invisible ghosts with sensors that provide nosiy readings of the Manhattan distance.

**Result:**

## Reinforcement Learning

Value Function, Q learning, and Approximate Q learning to teach Pacman and crawler agents rational policies.

## What I've Learned

1. **Algorithm Implementation**: Through Projects 1 and 2, I gained proficiency in implementing search algorithms and understanding their effectiveness in solving navigation and adversarial problems. This involved grasping concepts like depth-first, breadth-first, uniform cost, A\* search, multiagent minimax, and expectimax algorithms.

2. **Reinforcement Learning**: Project 3 introduced me to reinforcement learning techniques such as Value Function, Q learning, and Approximate Q learning. By implementing these algorithms, I learned how agents can learn optimal policies through trial and error, improving decision-making in dynamic environments.

3. **Probabilistic Inference**: Project 4 deepened my understanding of probabilistic reasoning by employing Bayes Nets and Hidden Markov Models (HMMs). I learned how to use inference algorithms like the forward algorithm and particle sampling to make probabilistic predictions, crucial for tasks like ghost tracking in Pacman.

4. **Problem Solving and Critical Thinking**: Across all projects, I honed my problem-solving skills and developed a critical mindset towards algorithm selection and implementation. I learned to evaluate the trade-offs between different approaches and make informed decisions based on the specific requirements of each problem.

Overall, working on these projects equipped me with practical skills in AI and machine learning, fostering a deeper understanding of their applications in gaming and beyond. Additionally, it instilled in me a sense of confidence in tackling complex problems and leveraging advanced techniques to find effective solutions.

**Disclaimer**

This project was developed for UC Berkeley's CS188: Introduction to Artificial Intelligence Course.
Source code can be provided on
<a class="button primary big" href="mailto:leechristopher722@gmail.com" target="_blank" >request</a>.
