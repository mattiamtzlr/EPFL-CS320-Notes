#implementation 

# LL(1) Parsing 
**Recursive Descent** parsing also known as **LL(1)** parsing is a pretty good parsing technique which can be easily implemented manually on the grammar and is *linear* in the size of the token sequence.

<br>

## Transforming the Grammar
To make LL(1) parsing work, we usually have to *rewrite the grammar* so that it is suitable for recursive parsing. This means there cannot be any ambiguity and all recursions have to have a base case.

The *rough idea* is that we transform a rule where the RHS has multiple alternatives (unions) into an if-else statement where the *first element* of each alternative is checked against. Then the corresponding alternative is recursively parsed.

**Remark**: by first element, we mean the '$\text{first}$' *predicate* defined on grammars as
$$\text{first}(B_1 ... B_p) = \{a \in \Sigma \ | \ B_1 ... B_p \ \Rightarrow\ \ ... \ \Rightarrow \ aw\}$$
for some word $w$, meaning it is possible to [[04a_grammar_derivation|derive]] the sequence $B_1 ... B_p$ to $aw$.
The first elements for each case should be distinct sets of tokens.

<br>

## Abstracting into Constraints
After having found all the first elements, we can abstract the grammar into just these first elements as **constraints** for the RHS of each rule, reducing complexity.
For nice grammars, these will be non-recursive and can be *solved directly by substitution*.

If the grammar still contains recursion, we have to solve the constraints *iteratively* until they stabilize, which will be in the worst case when every RHS is the union of all tokens.
Below is an example of this:
![[iterative_constraint_solving.png]]

<br>

## Follow Sets
We define the notion of a token **following** another as:
$$\text{follow}(X) = \{a \in \Sigma \ | \ S \ \Rightarrow \ ... \ \Rightarrow \ ...Xa... \}$$
In words: There exists a derivation from the start symbol $S$ producing a sequence of terminals and non-terminals where $a$ follows $X$ at some point.

**Rules**:
- Given $X ::= YZ$, we get
	- $\text{first}(Z) \subseteq \text{follow}(Y)$
	- $\text{follow}(X) \subseteq \text{follow}(Z)$
<br>

- Given $X ::= Y_1 \ ... \ Y_p \ ... \ Y_q \ ... Y_r$, $\text{follow}(Y_p)$ should contain
	- $\text{first}(Y_{p+1}Y_{p+2}\ ...\ Y_r)$
	- $\text{follow}(X)$ if $\text{nullable}(Y_{p+1}Y_{p+2}\ ...\ Y_r)$

<br>

## LL(1) Grammars
A grammar is **LL(1)** if for every non-terminal $X$:
- the $\text{first}$ sets of all alternatives of $X$ are *disjoint*
- at most one alternative of $X$ may be [[02_regular_expressions#Nullable|nullable]], and if $\text{nullable}(X)$ then the sets $\text{first}(X)$ and $\text{follow}(X)$ must be disjoint

**Example**: the following grammar is **not** LL(1):
```scala
stmtList ::= ε | stmt stmtList
stmt     ::= assign | block
assign   ::= ID = ID;
block    ::= beginof ID stmtList ID ends
```
because $\text{nullable}($`stmtList`$)$ but $\text{first}($`stmt`$) \cap \text{follow}($`stmtList`$) = \{$`ID`$\}$.

### Parsing Tables
A **parsing table** describes which alternative(s) to take given a choice of a non-terminal and a token: $N \times T \rightarrow \text{Set}[\text{Int}]$
If a given input produces an empty set, a **syntax error** should be reported.
If a given input produces a set with more than one element, the grammar **is not LL(1)**.

**Example**: slide 31