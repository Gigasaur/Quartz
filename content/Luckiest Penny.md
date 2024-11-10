This one isn't *technically* part of Fiddler on the Proof, but is in fact from its no-defunct spiritual predecessor on FiveThirtyEight called the Riddler. Original post found [here](https://fivethirtyeight.com/features/can-you-find-the-luckiest-coin/).

# The Problem
You've just been handed $N$ pennies (in the original post $N=1,000,000$). You endeavor to find the luckiest of all them to keep as your own. So you flip all of the pennies at once, keep the ones that land on heads, and rinse and repeat until you are, hopefully, left with a lone survivor that you will keep to flip head to head with your friend's luckiest penny.
# My Solution
Denote by $P_n$ the probability that a sequence of throws, starting with $n$ pennies *does* result in a lone luckiest penny. Then we may write
$$
P_{n} = \frac{1}{2^n}\sum_{j\geq 1} \binom{n}{j}P_{j}.
$$
Note that $P_0=0$ since no luckiest penny has been, nor can be, selected if you are starting with no pennies, and we define $P_1=1$. 

# Closing Thoughts
