# Solution Part I : Theory

## Task
*	Determine the parameters of a quantum algorithm for a neutral atom quantum computer to solve MIS with a unit-disk radius for connectivity larger than king’s.
*	Determine if the parameters above can fit a machine like [Aquila](https://www.quera.com/aquila). If so, generate problem instances on [Bloqade](https://queracomputing.github.io/Bloqade.jl/dev/) that could be run on it.
    +	Extra points for finding if Rb/a=3 is realizable experimentally on Aquila, or what would be the best performant largest value.
 
## Solution
### Parameters for an MIS Algorithm on Aquila
<img src="/figs/latticegeo.png/" width=400, align="right">

We take this challenge head on and tackle the critical point informed by [current research](https://arxiv.org/pdf/2307.09442) placing the radius of interest at $R_b=3a$, meaning the furthest atom to link to any other in the lattice is the one three parallel vertices removed. However we cannot choose the radius to fall directly on the atom itself, thus, as in the King's walk connectivity, we must in actuality choose the geometric mean between this and the next closest atom yielding $\hat R_b=\sqrt{3\sqrt{10}}a=\sqrt[4]{90}a$. Later we can further adjust this value between $3$ and $\sqrt{10}$ if beneficial. A sidenote: While here this is not a limiting factor, this is where Aquila imposes its first constraint. If we desire to impose an even larger connectivity of $\frac{R_b}a=N\in\mathbb N$ we by the same method must choose ${\hat R_b}=a\sqrt[4]{N^4+N^2}$, but given the precision of Aquila's MOTs at $\Delta r=0.1\mu m$ and a minimal lattice constant of $a=4\mu m$ we are limited to $\hat R_b-R_b\geq\Delta r\iff\sqrt[4]{N^4+N^2}-N\geq \frac{\Delta r}a\iff N\leq 9$. 

Considering the Hamiltonian in the slides, we first tackle the initialization of the system. Setting $\Omega = 0$ MHz, we choose $\Delta < 0$ and let the system spontaneously evolve to the ground state, where all the atoms are in the ground state. <br>
After the initialization, we vary $\Delta$ and $\Omega$ to create a superposition of states. Eventually, we unbalance it further by setting $\Omega = 0$ MHz to conclude our protocol. We choose the final $\Delta$ to be $\Delta = C_6 / \hat R_b^6 = C_6 / (\sqrt[4]{90}a)^6$ in order to obtain the required MIS. Picking $a = 4 \mu m$, we would need $\Delta = 0.25$ MHz. In order for the process to work, we believe $\Omega \ll \Delta$ [^1]. For example, $\Omega = 0.1$ MHz. However, due to the coherence time of the Aquila processor, we cannot run the protocol for more than 4 $\mu s$. Therefore, the driving term will not be fast enough to create a useful states superposition.

We want to propose here two workarounds when the time-behavior of $\Delta$ and $\Omega$ is not optimized to boost the performance of the protocol. <br>
*   We look for the largest $R_b = k_{max} a$ to satisfy our hardware constraints. <br>
    We run the process for $T = 4 \mu s$. Choosing $\Omega = 1/T = 0.25$ MHz, we think the driving term should be efficient enough to generate
    the wanted superpostion. Inverting $\Delta = C_6 / (k_{max} a)^6$, we get $k_{max} = (C_6/\Delta)^{(1/6)} \times 1/a$. Picking $\Delta =       0.5$ MHz $> \Omega$, we obtain $k_{max} = 2.73$. Therefore, the standard approach could be probably used for use cases involving $R_b \geq
    2a$, that we know being of practical interested from [current research](https://arxiv.org/pdf/2307.09442).
*   We know that, even if not cloud-accessible, it is possible to reduce a down to $a = 2.5 \mu m$. This would be beneficial because, in order 
    to obtain a final $\hat R_b=\sqrt{3\sqrt{10}}a$, we could set $\Delta = 4.19$ MHz. Thus, we could adopt a value of $\Omega$ large enough
     to drive the state into superposition but maintaining $\Omega \ll \Delta$.


[^1]: We compare Ω in the middle of the protocol with the final value of Δ. This is true throughout our solution files when Ω is compared with Δ.



































