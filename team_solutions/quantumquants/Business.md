# Solution Part III : Business

## Task
*   Determine an application for MIS in the areas of risk analysis, and finance.
    + bonus points if your application requires graphs fitting exactly the constraint of the Rb/a=3 type
*   Estimate how many vertices would be necessary for real-life problems. 
    + Bonus points for finding applications that require order near-term numbers of qubits, order few hundreds.
 
## Solution
### Part I : Motivation
<img src=/figs/solved_hugenet.png align="right" width=500>

The image on the right hand side portrays a 30x30 lattice with 100 random dropout points and a connectivity at $R_b=4a$. Needless to say this exceeds the quantum capabilities we have developed this Hackathon by an order of magnitude in every regard. However, it took one teammate 5 minutes to code up a classical solver to find an MIS for this graph. It ran unoptimized on his 6 year old MacBook with what can only be estimated to range from 7 to 42 active spam processes in the background for 52 seconds before finding an optimal solution. We say this to curb our expectations: At the current scale it is unlikely that the neutral atoms analogue computing approach presented in this challenge will outperform a classical solution. Instead of implementing a far fetched 4x4 toy problem (or be the hundredth team to connect correlated stocks into a graph and perform miniscule scale portfolio optimization) we would like to seize this opportunity to think outside the box and consider what this method can provide once it scales up to a size that may not be too far out of grasp. 

### Part II : Topological Data Analysis

We believe Topological Data Analysis (TDA) to be a strong contender for major use cases in the coming era of quantum utility. In particular we propose a concrete quantum pipeline for the calculation of Betti Numbers in financial data for crisis prediction and prevention.

<img src=/figs/TDA_scheme.png align="left" width=500>

The fundamental idea of TDA is to extract inherently noise-resiliant topological properties from large amounts of noisy data. This is ideal for applications in finance, where the data at our disposal is both vast and noisy. TDA allows us to cut through the chaff and observe large-scale patterns that remain otherwise invisible. The preparation of TDA is outlined in the figure to the left taken from an MIT/ETH [paper](https://arxiv.org/abs/2209.14286) on the subject. TDA usually starts with data provided in the form of a point cloud $\lbrace x_i\rbrace_{i=1}^N\subset X$ in some manifold $X$ with a metric $d$ (often $\mathbb R^n$ with the euclidean distance). Fixing a resolution $\epsilon$ we connect the cloud into a graph by drawing a vertex $(x_i,x_j)\iff d(x_i,x_j)<\epsilon$. From here we upgrade the graph to a simplicial clique complex by finding all simplexes in the graph. Here TDA starts, analyzing the topology of the resulting object. 

We are motivated by a multitude of recent findings observing topological changes in financial data in the advent of financial crises. An example are the peaks in Banach representation in stock market data occuring before the crashes of 2000 and 2008 [(Gidea & Katz, 2017)](https://arxiv.org/pdf/1703.04385). We want to focus on the calculation of Betti Numbers, the ranks of the Homology groups of the defined topological space, a [#P hard problem](https://www.sciencedirect.com/science/article/pii/S0925772116300463). These too [have been shown](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8467365/pdf/entropy-23-01211.pdf) to display predictable anomalies in the prelude to financial crises. This is a relatively recent trend in quantitative research and much remains to be discovered as these patterns are notoriously elusive, however we want to consider a quantum pipeline involving MIS finding in particular, can speed up their discovery. 

### Part III : Quantum TDA & Even Quantummer TDA

TDA has been on the radar for quantum supremacy since at the latest 2016, when Lloyd et al. published a [quantum algorithm](https://www.nature.com/articles/ncomms10138) yielding a [quadratic](https://arxiv.org/abs/2209.14286) to [exponential](https://www.nature.com/articles/ncomms10138) speedup over known classical algorithms.To calculate the $k$-th Betti Number $\beta_k$ its input it requires the simplex state $\ket{\psi_k}=\frac 1 {|S_k|}\sum_{s\in S_k}\ket s$ for $S_k$ the set of k simplexes on S and $\ket{s}\in(\mathbb C^{2})^{\otimes n}$ the computational basis state vector indicating 1s at the positions of vertices belonging to the simplex. Overall, this provides us with a clear and existing (and expensive) pipeline to estimate Betti Numbers and look for patterns in financial data: We receive a point cloud, transform it into a graph and then simplicial clique complex, find all simplices on it  and run this information through the LGZ algorithm. 

We are now ready to connect this theoretical background with MIR finding. The one remaining [NP-hard problem](https://www.geeksforgeeks.org/proof-that-clique-decision-problem-is-np-complete/) in the pipeline that we need to solve classically as a prerequisite for LGZ and thus Betti Number calculation is the maximum clique problem for the simplicial clique complex. However this is equivalent to MIS finding, since for any given graph $G$ a k-Clique is simultaneously a k-independent set of the complementary Graph $\bar G$ consisting of the same nodes and complementary edges. It is here that we can employ a neutral atoms analogue approach.

### Part IV: Empirical Dataset and Scale. 

#### 1. Problem. TDA to predict the evolution of topological features in stock market data.
Previous work: As proposed by (arXiv:1703.04385 [q-fin.MF]), we can use a sliding window to enclose time-dependent point cloud data sets in the $d$-dimensional space determined by the muti-dimensional time series data of the stock market, and associate them to a topological space for an early warning of market crashes. The idea is to use ``persitency homology'' to detect and quantify topological patterns in this data. The authors proposed to employ the $L^p$-norm of persistence landscapes to query the stability in topological features as a predictor for market crashes. They interpret the persistence landscape as follows:
+ horizontal axis: birth indices
+ vertical axis: death indices
Used is the Wasserstein distance, with $p \geq 1$ as metric
TDA is applied on top of the time-ordered sequence of point clouds to study the time-varying topological properties of the multidimensional time series, from window to window. For each point cloud we compute the persistence diagram of the Rips filtration, the corresponding persistence landscape, and its $L_p$-norms.

In our approach, instead we look at the Betti numbers as an output of the LGZ algorithm, these powerful numbers can be compared for different trajectories of the stock market, given these statistics we can find indicators of early warning of market crashes.

#### 2. Dataset. Empirical analysis of financial data.
We take a multi-dimensional time series composed by time series data of multiple stocks or stock market indices. For each stock/index, take the log-returns of daily changes  of adjusted closing value of stock/index $j$ at day $i$ defined as 
$l_j^{(i)} = \ln\frac{P_j^{(i)}}{P_j^{(i-1)}}$. The figure bellow (arXiv:1703.04385 [q-fin.MF]) shows an example of 2D point clouds as scattered plots of time series for the S&P500 and NASDAQ within 50 and 100 days. The left column illustrates the appearance of loops (1D homology) at certain radii in the 2D point cloud.



#### 3. Scale. 
Each point cloud is formed by $c$ points in $\mathbb{R}^d$. Each represented by a $c \times d$ matrix, with $d$ columns, the number of stocks, and $c$ rows, the trading days. 

Recalling the correctness of reduction: $G$ has an independent set of size k if and only if $\bar{G}$ has a clique of size $k$. From the argument above, the maximum clique problem for the simplicial clique complex is equivalent to MIS finding, since for any given graph $G$ a k-Clique is simultaneously a k-independent set of the complementary Graph $\bar G$ consisting of the same nodes and complementary edges. Therefore MIS finding on this point cloud requires $c \times d$ vertices. 

The number of vertices required for real-life problems will be dependent on the time resolution of interest for the time horizon. The cited paper analyzes intervals of 50-100 days, and meaninful statistics can be extracted from several years of data.

<img width="689" alt="Screenshot 2024-05-05 at 05 04 17" src="https://github.com/Jurglex/QuantumQuants/assets/156319597/9feef035-c03e-4474-bd4d-42d6f751bea8">
