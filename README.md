Collaborators: [Kasra Khoshjoo](mailto:kasrakhoshjoo@gmail.com), [Reza Pishkoo](mailto:pishkoo.reza2001@gmail.com)
# Profit Maximization on The Lightning Network

This repository contains summaries and presentations of the project "Profit Maximization on Lightning Network", in which we investigate strategies for maximizing revenue from transaction fees.

## Introduction
In the Lightning Network, transactions pass through channels to avoid the difficulties of making transactions in the Bitcoin Blockchain. Each owner of payment channels sets a fee rate on transactions passing through. The question is, 
who should connect with, and how much capacity should be allocated to each channel to become a transaction hub and maximize the profit of receiving fees? We investigate this problem from the perspective of bandit problems and available
theoretical tools in the literature[^fn1]. (slides of a presentation summarizing our progress until Nov 2023 are available [here](presentations/november_report.pdf))

## Summary of our findings (non-chronological order)
### Maximum Betweenness Improvement
The MBI problem is defined on graphs. The maximum number of pairs of nodes, the shortest path between which passes through us 
by choosing at most $k$ nodes as our neighbors. This is a reasonable simplification of our setting. If we assume that
all channel owners set the same fee rates, transactions pass through the shortest path between the two members, as this path
requires the minimum fee to be paid. It has been proven that no polynomial-time algorithm can be proven to
reach a solution as least as good as $1 - \frac{1}{2e}$ relative to the optimal solution.[^fn3] As far as we can tell, only algorithm with a guaranteed level of optimality 
can reach $\Omega (\frac{1}{\sqrt{n}})$.no algorithm is proven to guarantee any constant
level of optimality compared to the optimal solution. The greedy algorithm is the standard solution of choice, which has been
proven to be arbitrarily far from optimal.[^fn3] We have proved that we can reduce instances of MBI with weighted importance in two cases can be reduced to a normal MBI.
Firstly, the case in which a parameter vector $\mu$ determines the likelihood of transactions taking place and each pair $(u,v)$ is expected
to pay us $\mu_u\mu_v$ as transaction fee given that our node lies in their shortest path. Secondly, we proved that
if channel capacities determine the maximum possible amount transferred between nodes and this determines the importance of each pair in
calculating our "Betweenness", we can reduce this problem to a normal MBI. Formal theorems regarding these results are stated [here](theorems.pdf).

### Spectral Bandits
In Spectral Bandit problems, arms that could be chosen are nodes of a given graph, and the reward for two nodes is guaranteed to be 
close if they are connected by an edge. We find this problem related to our setting because we are choosing a subset of nodes
on a graph, and subsets with "close" elements are expected to result in the same expected reward value (earning fee). we intend to use Spectral-UCB[^fn2]
in our setting by defining a graph based on the network's topology in which nodes represent subsets of network members. The challenge is to 
develop a useful construction algorithm or definition for the hypothetical subset graph, applying spectral-UCB
to our setting and proving regret bounds given the bounds of the vanila algorithm. We ran simulations to compare spectral-UCB on 
a hyper-cube and a greedy algorithm for combinatorial settings that showed spectral-UCB does not resemble a greedy algorithm that
adds nodes one at a time, which was our first speculation.

### Combinatorial Bandits
In Combinatorial Bandits, our choices are subsets of the set of arms (or super arms). Our problem is 
a combinatorial bandit. However, the MBI problem and our setting generally do not enjoy assumptions such as 
monotonicity, which would lead to using a greedy algorithm with guaranteed optimality.[^fn6] We speculate that
some properties of the Lightning Network might help us reach a working solution using algorithms for combinatorial bandits
with assumptions applicable to our setting. One prospective approach is using an algorithm that relies on a 
$(\alpha, \beta)$-approximator ( $\alpha$ and $\beta$ are minimum guarantees for approximation) to 
simulatiously approximate an unknown distribution parameter vector $\mu$ and the best super arm for the problem.[^fn7] Finding proper assumptions and an algorithm as our "Oracle" approximator that has a guaranteed accuracy under the introduced assumptions is one of the possible approaches we are working on. Reza Pishkoo also gave a presentation in an event we called "The Lightning Day" to describe our findings and the algorithm in the paper. Slides of this presentation are available [here](presentations/alpha-beta_approximation.pdf).

### Capacity Allocation

One issue that we ignored for simplification is the effect of fee rates on the paths chosen for transactions.
We addressed this problem in a simple form. Given that our neighbors are fixed, we studied how we can distribute a fixed amount of currency in our channels to maximize our revenue. By adding assumptions such as sub-gaussian distribution of transaction amounts we proved that this problem is an instance of Convex Optimization with Bandit Feedback, a solution to which is given with desirable regret bounds.[^fn8] Kasra Khoshjoo gave a presentation in the aforementioned event discussing the capacity allocation problem and convex optimization with bandit feedback. Slides of the presentation are available [here](presentations/convex_optimization.pdf).






[^fn1]: how to profit from payment channels, https://arxiv.org/abs/1911.08803
[^fn2]: Spectral Bandits, https://jmlr.org/papers/v21/16-529.html
[^fn3]: On The Imporovement of Betweenness Problem, https://www.sciencedirect.com/science/article/pii/S1571066116300196
[^fn6]: The greedy algorithm for monotone submodular maximization, https://homes.cs.washington.edu/~marcotcr/blog/greedy-submodular/
[^fn7]: Combinatorial Multi-Armed Bandit: General Framework and Applications
, https://proceedings.mlr.press/v28/chen13a.html
[^fn8]: Stochastic convex optimization with bandit feedback, https://proceedings.neurips.cc/paper_files/paper/2011/hash/67e103b0761e60683e83c559be18d40c-Abstract.html
