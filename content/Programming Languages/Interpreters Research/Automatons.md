---
title: Automatons
draft: false
description: Summary on automata theory and different types of automatons
---
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

## Finite State Automaton (FSA)
When an `automaton` set of state is finite it is called a `finite state automaton (FSA)` or `finite state machine (FSM)`.

### FSA as Acceptors (Recognizers)
Most `FSA` are defined without an output alphabet because they are used only to accept or reject an input by looking at the final state of the automaton. This means that they produce not output or that their output is binary (true or false).
