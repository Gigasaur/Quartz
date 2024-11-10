[https://xkcd.com/356/](https://xkcd.com/356)] Oldie but a goodie. Plenty of folks have looked at this problem, but I wanted to give my take on it.

# Problem Statement

Consider a square grid tessellating $\mathbb{R}^2$. On each edge, there is a resistor of resistance $R$. We want to find the equivalent resistance between any two nodes. WLOG, identify the first node as $(0, 0)$ and the second node $(p, q)$ with $p, q \in \mathbb{Z}$ and $(p, q)\neq (0, 0)$.

## My Solution

We will consider this problem on $\mathbb{T}^2$ instead and take the limit as the Torus becomes arbitrarily large. This is equivalent to an $N\times N$ grid with periodic boundary conditions.

Identify the voltage at each node as $v_{j, k}.$ Inject a current $I$ into node $(0, 0)$ and extract a current $I$ from node $(p, q)$. Then the equations defining the distribution of voltages are Kirchoffs Voltage Rules. Here, they are written as
$$
\begin{aligned}
     \frac{4 v_{j, k} - v_{j-1, k} - v_{j+1, k} - v_{j, k-1} - v_{j, k+1}}{R} &= -I\delta_{j, k} + I\delta_{j-p, k-q}, \, j, k \in \{0, 1, 2, \ldots N-1\}^2 \\ 
    &v_{-1, k} = v_{N-1, k}\\
    &v_{N, k} = v_{0, k}\\
    &v_{j, -1} = v_{j, N-1}\\
    &v_{j, N} = v_{j, 0}.
\end{aligned}
$$

Now, define the Discrete Fourier Transform of $v_{j, k}$ as 
$$
    X_{l, m} \equiv \sum_{k=0}^{N-1}\sum_{j=0}^{N-1} v_{j, k} \exp\left(\frac{2\pi i (kl + jm)}{N}\right).
$$
Now, we can rewrite the primary recurrence relationship as 

$$    \frac{[4-\exp\left(\frac{2\pi i l}{N}\right)-\exp\left(-\frac{2\pi i l}{N}\right)-\exp\left(\frac{2\pi i m}{N}\right)-\exp\left(-\frac{2\pi i m}{N}\right)]X_{l, m}}{R} = I\left(\exp\left(\frac{2\pi i (pl + qm)}{N}\right) - 1\right).$$

This can be rearranged to solve for the $X_{l, m}$ as

$$    X_{l, m} = IR\frac{\left(\exp\left(\frac{2\pi i (pl + qm)}{N}\right) - 1\right)}{[4-\exp\left(\frac{2\pi i l}{N}\right)-\exp\left(-\frac{2\pi i l}{N}\right)-\exp\left(\frac{2\pi i m}{N}\right)-\exp\left(-\frac{2\pi i m}{N}\right)]}.$$

So, taking the inverse Discrete Fourier Transform as 

$$    v_{j, k} = \frac{1}{N^2} \sum_{l=0}^{N-1}\sum_{m=0}^{N-1} X_{l, m} \exp\left(\frac{-2\pi i (kl + jm)}{N}\right),$$

leads to $v_{j, k}$ being recovered as

$$    v_{j, k} = \frac{1}{N^2} \sum_{l=0}^{N-1}\sum_{m=0}^{N-1} IR\frac{\left(\exp\left(\frac{2\pi i (pl + qm)}{N}\right) - 1\right)\exp\left(\frac{-2\pi i (kl + jm)}{N}\right)}{[4-\exp\left(\frac{2\pi i l}{N}\right)-\exp\left(-\frac{2\pi i l}{N}\right)-\exp\left(\frac{2\pi i m}{N}\right)-\exp\left(-\frac{2\pi i m}{N}\right)]}.$$

Then, to find the voltage difference between the two nodes of interest yields

$$    v_{0, 0} - v_{l, m} = \frac{1}{N^2} \sum_{l=0}^{N-1}\sum_{m=0}^{N-1} IR\frac{\left(\exp\left(\frac{2\pi i (pl + qm)}{N}\right) - 1\right)\left(1-\exp\left(\frac{-2\pi i (kl + jm)}{N}\right)\right)}{[4-\exp\left(\frac{2\pi i l}{N}\right)-\exp\left(-\frac{2\pi i l}{N}\right)-\exp\left(\frac{2\pi i m}{N}\right)-\exp\left(-\frac{2\pi i m}{N}\right)]}.$$

Call this quantity $\Delta$. $\Delta$ can be simplified to 

$$   
\Delta = -\frac{IR}{N^2} \sum_{l=0}^{N-1}\sum_{m=0}^{N-1} \frac{1-\cos(2\pi (pl + qm) / N)}{(1-\cos(2\pi l/N)) + (1-\cos(2\pi m/N))}.
$$

Finally, this can be interpreted a Riemann sum. Letting $N$ diverge without bound, then $\Delta$ becomes $\Delta_{\infty}$ and is given by 

$$    \Delta_{\infty} = -IR\int_{0}^{1}\int_{0}^{1} \frac{1-\cos(2\pi(px + qy))}{(1-\cos(2\pi x))+(1-\cos(2\pi y))}\mathrm{d}x \mathrm{d}y.
$$
As a concrete example, take $p=q=1$. Integration yields $\Delta_\infty = -\frac{2IR}{\pi}$, and so the effective resistance across those two nodes is simply 
$R_{eff} = \frac{2R}{\pi}$. Similarly, for any immediately adjacent node, the calculation works out to $R_{eff}=\frac{R}{2}$.

While this concludes the problem in question, it also provides a solution for the same problem on $\mathbb{R}^N$ as

$$    \Delta_{\infty} = -IR\int_{0}^{1}\int_{0}^{1}\cdots\int_{0}^{1} \frac{1-\cos(2\pi\sum_{k = 0}^{N-1}a_k x_k)}{\sum_{k=0}^{N-1}(1-\cos(2\pi x_k))}\mathrm{d}x_0 \mathrm{d}x_1\cdots\mathrm{d}x_{N-1}.
$$
Finally, from $\Delta_{\infty}$, taking $p = 1, q=2$ as in the original problem statement, recovers 
$$
\frac{R_{eff}}{R} = -\frac{1}{2}+\frac{4}{\pi}
$$
