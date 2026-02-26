#math
# Definition
A context-free **grammar** is defined as $G = (A, N, S, R)$ where:
- $A$ are the *terminals*, the alphabet for generated words $w \in A^*$
- $N$ are the *non-terminals*, symbols with (recursive) definitions
- $R$ are grammar rules $(n, \ v)$ written $n ::= v$ where
	- $n \in N$ is a non-terminal
	- $v \in (A \cup N)^*$ is a sequence of terminals and non-terminals
- $S$ is the *starting symbol* from which *derivation* starts
	- Each step then replaces a non-terminal with one of its definitions, as there may be multiple.

**Example**:
The context-free grammar for $a^nb^n$ (which is impossible to describe with an NFA) has the following rules:
- $S ::= \varepsilon$
- $S ::= aSb$

This grammar could be derived as follows:
$$S \quad \Rightarrow \quad aSb \quad \Rightarrow \quad aaSbb \quad \Rightarrow \quad aaaSbbb \quad \Rightarrow \quad aaabbb$$

And it has the following formal definition:
$$G = \left(A = \{a, b\}, \ N = \{S\}, \ S = S, \ R = \{(S, \varepsilon), \ (S, aSb)\}\right)$$

<br>

## Parse Tree
We call $t$ a **parse tree** of a given grammar $G = (A, N, S, R)$ if and only if $t$ is a *node-labeled* tree with ordered children that satisfies:
- the *root* is labeled $S$
- all *leaves* are labeled by elements in $A$
- each *non-leaf node* is labeled by an element in $N$
- if the *children* of a non-leaf node $n$ are labeled $p_1, ..., p_n$ from left to right, then there must be a rule $(n ::= p_1 ... p_n) \in R$

### Yield
The unique word in $A^*$ obtained from reading the leaves of $t$ from left to right is called the **yield**.

We can then define the **[[02_languages#Definition|language]]** $L(G)$ of the tree $t$ as all yields of all parse trees of $G$.