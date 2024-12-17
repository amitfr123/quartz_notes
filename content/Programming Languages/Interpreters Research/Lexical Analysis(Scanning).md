---
title: Lexical analysis(scanning)
draft: false
description: Summary on lexical analysis and practical implementation
---
> [!info] Incomplete article head
# Overview
Lexical analysis (also called scanning) is the act of taking a stream of character and separating it into the programming language tokens.
This action is done by defining a **lexical grammar** which is a [[Formal Languages#Formal grammar|formal grammar]] and is usually implemented as a [[Formal Languages#Regular grammar|regular grammar]]. **The lexical grammar defines the syntax of the tokens.**

**Lexical analysis is the first step in implementing an interpreter or compiler.**

# Implementation methods
## Naive implementation
A simple implementation for lexical may be implemented using character by character iteration with a switch/match/map operation in order to extract the tokens from the string.
Here is a pseudo code example:

```js
while !string.empty() {
	token, leftover_string = switch(string[0]) {
		case number:
			break handleNumber(string)
		case ":
			break handleStringLiteral(string)
		case letter:
			break handleIdentifier(string)
		...
	}
	push(token)
	string = leftover_string
}
```


## Regex based implementation
While you may implement functions for the tokens syntax in cases where your **lexical grammar** is a **regular grammar** you may also implement them using regex and checking for matches.
Here is an example of regex usage for a **C** styled language:
* String literal - "(\\.|\[^\\"\])\*"
* Decimal Integer - \[1-9\]\[0-9\]\*|0
* Hexadecimal integer - 0\[Xx\]\[0-9A-Fa-f\]+
* Identifier - \[A-Za-z_\$\]\[A-Za-z0-9_\$\]\*

Here is a pseudo code example:

```js
while !string.empty() {
	token = empty
	leftover_string = string
	if string.match(regex_1) {
		token_str, leftover_string = string.slice(regex_1)
		token = {type1,token_str}
	} else if string.match(regex_2) {
		token_str, leftover_string = string.slice(regex_2)
		token = {type2,token_str}
	}
	...
	push(token)
	string = leftover_string
}
```