The original problem comes from the Fiddler on the Proof folks [here](https://thefiddler.substack.com/p/can-you-solve-the-tricky-mathematical). The problem statement is suppose you are handed a container with candy in it. The only information that you are given is that there are exactly 3 of candy A and the remainder is candy B. Suppose the bag is large and shaped in such a way that there is no reasonable guess that you could make on how much candy in total is in the container. After drawing 3 pieces of candy, you find they are all candy A. Can a reasonable guess be made as to how much candy is in the container? Lets give it a try. 

# Solving the problem
Lets start by testing a few values. Suppose there are $k\geq3$ pieces of candy in the bag. Then the chance of the first three pieces of candy being A is
$$
\text{Pr}(\text{First 3 are A}|k \text{ pieces total}) = \frac{3}{k} \frac{2}{k-1} \frac{1}{k-2}.
$$
Assuming that there is no reasonable prior on the amount of candy currently in the bag, we can use a constant prior with Bayes rule to get a posterior on the distribution on the total amount of candy.
$$
\text{Pr}(k \text{ pieces total}|\text{First 3 are A}) = \frac{\text{Pr}(\text{First 3 are A}|k \text{ pieces total})}{\sum_{k\geq 3}\text{Pr}(\text{First 3 are A}|k \text{ pieces total})}.
$$
First, lets tackle the normalization constant given in the denominator. With a partial fraction decomposition, we have
$$
3!\sum_{k\geq 3} \left( \frac{1}{2} \frac{1}{k} - \frac{1}{k-1} + \frac{1}{2} \frac{1}{k-2} \right) = \frac{1}{4}.
$$
Which is given by a telescoping series. Now we have
$$
\text{Pr}(k \text{ pieces total}|\text{First 3 are A}) = \frac{4}{k (k-1) (k-2)}.
$$
Taking the expected value gives
$$
\sum_{k\geq 3} \frac{4}{(k-1)(k-2)} = 4.
$$
Success! We have an expected value, so we're done, right? That *is* what the prompt asked for, but lets dig a little deeper

# Other metrics
The mean is just one way of expressing expectation of a how a distribution behaves. Another common estimation is the maximum likelihood, which simply takes the most likely (highest probability) outcome. Then the maximum likelihood outcome is actually $k=3$ pieces of candy in the bag. After all, this is four times more likely than the mean case of $k=4$. That *still* isn't really satisfying though. What else do we have

The median is defined as the value $m$ where half the time the realization falls above $m$ and half the time it falls below $m$. This value falls between $3$ and $4$, (taken to be $3.5$ by convention). Okay, so this all seems great, we can be relatively confident that $3$ or $4$ are both pretty solid guesses by standard metrics, but there's a catch. the variance of this distribution is undefined. The spread around any of these metrics diverges. So there's no 'tightness' around any of these. This phenomena is common in situations where a distribution behaves asymptotically as a power law ($k^{-3}$ in this case)
# Extra Credit
Suppose now that there are guaranteed to be $N$ of candy A and the rest are candy B, and you reach in $j$ times to find them all to be candy A. Using the same process as above, we get
$$
\text{Pr}(k) \propto \frac{1}{k(k-1)\dots(k-j+1)},
$$
and a support of ${N, N+1, \dots}$ . Summing over the support to get the normalization constant gives
$$
A = \frac{(j-2)!(N+j-1)!}{(j-1)!(N+j-2)!N(N-1)\dots(N-j+1)}.
$$
Again, taking the expected value and using the resulting telescoping series gives
$$
E[k] = N\left( 1+\frac{1}{j-2} \right).
$$
For $N$ and $j$ large, we can approximate this by $E[k]=N + \frac{N}{j}$. Note that this distribution is asymptotically a power law with $k^{-j}$ for large $k$. 