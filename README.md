# Profit-Maximization-Lightning-Networks

This repository contains summaries and presentations of the project "Profit Maximization on Lightning Network", in which we investigate strategies for maximizing revenue from transaction fees.

## Introduction
In the Lightning Network, transactions pass through channels to avoid the difficulties of making transactions in the Bitcoin Blockchain. Each owner of payment channels sets a fee rate on transactions passing through. The question is, 
who to connect with and how much capacity should be allocated to each channel in order to become a transaction hub and maximize the profit of receiving fees. We investigate this problem from the perspective of bandit problems and available
theoretical tools in the literature[^fn1]. (slides of a presentation summarizing our progress until Nov 2023 are available [here](presentations/november_report.pdf))

## Summary of our findings (non-chronological order)
### Maximum Betweenness Improvement
The MBI problem is defined on graphs. the maximum number of pairs of nodes the shortest path between which pass through us 
by choosing at most $k$ nodes as our neighbours. This is a good simplification of our setting. If we assume that
all channel owners set the same fee rates, transactions pass through the shortest path between the two members as this path
requires minimum amount of fee to be payed. It has been proven that no polynomial-time algorithm can be proven to
reach a solution as least as good as $1 - \frac{1}{2e}$ relative to the optimal solution.[^fn3] As far as we can tell, only algorithm with a garanteed level of optimality 
can reach $\Omega (\frac{1}{\sqrt{n}})$.no algorithm is proven to garantee any constant
level of optimality compared to the optimial solution. The greedy algorithm is the common solution of choice which is
proven to be arbitrarily far from optimal.[^fn4] We have proved that we can reduce instances of MBI with weighted importance in two cases can be reduced to a normal MBI.
Firstly, the case which a paramter vector $\mu$ determines the likelihood of transactions taking place and each pair $(u,v)$ is expected
to pay us $\mu_u\mu_v$ as transaction fee given that our node lies in their shortest path. Secondly, we proved that
if channel capacities determine the maximum possible amount transfered between nodes and this determines the importance of each pair in
calculating our "Betweenness", we can reduce this problem to a normal MBI.[^fn5]

### Spectral Bandits
In Spectral Bandit problems, arms that could be chosen are nodes of a given graph and reward for two nodes is garanteed to be 
close if they are connected by an edge. We find this problem related to our setting because we are chosing a subset of nodes
on a graph and subsets with "close" elements are expected to result in same expected value of reward (earning fee). our intention is to use Spectral-UCB[^fn2]
in our setting by defining a graph based on the topology of the network in which nodes represent subsets of members of the network. The challenge is to 
come up with a useful contruction algorithm or definition for the hypothetical subset-graph, applying spectral-UCB
to our setting and proving regret bounds given the bounds of the vanila algorithm. We ran simulations to compare spectral-UCB on 
a hyper-cube and a greedy algorithm for combinatorial settings that showed spectral-UCB does not resemble a greedy algorithm that
adds nodes one-at-a-time, which was our first speculation.

### Combinatorial Bandits
In Combinatorial Bandits, our choices are subsets of the set of arms (or super arms). Our problem is clearly
a combinatorial bandit. However, the MBI problem and our setting in general, does not enjoy assumptions such as 
monotonicity which would lead to using a greedy algorithm with garanteed optimality.[^fn6] We speculate that
some properties of the Lightning Network might help us to reach a working solution using algorithms for combinatorial bandits
that have assumptions applicable to our setting.





[^fn1]: how to profit from payment channels, https://arxiv.org/abs/1911.08803
[^fn2]: Spectral Bandits, https://jmlr.org/papers/v21/16-529.html
