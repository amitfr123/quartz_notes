---
title: Automatons
draft: false
description: Summary on automata theory and different types of automatons
---
# Context needed before:
1) [[Set]]
2) [[Formal Languages]]
# Abstract Machines
An `abstract machine` is a theoretical model used to analyse how a computer works. Abstract machines are similar to mathematical functions as they receive an input and produce an output based on predefined rules.
They are called `abstract` as they ignore hardware, and `machines` because they perform step by step execution typically characterized by changes of state.

## Deterministic vs Non-Deterministic
`Deterministic abstract machines` are models that for a given input will always produce the same output is the same manner (there is no randomness or variation). 
`Non-Deterministic abstract machines` are models that for a given input may produce a different outputs or take several different paths to produce the same output.

# Model of Computation
A `module of computation` is a module that describes hos an output of a mathematical function is computed given an output.
# Automaton Definition
Automata are a subset of `abstract machines` that are "self operating" and they are also a `modules of computation`.
## Formal definition
`Automaton` $M$ can be represented by $M = (\Sigma,\Gamma,Q,\delta,\lambda)$.
* $\Sigma$ - finite set of symbols called the input alphabet.
* $\Gamma$ - finite set of symbols called the output alphabet.
* $Q$ - set of states.
* $\delta$ - state transition (next state) function $\delta : Q \times \Sigma \rightarrow Q$. 
* $\lambda$ - next output function $\lambda : Q \times \Sigma \rightarrow \Gamma$ that maps state and input pairs to outputs.
This definition does not explicitly define the starting state $q_{0}$ and assume that it is defined within $\delta$. If we want to explicitly define $q_{0}$ we can use the definition $M = (\Sigma,\Gamma,Q, q_{0},\delta,\lambda)$.
## Acceptors (Recognizers)
Common uses of `automata` are accept/reject functions. For any given input the there is a binary output based on the final state. The generic definition of this types of `automata` is $M = (\Sigma,Q,q_{0},F,\delta)$ where:
* $q_{0}$ - initial state.
* $F$ - set of accepting states.
**When the `automaton` $M$ is used to accept a language $L$ it is commonly denoted $L(M)$.**

# Closure Properties
Let language $L_{1}$ be accepted by `acceptor automaton` $M$, and $O$ an operation that can be performed on a language. 
If $L_2$ the result of $O(L_{1})=L_{2}$ is accepted by $M$ then it is said that $M$ is closed under $O$.

# Product Construction
## Formal Definition
Let 2 `acceptor automata` $M_{1} = (\Sigma,Q_{1},q_{0}\in{Q_{1}},F_{1},\delta _{1})$ that accepts $L1$ and $M_{2} = (\Sigma,Q_{2},q_{0}\in{Q_{2}},F_{2},\delta _{2})$ that accept $L2$. `Product automaton` $M$ of $M_{1}$ and $M_{2}$ is defined:
$$
M=(\Sigma,Q,(q_{0}\in{Q_{1}}, q_{0}\in{Q_{2})},F,\delta)
$$
where:
* $Q=Q_{1}\times{Q_{2}}$
* $c\in{\Sigma}$
* $q_{i}\in{Q_{1}}$, $q_{j}\in{Q_{2}}$
* $\delta$ - $\delta : ((Q_{1}, Q_{2}) \times \Sigma) \rightarrow (Q_{1},Q_{2})$
	* $\delta((q_{i},q_{j}), c)=(\delta _{1}(q{i}, c), \delta _{2}(q_{j}, c))$
* $F$ - is the accepting set depending on the product we want.
	* To recognize $L_{1}\cup{L_{2}}$ the accepting set will be $F=(F_{1}\times{Q_{2}})\cup{(F_{2}\times{Q_{1}})}$
	* To recognize $L_{1}\cap{L_{2}}$ the accepting set will be $F=F_{1}\times{F_{2}}$

# Automaton Types
## Finite State Automaton (FSA)
When an `automaton` set of state ($Q$) is finite it is called a `finite state automaton (FSA)` or `finite state machine (FSM)`.
There are 2 types of `FSA`:
* [[#Deterministic Finite State Automaton (DFSA)]]
* [[#Non-Deterministic Finite State Automaton (NFSA)]]
`FSA automaton` are capable of accepting [[Formal Languages#Regular language|regular languages]].
### Deterministic Finite State Automaton (DFSA)
`DFSA` (also called `DFA`,`DFM`,`DFSM`) is type of `FSA` that is [[#Deterministic vs Non-Deterministic|deterministic]].

#### DFSA Acceptor Example
Here is an example for an accpetor `DFSA` whose input is binary representation of a number and accepts it only if it is a multiple of 3:
![[dfa_example_multiplies_of_3.svg.png]]
The way it works is by keeping track of the reminder $r$ and the next input bit $i$.
* $S_{0}$ is $r = 0$.
* $S_{1}$ is $r = 1$.
* $S_{2}$ is $r = 2$.
The reminder can be calculated in the following manner:
$$
\displaylines{
r_{0} = 0 \\
r_{n+1} = (r_{n} \cdot 2 + i_{n+1}) \mod {3}
}
$$
The $(r_{n} \cdot 2 + i_{n+1}) \mod {3}$ can be expressed with states instead. The following code performs the same logic but with states:
```C
// bit is a bool where TRUE==1 and FALSE==0
switch(reminder) {
	case 0:
		reminder = (bit)? 1 : 0;
		break;
	case 1:
		// (1 * 2 + 1) % 3 = 0
		// (1 * 2 + 0) % 3 = 2
		reminder = (bit)? 0 : 2;
		break;
	case 2:
		// (2 * 2 + 1) % 3 = 2
		// (2 * 2 + 0) % 3 = 1
		reminder = (bit)? 2 : 1;
		break;
}
```
Here is a code example that implements the entire automaton:
```C
bool is_mult_of_3(bool* bit_array, unsigned array_size) {
	unsigned reminder = 0; // starting state S0
	for (unsigned i = 0; i < array_size; i++) {
		bool bit = bit_array[i];
		switch(reminder) {
			case 0:
				reminder = (bit)? 1 : 0;
				break;
			case 1:
				// (1 * 2 + 1) % 3 = 0
				// (1 * 2 + 0) % 3 = 2
				reminder = (bit)? 0 : 2;
				break;
			case 2:
				// (2 * 2 + 1) % 3 = 2
				// (2 * 2 + 0) % 3 = 1
				reminder = (bit)? 2 : 1;
				break;
		}
	}
	return reminder == 0; // accepting state S0
}
```

### Non-Deterministic Finite State Automaton (NFSA)
`NFSA` (also called `NFA`,`NFM`,`NFSM`) is type of `FSA` that is [[#Deterministic vs Non-Deterministic|non-deterministic]].
#### NFSA Acceptor Formal Definition
`NFSA` `acceptor` $M$ can be represented by $M = (\Sigma,Q,q_{0},F,\delta)$ where $\delta$ is redefined as $\delta : Q \times \Sigma \rightarrow P(Q)$  ($P(Q)$ is a [[Set#Powerset|powerset]] of $Q$). This means that when calculating the next state there could be multiple options.
#### Powerset Construction
`Powerset construction` is a standard method to convert `NFSA` to `DFSA`. Using this method any `NFSA` can be expressed instead using a `DFSA`.
Here are the steps of the algorithm:
 1. We take the original `automaton` $M = (\Sigma,Q,q_{0},F,\delta)$.
 2. We create a new `automaton` $M' = (\Sigma,Q',q_{0}',F',\delta')$.
	 1. $Q'=P(Q)$.
	 2. $q_{0}'=\{q_{0}\}$.
	 3. $F'=\{S|S\subseteq{Q'},S\cap{F}\ne{\emptyset}\}$
	 4. $\delta': Q'\times{\sigma} \rightarrow Q'$.
		 1. $\delta'(S\in{Q'}, c\in{\Sigma}) = \bigcup_{s \in S} \delta(s, c)$
##### Example
For $L(M)$ with $\Sigma = \{a,b\}$ contain all of the strings that end with ab:
$$L(M) = \{s \cdot ab|s\in{\Sigma^{*}}\}$$
`NFSA automaton`:![[powerset_construction_nfsa.png]]
Can be changed to a `DFSA`:
![[powerset_construction_dfsa.png]]
### Closure Properties
Because `NFSA` and `DFSA` are equivalent they share the same closure properties(see [[#Powerset Construction]]).
`FSA` are closed on the following properties:
* Union
* Complement
* Intersection
* Difference
* Concatenation
* Exponentiation
* Kleene star
* Kleene plus
* String homomorphism
* Inverse string homomorphism
* Reversal