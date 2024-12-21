---
title: Set
draft: false
description: Summary on set
---
# Knowledge sources
* [Set wiki article](https://en.wikipedia.org/wiki/Set_(mathematics))
# Definition
A `set` is a collection of different things that are usually called members or elements.
# Special sets
## Empty set
An `empty set` (also called `null set`) is a set with no items and denoted with: $\emptyset$, $\{\}$.
## Singleton set
A `singleton set` (also called `unit set`) is a set with a single item and denoted with $\{x\}$.
## Special Number Subsets
* $N$ - set of all natural numbers $N=\{0,1,2,3,\dots\}$
* $Z$ - set of all integers $Z=\{\dots,-2,-1,0,1,2,\dots\}$
* $Q$ - set of all rational numbers $Q = \{\frac{a}{b}|a,b\in{Z},b\ne{0}\}$
* $R$ - Set of all real numbers.
* C - Set of all complex numbers $C=\{a+bi|a,b\in{R}\}$
# Basic Operations
For the operations assume that the universal set $U$ exists that contains all of the possible members and any other set is subset of $U$.
## Complement
If $S$ is a set then its complement $\overline{S}$ is a set that contains all of the members not is $S$.
$$
\overline{S}=\{a|a\in{U},a\notin{S}\}
$$
## Union
If $S_{1}$ and $S_{2}$ are sets then their `union` $S$ is a set that contains all of the members in $S_{1}$ or $S_{2}$.
$$
\displaylines{
\begin{aligned}
S &= S_{1}\cup{S_{2}} \\
&= \{a|a\in{U},a\in{S_{1}} \lor a\in{S_{2}}\}
\end{aligned}
}
$$
## Intersection
If $S_{1}$ and $S_{2}$ are sets then their `intersection` $S$ is a set that contains all of the members in $S_{1}$ and $S_{2}$.
$$
\displaylines{
\begin{aligned}
S &= S_{1}\cap{S_{2}} \\
&= \{a|a\in{U},a\in{S_{1}} \land a\in{S_{2}}\}
\end{aligned}
}
$$
## Difference
If $S_{1}$ and $S_{2}$ are sets then their `difference` $S$ is a set that contains all of the members in $S_{1}$ that are not in $S_{2}$.
$$
\displaylines{
\begin{aligned}
S &= S_{1}\setminus{S_{2}} \\
&= \{a|a\in{U},a\in{S_{1}} \land a\in{\overline{S_{2}}}\}
\end{aligned}
}
$$
**The symbols $-$ may also be used instead of $\setminus$**.

## Symmetric Difference
If $S_{1}$ and $S_{2}$ are sets then their `symmatric difference` $S$ is a set that contains all of the members in $S_{1}$ that are not in $S_{2}$ and all of the members in $S_{2}$ that are not in $S_{1}$.
$$
\displaylines{
\begin{aligned}
S &= S_{1}\Delta{S_{2}} \\
&= S_{1}\setminus{S_{2}} \cup S_{2}\setminus{S_{1}} \\
&= (S_{1}\cup{S_{2}}) \setminus (S_{1}\cap{S_{2}})
\end{aligned}
}
$$
## Cartesian Product
If $S_{1}$ and $S_{2}$ are sets then their `cartesian product` $S$ is a set that contains all of the ordered pairs of members in $S_{1}$ and $S_{2}$.
$$
\displaylines{
\begin{aligned}
S &= S_{1}\times{S_{2}} \\
&= \{(a,b)|a\in{S_{1}, b\in{S_{2}}}\}
\end{aligned}
}
$$
## Length (Cardinality)
If $S$ is a set then $n$ is called the length (cardinality) of $S$ if n is equal to the number of members within $S$.
$$
n=|S|
$$
# Powerset
A `powerset` of set $S$ is a set of all subsets of $S$ including $\emptyset$ and $S$ and is symbolized by $P(S)$. If $|S|=n$ then $|P(S)|=2^{n}$.
Here is an example:
$$
\displaylines{
S=\{x,y,z\} \\
P(S)=\{\{\}, \{x\}, \{y\}, \{z\}, \{x,y\}, \{x,z\}, \{y,z\}, \{x,y,z\}\}
}
$$