---
title: 'PacmanAI'
date: 2022-05-01
draft: false
description: 'AI agents for Pac-Man using search algorithms, reinforcement learning, and probabilistic inference'
slug: 'pacmanAI'
---

A comprehensive suite of AI agents for the Pac-Man game, implementing fundamental artificial intelligence techniques from graph search to reinforcement learning. Built as part of UC Berkeley's CS188 Introduction to Artificial Intelligence course, this project demonstrates practical applications of AI algorithms in a challenging game environment.

## The Problem

Real-world AI problems require agents that can navigate complex environments, make decisions under uncertainty, and learn from experience. While Pac-Man may seem like just a game, it presents all these challenges: path planning through mazes, adversarial ghost agents with stochastic behavior, partial observability with invisible ghosts, and the need to learn optimal strategies through trial and error.

## Key Features

**🔍 Intelligent Path Finding**  
Implemented four fundamental search algorithms (DFS, BFS, UCS, A*) to navigate mazes efficiently. Developed custom heuristics for A* that reduce node expansions by up to 80% while maintaining optimality. Solved complex problems like finding optimal paths to eat all food pellets using advanced state space representations.

**🎮 Multi-Agent Game Playing**  
Built adversarial search agents using Minimax and Expectimax algorithms to play against multiple ghosts. Designed sophisticated evaluation functions that balance food collection, ghost avoidance, and winning strategies. Handles both deterministic and probabilistic ghost behaviors.

**🧠 Reinforcement Learning**  
Implemented model-based (Value Iteration) and model-free (Q-Learning, Approximate Q-Learning) algorithms. Trained agents that learn optimal policies through experience, starting from zero knowledge about the game. Applied function approximation to handle large state spaces efficiently.

**👻 Probabilistic Tracking**  
Created ghost-hunting agents that track invisible ghosts using noisy distance sensors. Implemented Bayes Nets for probabilistic inference and particle filters for Hidden Markov Models. Achieved accurate ghost localization even with Manhattan distance readings corrupted by noise.

**🗺️ Logic-Based Planning**  
Applied propositional logic to solve SLAM (Simultaneous Localization and Mapping) problems. Built agents that can locate themselves with unknown starting positions, map unknown environments, and plan action sequences using logical inference.

## Implementation Details

**Search Algorithms**  
The search module implements graph-based versions of all algorithms to avoid revisiting states. Used Python's data structures strategically—`Stack` for DFS, `Queue` for BFS, and `PriorityQueue` for UCS and A*. Custom heuristics for A* use Manhattan distance for simple cases and actual maze distances for complex multi-goal problems.

**Game Playing Agents**  
Minimax with alpha-beta pruning handles deterministic ghosts, searching up to depth 4 in reasonable time. Expectimax models probabilistic ghost behavior, computing expected utilities across all possible ghost actions. Evaluation functions combine weighted features: food proximity, ghost distances, power pellet opportunities, and winning conditions.

**Reinforcement Learning**  
Value Iteration computes optimal policies for known MDPs using dynamic programming. Q-Learning agents explore using epsilon-greedy strategies, learning state-action values through experience. Approximate Q-Learning uses feature extraction to generalize across similar states, enabling learning in large state spaces.

**Probabilistic Inference**  
Exact inference using the Forward Algorithm for HMMs when tracking single ghosts. Particle filtering approximates joint distributions when tracking multiple ghosts simultaneously. Dynamic particle count adjustment based on uncertainty levels improves both accuracy and performance.

## Key Algorithms

**Search & Planning:**

- DFS/BFS – Complete maze exploration and shortest path finding
- UCS – Optimal paths with non-uniform costs
- A\* – Optimal paths with admissible heuristics reducing search space

**Adversarial Search:**

- Minimax – Perfect play against optimal opponents
- Alpha-Beta Pruning – Efficient minimax through branch elimination
- Expectimax – Rational decisions against stochastic opponents

**Learning & Inference:**

- Value/Policy Iteration – Computing optimal policies for MDPs
- Q-Learning – Model-free reinforcement learning
- Particle Filtering – Approximate inference in continuous spaces

## Challenges & Solutions

**Challenge**: Designing admissible heuristics for multi-goal search problems (eating all food)  
**Solution**: Used minimum spanning tree of remaining food as heuristic, ensuring admissibility while providing tight bounds that dramatically reduce search space

**Challenge**: Balancing exploration vs exploitation in Q-Learning  
**Solution**: Implemented adaptive epsilon-greedy strategy that decreases exploration over time, transitioning from learning to optimal play

**Challenge**: Tracking multiple invisible ghosts with noisy sensors  
**Solution**: Particle filter with intelligent resampling—concentrates particles in high-probability regions while maintaining diversity to avoid particle depletion

---

**Note**: Source code available upon request as per UC Berkeley's academic integrity policies.  
<a class="button primary big" href="mailto:leechristopher722@gmail.com" target="_blank">Request Source Code</a>
