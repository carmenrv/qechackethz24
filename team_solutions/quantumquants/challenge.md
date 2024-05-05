# Challenge Statement: MIS in complex graphs and applications in finance
### (Based on https://arxiv.org/pdf/2307.09442.pdf)

Neutral-atom quantum hardware is well-adapted to solving maximum independent set (MIS) problems, which are known to have several applications including industries such as telecom and chip design. Previously, [studies reported](https://arxiv.org/pdf/2202.09372.pdf) a quantum speed-up of quantum adiabatic algorithms using neutral-atoms hardware against simulated annealing in graph instances of the defective king’s lattice (aka Union-Jack) class, where vertices are positioned in a square lattice with a few vertices removed randomly and connections are defined via a unit-disk constraint binding first neighbors (laterals) and second neighbors (diagonals). The king’s lattice problem instances are relevant for specific applications, in particular being [directly related](https://www.cs.du.edu/~snarayan/sada/research/docs/p130-hochbaum.pdf) to very large-scale integration (VLSI) in chip design.

More recently, the quantum speed-up against simulated annealing was more [thoroughly analyzed](https://arxiv.org/pdf/2306.13123.pdf), and conditions for performance guarantee were determined. Yet, [another work](https://arxiv.org/pdf/2307.09442.pdf) has demonstrated that more advanced classical algorithms can compute the MIS of defective king’s lattice graphs with efficiency comparable to quantum systems, solving problem instances in the order of thousands of vertices without need of specialized hardware. This sets a high bar for quantum hardware to beat, yet it introduces a silver lining. In this same work, it was also discovered that with vertices placed in a defective square grid, but with connectivity via unit-disks of order of 3 times the grid lattice constant, leads to graphs that are orders of magnitude harder for these advanced classical exact solvers. Here are some examples for you to draw inspiration from.

<p float="middle">
  <img src="/assets/Rba_sqrt2.png" width="30%" />
  <img src="/assets/Rba_3.png" width="30%" /> 
</p>
<em> (left) A defective king's lattice with vertices on a 5x5 square grid with 20% dropped-out points. Edges connect the first (lateral and vertical) and second (diagonal) neighbors. (right) A similar defective square grid, but with vertices connecting edges at a larger scale equals up to 3 times the lattice constant. </em>
</br>
</br>

Your challenge is twofold:

* Explore this as a path for quantum advantage!
* Find an application in the areas of risk analysis and finance.

An example of an application of a (different) graph theoretic problem to the study of [**economic networks**](https://arxiv.org/pdf/2203.11972) is the maximal independent set (mIS) defined as following: mIS is an independent set that is not a subset of any other independent set. mIS is a different graph problem to MIS and, as seen [here](https://arxiv.org/abs/0907.3309), can be used to the study of strategic interactions in economic networks. After finishing the **theory** and **programming** part of the challenge below, you will need to find an application of the **MIS graph problem** to the areas of risk analysis and finance. 

Other good resources to look for inspiration are:

* [Quantum computing for finance: overview and prospects July 2018](https://arxiv.org/abs/1807.03890)
* [Quantum Computing for Finance: State of the Art and Future Prospects June 2020](https://arxiv.org/abs/2006.14510)
* [A Survey of Quantum Computing for Finance Jan 2022](https://arxiv.org/abs/2201.02773)
* [Quantum Computing for Finance July 2023](https://arxiv.org/pdf/2307.11230)
* [Quantitative Risk Management](https://www.researchgate.net/publication/235622467_Quantitative_Risk_Management_Concepts_Techniques_and_Tools)

</br>
</br>

## Activities:

### Theory
*	Determine the parameters of a quantum algorithm for a neutral atom quantum computer to solve MIS with a unit-disk radius for connectivity larger than king’s.
*	Determine if the parameters above can fit a machine like [Aquila](https://www.quera.com/aquila). If so, generate problem instances on [Bloqade](https://queracomputing.github.io/Bloqade.jl/dev/) that could be run on it.
    +	Extra points for finding if Rb/a=3 is realizable experimentally on Aquila, or what would be the best performant largest value.

### Programming
* Perform simulations using [Bloqade](https://queracomputing.github.io/Bloqade.jl/dev/) on the largest possible system sizes you can. Optimize your protocol to maximize chance of measuring an MIS (optimization methods include [Nelder-Mead](https://queracomputing.github.io/Bloqade.jl/dev/tutorials/5.MIS/main/), [Bayesian methods](https://arxiv.org/pdf/2305.13365.pdf), counter-diabatic methods, or more)
    + Demonstrating higher probability of measuring MIS => more points!
    + Being able to simulate larger systems => more points!
*	Use [generic tensor networks](https://github.com/QuEraComputing/GenericTensorNetworks.jl), [Bloqade](https://queracomputing.github.io/Bloqade.jl/dev/), or your favorite method to estimate the (quantum and classical) annealing hardness parameters of the problem instances you found
    + Finding harder instances => more points!
*   Run jobs of comparative problem instances on Aquila with algorithm of choice and analyze data (no hybrid closed-loop optimization is allowed, so just use classically optimized controls)
    + Larger problems and better chance to solution => more points

### Business
*   Determine an application for MIS in the areas of risk analysis, and finance.
    + bonus points if your application requires graphs fitting exactly the constraint of the Rb/a=3 type
*   Estimate how many vertices would be necessary for real-life problems. 
    + Bonus points for finding applications that require order near-term numbers of qubits, order few hundreds.

You are encouraged to play to your strengths and choose focus on activities above. The winning team will be the one with the strongest content overall, covering a minimum of 2 out of the 3 activities above. So make sure you divide your attention and efforts properly.

And to finish: don't forget to prepare a 5-10min presentation with your findings and results! Support visuals (such as slides) will be welcome. You will have an opportunity to present your results in this format during the project presentation time on Sunday.
