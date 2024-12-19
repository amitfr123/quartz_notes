---
title: Words
draft: false
description: Short summary on words and operation definitions
---
# Definition of a word
A word (string) is finite set of a specific alphabet symbols.
Go to [[Formal Languages#Basic definitions|formal languages basic definitions]] for examples and more background.

# Operations
### Concatenation
If $W_{1}$ and $W_{2}$ are words, then the concatenation of the two ($W=W_{1}\cdot{W_{2}}$) is a word.
$$
\displaylines{
W_{1}=ab, W_{2}=bcc \\
\begin{aligned}
W&=W_{1}\cdot{W_{2}} \\
&=abbcc
\end{aligned}
}
$$
### Power
If $W$ is a word, then $W^{n}$ is the concatenation of the word W $n$ times.
$$
\displaylines{
W^{n+1} = W^{n} \cdot W\\
W^{0} = \epsilon
}
$$
## Reverse
If $W$ is a word then $W^{R}$ is the word $W$ reversed.
$$
\displaylines{
(W\cdot\sigma)^{R} = \sigma \cdot W^{R} \\
\epsilon^R = \epsilon
}
$$
## Length (cardinality)
If $W$ is a word then $n$ is called the length (cardinality) of $W$ if n is equal to the number of `symbols` within $W$.
$$
\displaylines{
W = aa \\
\begin{aligned}
n &= |W| \\
&=2
\end{aligned}
}
$$
## More definitions
## Prefix
$X$ is called a `prefix` of word $W$ if:
$$
W=X \cdot Y
$$
## Suffix
$X$ is called a `suffix` of word $W$ if:
$$
W=Y \cdot X
$$
## Sub Word (Sub String)
$X$ is called a `sub word` suffix of word $W$ if:
$$
W=Y \cdot X \cdot Z
$$
$\epsilon$ is a `sub word` for any word.
### The number of sub words
If $W$ is a word and $S$ is a set that contains every possible `sub words` for $W$ then:
$$
\displaylines{
n + 1 \le |S| \le 1 + \frac{n(n+1)}{2}
}
$$
$n+1$ is when the word is comprised only using a single `symbol` and $1 + \frac{n(n+1)}{2}$ is when the word doesn't contain a duplicate symbols.