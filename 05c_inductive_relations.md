#math 

These notes give the theoretical background required to understand type systems and their soundness.

# Inductively Defined Relations
An **inductively defined relation** (IDR) $r$ is a mathematical relation defined by **rules** of the form
$$\frac{t_1(\mathbf x) \in r, \ ..., \ t_n(\mathbf x) \in r}{t(\mathbf x) \in r}$$

where $t_i(\mathbf x)$ area assumptions and $t(\mathbf x)$ is the conclusion.

> [!cite] Viktor Kunčak
> A rule without assumptions is an axiom

**Example**: slide 16 (7)

<br>

## Context-Free Grammars as IDRs
We can describe a [[03b_grammars|context-free grammar]] using inductively define relations by treating each non-terminal as a set of strings.

**Example**:
![[cfg_to_idr.png]]

A [[04a_grammar_derivation|derivation tree]] of the grammar then has nodes marked by tuples $t(\mathbf a)$ where $\mathbf x$ has taken some specific value $\mathbf a$. We then define the relation $r$ as the set of all tuples for which there exists a *derivation tree*.
