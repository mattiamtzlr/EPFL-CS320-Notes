# (Non-)Deterministic Finite Automata
Most of this has already been covered in CS-251 "Theory of computation", thus it will not be covered again here.

#implementation 
**NFA in Scala**:
```scala
def δ(a: Char)(q: State): Set[States] = { ??? }

def δ_(a: Char, S: Set[States]): Set[States] = {
	for (q1 <- S, q2 <- δ(a)(q1)) yield q2 // S.flatMap(δ(a))
}

def accepts(input: MyStream[Char]): Boolean = {
	var S: Set[State] = Set(q0) // current set of states
	while (!input.EOF) {
		val a = input.current
		S = δ_(a,S) // next set of states
	}
	!(S.intersect(finalStates).isEmpty)
}
```

**Theorem**: A language $L$ can only be described by a [[02_regular_expressions#Definition|reg-ex]] if and only if there is a finite automaton that accepts $L$.

<br>

#math
## Epsilon Closure
The **epsilon closure** $E(q)$ describes all states reachable from $q$ through only epsilon transitions:
- $q \in E(q)$
- If $q_1 \in E(q)$ and $\delta(q_1, \ \varepsilon, \ q_2)$ then $q_2 \in E(q)$

<br>

## Determinisation: NFA into DFA
The general procedure to convert a nondeterministic finite automaton into a deterministic one is as follows:
- find all possible sets of states in which the the NFA could be, including the *epsilon closure*.
- use each of these sets as one state in the new DFA

*Formally*, the new DFA is $(\Sigma, 2^Q, q_0', \delta', F')$, where
- $q_0' = E(q_0)$ where $E$ is the epsilon closure
- $\delta'(q', a) = \bigcup_{\{\exists q_1 \in q', \ \delta(q_1 \ \varepsilon, \ q_2)\}}E(q_2)$
- $F' = \{q' \ | \ q' \in 2^Q, \ q' \cap F \neq \emptyset\}$

In addition, if there is no transition for some element $a$ in the alphabet, this is represented as a *sink state* in the DFA, i.e. a non-accepting state which has self-loops for all symbols.

<br>

## Relationship Between Automata and Derivatives
Reg-exs can be viewed as states of a deterministic automaton, for $a \in A$, $s' = \delta(s, a)$ corresponds to $s' = \partial_as$.

However, there can be an *infinite* number of different regular expressions for a language, thus the automaton is infinite. This can be remedied by using properties of regular expressions such as associativity and commutativity to identify identical reg-exs.

<br>

## Limitations
Finite automata are limited by their finiteness, as they for example cannot count.
If a string is too long, a DFA will repeat its behavior at some point. This is formalised by the **pumping lemma**:

If $L$ is a regular language, then there is a positive integer $p$ called the *pumping length*, such that every string $s \in L$, whose length is longer than $p$, can be partitioned into three pieces $s = x\, y\, z$, such that $x\, y^i z \in L \ \forall i \geq 0$, i.e. part $y$ can be repeated infinitely many times.
In addition, $|xy| \leq p$ and $|y| \geq 1$.

Because of these limitations we might prefer to use [[03b_grammars#Definition|grammars]] instead.