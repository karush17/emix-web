---
layout: default
---

<span style="color:grey">8 minute read</span>
<h1>Nergy-based Surprise Minimization for Multi-Agent Value Facotrization</h1>

<p align="center"><img src="/images/2s_vs_1sc.gif" height="200" width="400" />   <img src="/images/so_many_baneling.gif" height="200" width="400" /></p>  

<h3>Multi-Agent Value Factorization</h3>  
<p>Reinforcement Learning (RL) has seen tremendous growth in applications such as arcade games, board games, robot
control tasks and lately, real-time games such as StarCraft II. The rise of RL has led to an increasing interest in the study of multi-agent systems, commonly known as Multi-Agent Reinforcement Learning (MARL). In the case of partially observable settings, MARL enables the learning of policies with centralised training and decentralised control. This has proven to be useful for exploiting value-based methods which are often found to be sample-inefficient.  

Value Factorization is a common technique which enables the joint value function to be represented as a combination of individual value functions conditioned on states and actions. In the case of Value Decomposition Network (VDN), a linear additive factorization is carried out whereas QMIX generalizes the factorization to a non-linear combination, hence improving the expressive power of centralised action-value functions. Furthermore, monotonicity constraints in QMIX enable scalability in the number of agents. On the other hand, factorization across multiple value functions leads to the aggregation of approximation biases originating from overoptimistic estimations in action values which remain an open problem in the case of multi-agent settings. Moreover, value factorization methods are conditioned on states and do not account for spurious changes in partially-observed observations, commonly referred to as surprise.</p>  

<h3>Surprise, Surprise ...</h3>  
<p>Surprise minimization is a recent phenomenon observed in the case of single-agent RL methods which deals with environments consisting of spurious states. In the case of model-based RL, surprise minimization is used as an effective planning tool in the agent’s model whereas in the case of model-free RL, surprise minimization is witnessed as an intrinsic motivation or generalization problem. On the other hand, MARL does not account for surprise across agents as a result of which agents remain unaware of drastic changes in the environment. Thus, surprise minimization in the case of multi-agent settings requires attention from a critical standpoint.</p>  

<h3>The Energy-based MIXer (EMIX)</h3>  
<p>We have introduced the Energy-based MIXer (EMIX), an algorithm based on QMIX which minimizes surprise utilizing the energy across agents. EMIX makes use of a novel surprise minimization technique consisting of a surprise value function in conjunction with an energy operator to minimize surprise across multiple agents in the case of multi-agent partially-observable settings. Additionally, EMIX presents a novel scheme consisting of multiple target function approximators for addressing overestimation bias across agents in MARL which, unlike previous single-agent methods, do not rely on a computationally-expensive family of action value functions. When evaluated on a range of challenging StarCraft II scenarios in SMAC, EMIX demonstrates state-of-the-art performance for multiagent surprise minimization by significantly improving the consistent performance of QMIX.</p>    

<p align="center"><img src="/images/emix.gif" height="500" width="650" /></p>  


<h3>The EMIX objective</h3>  
<p>EBMs have been successfully implemented in single-agent RL methods. These typically make use of Boltzmann distributions to approximate policies. Such a formulation results in the minimization of free energy within the agent. While policy approximation depicts promise in the case of unknown dynamics, inference methods play a key role in optimizing goal-oriented behavior.In the case of MARL, EBMs have witnessed limited applicability as a result of the increasing number of agents and complexity within each agent. While the probabilistic framework is readily transferable to opponent-aware multi-agent systems, cooperative settings consisting of coordination between agents require a firm formulation of energy which is scalable in the number of agents and accounts for environments consisting of spurious states.  

Our choice of the energy operator is based on its unique mathematical properties which result in better convergence.The energy-based surprise minimization objective can be formulated by simply adding the approximated energy-based surprise to the initial Bellman objective as expressed below</p>    

<p align="center"><img src="/images/obj.PNG" height="100" width="700" /></p>  

<h3>Performance on StarCraft II micromanagement</h3>  
<p>We assess the performance and sample-efficiency of EMIX on multiagent StarCraft II micromanagement scenarios. We select StarCraft II scenarios particularly for three reasons. Firstly, micromanagement scenarios consist of a larger number of agents with different action spaces. This requires a greater deal of coordination in comparison to other benchmarks which attend to other aspects of MARL performance such as opponent-awareness. Secondly, micromanagement scenarios consist of partial observability wherein agents are restricted from responding to enemy fire and attacking enemies when they are in range. This allows agents to explore the environment effectively and find an optimal strategy purely based on collaboration rather than built-in game utilities. Lastly, micromanagement scenarios in StarCraft II consist of multiple opponents which introduce a greater degree of surprise within consecutive states. Irrespective of the time evolution of an episode, environment dynamics of each scenario change rapidly as the agents need to respond to enemy’s behavior.</p>  

<p>We compare our method to current state-of-the-art methods, namely QMIX [41], VDN [51], COMA [8] and IQL [53]. We assess the performance and sample-efficiency of agents by evaluating the success rate of the multi-agent system in completing each scenario. A completing of a scenario indicates the victory of the team over its enemies. Figure below presents the results of our experiments. Out of the 12 scenarios considered, EMIX presents higher success rates on 9 of these scenarios depicting the suitability of the proposed approach. In scenarios such as 3m, 3s5z and 8m performance gain between EMIX and other methods such as QMIX and VDN are incremental as a result of the small number of agents and simplicity of tasks. On the other hand, EMIX presents significant performance gains in cases of so_many_baneling and 5m_vs _6m which consist of a large number of opponents and a difficult map location respectively.</p>

<p align="center"><img src="/images/rewards.png" height="800" width="800" /></p>  

When compared to QMIX, EMIX depicts improved success rates on all of the 12 scenarios. For instance, in scenarios such as 3s_vs_5z, 8m_vs_9m and 5m_vs_6m QMIX presents sub-optimal performance. On the other hand, EMIX utilizes a comparatively improved policy and yields better convergence in a sample-efficient manner. Thus, EMIX augments the performance and sample-efficiency of the QMIX agent utilizing the energy-based surprise minimization scheme.

<h3>Surprise Minimization Across Agents</h3>  

<p>The importance of 𝛽 can be validated by assessing its usage in surprise minimization. However, it is difficult evaluate surprise minimization directly as surprise value function estimates vary from state-to-state across different agents and thus, they present high variance during agent’s learning. This, in turn poses hindrance to gain an intuitive understanding of the surprise distribution. We instead observe the variation of 𝐸 as it is a collection of surprise-based sample estimates across the batch. Additionally, 𝐸 consists of prior samples  which makes inference across different agents tractable. Figure below presents the variation of Energy ratio 𝐸 with the temperature parameter 𝛽 during learning.We compare two stable variations of E at 𝛽 = 0.001 and 𝛽 = 0.01. The objective minimizes 𝐸 over the course of learning and attain thermal equilibrium with minimum energy. Intuitively, equilibrium corresponds to convergence to optimal policy 𝜋∗ which validates the claim in Theorem 2. With 𝛽 = 0.01, EMIX presents improved convergence and surprise minimization for 5 out of the 6 considered scenarios, hence validating the suitable choice of 𝛽. On the other hand, a lower value of 𝛽 = 0.001 does little to minimize surprise across agents. In the case of high 𝛽 values, EMIX demonstrates unstable behavior as a result of increasing overestimation error. Thus, a suitable value of 𝛽 is critical for optimal performance and surprise-robust behavior.</p>

<p align="center"><img src="/images/surprise.png" height="150" width="1600" /></p>  

<h3>The Way Ahead</h3>  

<p>EMIX proposes a novel energy-based surprise minimization objective consisting of an energy operator in conjunction with the surprise value function across multiple agents in the case of multi-agent partially-observable settings. The EMIX objective satisfies theoretical guarantees of total energy and surprise minimization with experimental results validating these claims. Additionally, EMIX presents a novel technique for addressing overestimation bias across agents in MARL based on multiple target value approximators. While EMIX serves as the first practical example (to our knowledge) of energy-based models in cooperative MARL, we aim to extend the energy framework to opponent-aware and hierarchical MARL. We leave this as our future work.</p>

