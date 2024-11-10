I like to play Tabletop Roleplaying games from time to time, which means that I roll of twenty sided dice. One game I play in uses a peculiar system wherein if the maximum of the die is rolled, you can roll again and combine the results. If you roll the maximum again, you rinse and repeat. It got me thinking how often that should this happen. A single natural 20 is a rare event by itself, but how often do I need to wait between getting 2, 3, or even 4 consecutive natural 20s? It turns out to be a bit more complicated than a Poisson distribution. 

Consider how many rolls you expect to take between single instances of a natural 20. Since each roll of the die is independent, we the number of rolls between 20s follow a geometric distribution with $k\sim \text{Geom}(p)$, with  $p=1/20$. So $E[k]=20$ is the expected time between natural $20$s. Now, consider expected time between streaks of $2$ natural $20$s. For this to happen, we need a natural $20$ to occur, then there's another 1 in $20$ chance of a streak. That adds another roll slot, and so increases the effective length needed to consider by $1$. This new effective slot of length $21$ needs to occur on average $20$ times for the extra natural $20$ to occur, so this gives $20*21=420$. \

Generally, this can be applied recursively. Letting $\mu_n$ denote the expected time between streaks of $n$ natural $20$ rolls, we have
$$
\mu_{n+1}=20 (\mu_{n}+1).
$$
After de-recursing, we have
$$
\mu_{n}=\frac{20}{19} (20^n-1).
$$
We can show this a bit more rigorously. Let $T_n$ be the random variable denoting the number of rolls between streaks of $n$ natural $20$s. Then we have
$$
T_{n+1} = \sum_{k=1}^X (T_{n}^k +1),
$$
where $X$ is distributed as $\text{Geom}(p), \, p=\frac{1}{20}$ and $T_n^k$ is the $k$th realization of $T_n$. Then, taking expected values of each side, since $T_n^k$ and $X$ are all independent, we recover $T_{n+1}=20(T_{n}+1)$.  We can also take second moments to get
$$
\langle T_{n+1}\rangle^2 = 20 \langle T_{n} + 1\rangle^2 = 20 (\langle T_{n}\rangle^2 + 2 \langle T_{n}\rangle +1).
$$
Combining gives
$$
\sigma_{n+1}^2 = 20 \sigma_{n}^2; \quad \sigma_{n}^2 = 19*20^{n}.
$$
Clearly, the mean and variance of $T_{n}$ are not equal, which is the characteristic of a Poisson distribution, but what distribution *is* $T_n$? Well, we know that $T_1$ is a geometric random variable. Then we can use the relationship between $T_{n+1}$ ant $T_n$ to see that $T_2$ is a sum of one geometric random variable and a geometric number of geometric variables. Then we have that 
$$
k \sim  \text{NB}(\nu+1, p);\quad \nu \sim \text{Geom}(p);\quad p=\frac{1}{20}
$$
Where NB denotes the negative binomial distribution. So $T_2$ itself is necessarily a compound distribution as opposed to a Poisson distribution. Further compound distributions for higher $T_n$ can be built in the same manner.

What does all of this even mean at the end of the day? Think of each factor of $20$ as weight classes in combat sport. Then, in this wonderful, imaginary world governed by dice rolls, you fight at your weight class most of the time. One in twenty days (about three weeks) you can win in a weight class one up. One in every $420$ days (about a year and change), you can win in two weight classes up, once every $8420$ days (around 23 years or a quarter of a lifetime), you can win in three weight classes up, four weight classes once every $168420$ days (about 460 years, or the lifespan of fantasy races, depending on the lore), and five weight classes once every $3,369,220$ days (almost $10, 000$ years, so human civilization may have seen it ... once).

Now, to top it off, we can recursively determine the distribution for $T_n$ generally as a compounded distribution. 
$$
\begin{aligned}
T_{n} \sim \text{NB}(\nu_{n}+1, p)\\
\nu_{n} \sim \text{NB}(\nu_{n-1}+1, p)\\
\nu_{0}=0
\end{aligned}
$$
