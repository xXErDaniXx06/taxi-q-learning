# Autonomous Taxi Agent with Q-Learning 🚕🤖

An implementation of a Reinforcement Learning agent trained to solve the Taxi-v3 environment from Gymnasium using a tabular Q-Learning approach. 

## 📌 Project Overview
This project demonstrates the "Sense-Think-Act" cycle of an autonomous agent navigating a stochastic grid world. The agent learns to pick up a passenger from one of four locations and drop them off at a designated destination in the shortest time possible, completely from scratch.

## 🧠 State and Action Space
* **States (500):** A combination of 25 taxi positions, 5 passenger locations, and 4 destinations.
* **Actions (6):** Move South, North, East, West, Pickup passenger, Dropoff passenger.

## ⚙️ Mathematical Approach
The agent's "brain" is a Q-Table updated using the **Bellman Equation** over thousands of episodes:

$$Q(s, a) \leftarrow Q(s, a) + \alpha \left[ r + \gamma \max_{a'} Q(s', a') - Q(s, a) \right]$$

Where:
* $\alpha$ (Learning Rate): [We will put our value here]
* $\gamma$ (Discount Factor): [We will put our value here]
* Epsilon-Greedy strategy used for the Exploration vs. Exploitation trade-off.

## 🚀 Installation & Usage
1. Clone the repository:
   ```bash
   git clone [https://github.com/tu-usuario/taxi-q-learning.git](https://github.com/tu-usuario/taxi-q-learning.git)