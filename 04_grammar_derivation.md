#math 

# Definition
A **derivation** for a [[03b_grammars#Definition|grammar]] $G = (A, N, S, R)$ is any sequence of words $p_i \in (A \cup N)^*$ satisfying:
- the *first* word is $S$
- every *following* word can be obtained from the previous word by applying a rule in $R$ to one of its non-terminals
- the *last* word only has letters from $A$
<br>

Each parse tree of $G$ has multiple derivations resulting from expanding the tree gradually from $S$, including:
- **leftmost derivation**: always expand the leftmost non-terminal
- **rightmost derivation**: always expand the rightmost non-terminal

**Notation**: if there are multiple rules with the same LHS, their RHSs can be joined into one by using a union:
$$S ::= p \quad \text{and} \quad S ::= q \qquad \Rightarrow \qquad S ::= p\ |\ q$$

<br>

## Example: Balanced Parentheses
Let $L$ be the language words consisting only of **balanced parentheses**, meaning every opening parenthesis has a matching closing one.

The grammar describing this is given by:
- $A = \ \textbf(,\ \textbf)$
- $N = \{S\}$
- $S = S$
- the only rule in $R$ is $S ::= \varepsilon\ |\ S(S)S$
