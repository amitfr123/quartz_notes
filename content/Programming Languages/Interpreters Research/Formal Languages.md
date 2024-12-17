---
title: Formal Languages
draft: false
description: Summary on Formal Languages
---
> [!info] Incomplete article head
# Knowledge sources
* [Formal language wiki article](https://en.wikipedia.org/wiki/Formal_language)
* [Regular language wiki article](https://en.wikipedia.org/wiki/Regular_language)
* [Formal grammar wiki article](https://en.wikipedia.org/wiki/Formal_grammar)
* [Production wiki article](https://en.wikipedia.org/wiki/Production_(computer_science))
* [Regular grammar wiki article](https://en.wikipedia.org/wiki/Regular_grammar)
* [Computer Science An Interdisciplinary Approach: Chapter 5.1 Formal Languages](https://introcs.cs.princeton.edu/java/51language/)
* [Lawrence University-Theory of Computation: Lecture 3 Regular languages](https://www2.lawrence.edu/fast/GREGGJ/CMSC515/chapt01/Regular.html) - a good and simple explanation for regular language operations.
* [Context free language wiki article](https://en.wikipedia.org/wiki/Context-free_language)
* [Neso Academy-Context Free Grammar & Context Free Language](https://www.youtube.com/watch?v=5_tfVe7ED3g)

# Basic definitions
* Symbol - the basic building block, typically a character or a digit.
* Alphabet - a finite set of symbols.
* Words (strings) - a finite set of a specific alphabet symbols.
* Formal language - a set of assembled strings (derived from the same alphabet) according to a specific set of rules.
* Formal grammar - describes which words are valid according to the language's syntax (rules).

Here are a couple of examples for this definitions:

|        language        |  symbols  |  valid words   | invalid words |     |
| :--------------------: | :-------: | :------------: | :-----------: | --- |
|        English         | a->z,A->Z |  Hello, World  |     uvbi      |     |
|         Binary         |    0,1    |   01001,1111   |       -       |     |
|      Odd integers      |   0->9    |   1,3,21,889   |    2,32,44    |     |
| Lower case palindromes |   a->z    | aba, aa, czdzc | av,var,hello  |     |
# Formal grammar
Formal grammar describes which words are valid according to the language's syntax. Grammar mainly consists of a set of [[#Production rules|production rules]].
The other components are a finite set $N$ of **non-terminal symbols**[^1], a finite set $\Sigma$ (the alphabet) of **terminal symbols**[^2] and a start symbol $S$ which is a non-terminal symbol $S\in N$.

**The letter $\epsilon$ is used to denote a terminal empty string\\symbol.**[^3]
## Production rules
A production rule is a rewrite rule (rule specifying when to replace a string with another), specifying a symbol substitution that can be recursively performed to generate new symbol sequences.
**A set of finite production rules is the main component of formal grammar.**
## Grammar generation
The process to generate a string in a formal language starts with a starting symbol and then successively applies production rules to rewrite this string. The process stops when a string containing only terminals is obtained.
The language consists of strings achieved by this process.

**If there are several ways yo generate this string then the grammar is deemed as ambiguous**

### String generation example
Assuming an alphabet $\Sigma=\{a,b\}$, non-terminal set $N=\{S\}$ and the start symbol $S$.
With the following production rules:
$$
S \to aSb
$$
$$
S \to ba
$$
We can develop the string we the following sequence.
$$
S\to aSb\to aaSbb\to aababb
$$
The language of the grammar is the infinite set $\{a^nbab^n|n\geq{0}\}=\{ba,abab,aababb,\dots\}$.
# Regular language
Regular language is a class of formal languages that can be defined using regular expressions.
Regular languages help us solve 2 problems:
* Specification - how do we have a complete definition for a formal language.
* Recognition - given language **L** and a word **W** how do we check if: $$W\in{L}$$
**It should be noted that tools like Yacc Bison and Flex may be used to create regular languages for programming.**

## Regular grammar
Regular grammar is a grammar that is **right-regular** or **left-regular** and it requires:
* All production rules have at most one **non-terminal symbol**[^1].
* That symbol is either always at the start(for left-regular) or the end(for right regular) of the rule.
### Example of a regular grammar
In order to describe the language $\{a^nb^m|m,n\geq{1}\}$. We can use the grammar $G$ with $N=\{S,A,B\},\Sigma=\{a,b\}$ and the start symbol $S$ and the following production rules:
$$
S \to aA
$$
$$
A \to aA
$$
$$
A \to bB
$$
$$
B \to bB
$$
$$
B \to \epsilon
$$
Note [^3].
We can develop the string we the following sequence.
$$
S\to aA\to aaA\to aabB\to aab
$$
## Operations
When using regular languages we have a set of basic operations:
* Union
* Complement
* Intersection
* Difference
* Concatenation
* Closure R*
* String homomorphism[^4]
* Inverse string homomorphism[^4]
* Reversal
### Union
Given 2 regular languages $L_{1}$ and $L_{2}$, then $L=L_{1}\cup{L_{2}}$ is also a regular language.

$$
L_{1} = \{a,ba\}
$$
$$
L_{2} = \{ab,ba,b\}
$$
$$
L=L_{1}\cup{L_{2}} = \{a, ab,ba,b\}
$$
### Complement
If **L** is a regular language, then $\overline{L}$ is also a regular language.

### Intersection
If $L_{1}$ and $L_{2}$ are regular languages, then the new language $L=L_{1}\cap{L_{2}}$ is also a regular language.
### Difference
If $L_{1}$ and $L_{2}$ are regular languages, then the new language $L=L_{1}-{L_{2}}$ is also a regular language.
### Concatenation
If $L_{1}$ and $L_{2}$ are regular languages, then the concatenation of the two ($L=L_{1}\cdot{L_{2}}$) is a regular language.
$$L=L_{1}\cdot{L_{2}}$$
$$
L = (x|x=x_{i}y_{j}, x_{i}\in{L_{1}}, y_{i}\in{L_{2}})
$$
### Closure R* (Kleene star)
If $L$ is regular language, then $L^{*}$ is the language comprised of all the possible concatenation 0 or more words.
$$
L = {a,b}
$$
$$
L^{*} = \epsilon\cup L\cup L\cdot L\cup L\cdot L\cdot L\dots
$$

### String homomorphism
If $L$ is a regular language and $h$ is a homomorphism on its alphabet then $L_{h}=h(L) = \{h(w) | w\in L\}$ is also a regular language.

### Inverse string homomorphism
If $L_{h}$ is a regular language and is the result of applying $h$ (a homomorphism) on an alphabet of a regular language then $L=h^{-1}(L_{h}) = \{w | h(w)\in L\}$ is also a regular language.

### Reversal
Given regular language $L$ then $L^R$ is the set of all the strings in $L$ reversed and is also a regular language.

## Generalized regular expressions
In practice regular expressions extend this basic operations and use meta-symbols like: "|", "\*", ".".
By using this meta-symbols we can define regular languages for things like:
* US phone numbers - \\(\[0-9]{3}\\) \[0-9]{3}-\[0-9]{4}

# Context-free language(CFL)
Context-free languages are languages that are generated by [[#Context-free grammar]].
This types of languages are used in many applications for example arithmetic expressions in programming languages.

## Context-free grammar(CFG)
Context-free grammar is defined as a formal grammar whose production rules can be applied to a non-terminal symbol[^1] regardless of its context.
This means that each production rule follows the form $A\to a$ where $A\in N$ and a is a string comprised of terminal[^2] and non-terminal symbols[^1] (may also be $\epsilon$[^3]).

**For an example of context-free grammar look at the example in [[#String generation example]].**
A popular way for context-free grammar notation is [BNF](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form).

# Context-sensitive language(CSL)
Context-sensitive languages are languages that are generated by [[#Context-sensitive grammar]].
## Context-sensitive grammar(CSG)
Context-sensitive grammar is defined as a formal grammar whose production rules can be surrounded by a context of terminal[^2] and non-terminal symbols[^1].

It is easier to explain it as a distinction from [[#Context-free grammar]] as instead of each rule forms $A\to a$ there is a context element described as $aAb\to a\lambda b$ where $a$ and $b$ and $\lambda$ are strings comprised of terminal[^2] and non-terminal symbols[^1] ($\lambda$ may not be $\epsilon$[^3]). This means that the rule only applies if the context surrounding $A$ is correct.

### Example of a context-sensitive grammar
In order to describe the language $\{a^nb^nc^n|n\geq{1}\}$. We can use the grammar $G$ with $N=\{S,A,B,C,W,Z\},\Sigma=\{a,b,c\}$ and the start symbol $S$ and the following production rules:
1. $S \to aBC$
2. $S \to aSBC$
3. $CB \to CZ$
4. $CZ \to WZ$
5. $WZ \to WC$
6. $WC \to BC$
7. $aB \to ab$
8. $bB \to bb$
9. $bC \to bc$
10. $cC \to cc$

We can develop the string we the following sequence (arrow number denotes the corresponding rule).
$$
\displaylines{
S \\
\to_{2} \pmb {aSBC} \\
\to_{2} a\pmb{aSBC}BC \\
\to_{1} aa\pmb {aBC}BCBC \\
\to_{3} aaaB\pmb {CZ}CBC \\
\to_{4} aaaB\pmb {WZ}CBC \\
\to_{5} aaaB\pmb {WC}CBC \\
\to_{6} aaaB\pmb {BC}CBC \\
\to_{3} aaaBBC\pmb {CZ}C \\
\to_{4} aaaBBC\pmb {WZ}C \\
\to_{5} aaaBBC\pmb {WC}C \\
\to_{6} aaaBBC\pmb {BC}C \\
\to_{3} aaaBB\pmb {CZ}CC \\
\to_{4} aaaBB\pmb {WZ}CC \\
\to_{5} aaaBB\pmb {WC}CC \\
\to_{6} aaaBB\pmb {BC}CC \\
\to_{7} aa\pmb {ab}BBCCC \\
\to_{8} aaa\pmb {bb}BCCC \\
\to_{8} aaab\pmb {bb}CCC \\
\to_{9} aaabb\pmb {bc}CC \\
\to_{10} aaabbb\pmb {cc}C \\
\to_{10} aaabbbc\pmb {cc}
}
$$

# Recursively enumerable language(RE)
**To be continued.**

# Footnotes
[^1]: Non-terminal symbols - symbols that can be replaced also called syntactic variables.
[^2]: Terminal symbols - symbols that may appear in the output of production rules and cannot be changed by the rules of the grammar this symbols comprise the alphabet.
[^3]: The symbol $\epsilon$ denotes an empty string.
[^4]: String homomorphism means that each character in the alphabet is replaced by a single string.