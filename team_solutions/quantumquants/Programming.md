# Solution part II: Programming

## Programming
* Perform simulations using [Bloqade](https://queracomputing.github.io/Bloqade.jl/dev/) on the largest possible system sizes you can. Optimize your protocol to maximize chance of measuring an MIS (optimization methods include [Nelder-Mead](https://queracomputing.github.io/Bloqade.jl/dev/tutorials/5.MIS/main/), [Bayesian methods](https://arxiv.org/pdf/2305.13365.pdf), counter-diabatic methods, or more)
    + Demonstrating higher probability of measuring MIS => more points!
    + Being able to simulate larger systems => more points!
*	Use [generic tensor networks](https://github.com/QuEraComputing/GenericTensorNetworks.jl), [Bloqade](https://queracomputing.github.io/Bloqade.jl/dev/), or your favorite method to estimate the (quantum and classical) annealing hardness parameters of the problem instances you found
    + Finding harder instances => more points!
*   Run jobs of comparative problem instances on Aquila with algorithm of choice and analyze data (no hybrid closed-loop optimization is allowed, so just use classically optimized controls)
    + Larger problems and better chance to solution => more points

## Solution

As discussed in the section regarding the theoretical solution, in order to find a MIS for $R_b \approx 3a$, we believe it is useful to use $a = 2.5 \mu m$. This would enforce $\Delta = 4.19$ MHz at the end of the protocol. Furthermore, to ensure $\Omega \ll \Delta$, we would pick $\Omega = 1$ MHz. Even if we cannot test it directly on the hardware, we simulate this scenario (via a quantum-mechanical solver) for different lattice dimensions (L[^1]) and with different defect configurations [^2]. In the insets of the following figures, we report the lattice configurations (and corresponding graphs) analyzed. The red dots represent a possible MIS.

<figure>
  <img src="/figs/lattice_probs4.png/" width="1200" alt="Figure">
  <figcaption style="text-align:center;">Figure: Simulation results for different lattice sizes. The histrograms report the probability of measuring a particular final state. </figcaption>
</figure>

<br>We summarize the results of the simulation in the following table:

| Simulation # | L | P |
|----------|----------|----------|
| 1 | 4 | 95% |
| 2 | 5 | 84% |
| 2 | 6 | 96% |


where P is the probability of obtaining a MIS with the qubit measurement. To evaluate P, we first find all the different MIS's via a generic tensor networks method. We then calculate the overlap between the simulation result and the MIS's subspace. Its square yields P. The table shows that $a = 2.5 \mu m$ is a promising candidate to solve the hackathon task. Furthermore, since the experimental sequence involving $\Delta$ and $\Omega$ is heuristic, we make a step further and optimize it. In particular, we let $\Delta(t)$ to be a variational piecewise linear function, with variational $\Delta(t = start)$ and $\Delta(t = end)$. As far as $\Omega(t)$ is concerned, we do not change its functional form, but we consider its maximum value to be a variational parameter of our optimization. We show an example of an optimal experimental sequence in the following figure:

<img src="/figs/optimal_pulse.png/" width="800" >

We simulate the system with the exact same configurations used to study the heuristic protocol and we report the results of the simulation here:
| Simulation # | L | P |
|----------|----------|----------|
| 1 | 4 | 96% |
| 2 | 5 | 99% |
| 2 | 6 | X[^3] |

Even if we did not report the results of enough simulations to be statistically correct, we empirically observe the optimization protocol used improves the result.<br>
To validate both our theoretical and numerical approaches to the problem, we also simulated a defective king's lattice. The following figure reports the result for the protocol following the theoretical model (i.e., the pulse is not optimized), again showing our method yields with high accuracy the desired result.

<img src="/figs/defective_king.png/" width="400" >

However, as explained in the theoretical part, we believe the theoretical model does not have the capacity to point us to the MIS if we want $R_b = \sqrt{3\sqrt{10}}a$ and $a = 4 \mu m$. Therefore, we must numerically optimize the experimental sequence to be used in this case. We report in the following figure a simulation involving this case, considering the optimal experimental sequence found.

<img src="/figs/sim_final.png/" width="400" >

As expected, the protocol yields an MIS with lower probability than the previous cases. On the other hand, the success probability is large enough for the protocol to be useful, maybe allowing a larger number of experimental runs. We can compare the simulation results with actual experimental ones, shown in the next figure.

<img src="/figs/experimental.png/" width="800" >

The figure reports the results of 100 experimental shots with $R_b = \sqrt{3\sqrt{10}}a$ and $a = 4 \mu m$. In green we just report the probability of obtaining one of the different MIS's (the most probable one, from now on called $\alpha$). Observing the simulation results, $Prob(\alpha, sim.) \approx 65 \%$. Therefore, the experimental results are worse than the simulated one, as expected, due to the presence of SPAMs, unwanted interactions, etc. However, we really want to stress here that using an optimal experimental sequence strongly improves the results. Therefore, importantly, we believe that, by reducing $a$, it would be possible to run this protocol to solve interesting problems mappable to the one studied in this challenge. 

## Bonus

States evolution during the heuristic sequence.

<img src="/figs/spectrum.png/" width="400" >

Further improvements on pulse optimization will probably encompass a slowly varying $\Delta(t)$ around the middle of the experimental evolution.




[^1]: We use square lattices with LxL sites.
[^2]: Different defect configurations might correspond to different defect densities.
[^3]: We did not optimize this instance
