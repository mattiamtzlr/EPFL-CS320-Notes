#math
# Definition
A **language** over an alphabet $A$ is a set $L \subseteq A^*$, thus it is a set of [[01b_words|words]].
- can be finite or empty
- can be infinite with structure or random

Languages support operations such as intersection and union.

<br>

# Properties and Operations 
The set of all languages $L \subseteq A^*$ is a **monoid** with neutral element $\{\varepsilon\}$, thus associativity holds.

<br>

## Concatenation
Given $L_1 \subseteq A^*$ and $L_2 \subseteq A^*$, their concatenation is
$$ L_1 \cdot L_2 = \{w_1w_2 \ | \ w_1 \in L_1, \ w_2 \in L_2\} $$

*examples*: slide 4

<br>

## Exponentiation
Given $L \subseteq A^*$, define
$$ L^0 = \{\varepsilon\} $$
$$ L^{n+1} = L \cdot L^n $$
where $L^n = \{w_1 ... w_n \ | \ w_1, \ ..., \ w_n \in L\}$

<br>

## Characteristic Function
In general it's not possible to represent languages in a program as they don't need to be recursively enumerable sets. The most powerful representation is a computable **characteristic function**.
$$ f_L : A^* \rightarrow \{0, 1\} \qquad f_L(w) = 1 \ \text{if} \ w \in L \ \text{else} \ 0 $$

<br>

#scala-example 
```scala
case class Lang[A](contains: List[A] -> Boolean)

def f(w: List[Int]): Boolean = w match {
	case Cons(0, Cons(1, Nil())) => true
	case Cons(0, Cons(1, wRest)) => f(wRest)
	case _ => false
}

val L2 = Lang(f)
val test = L2.contains(0::1::0::1::Nil()) // true
```

<br>

## Repetition (Kleene Star)
Given $L \subseteq A^*$, the **Kleene Star** is defined as
$$ L^* = \bigcup_{n\geq0} L^n $$

*Theorem*: $w \in A^*$ is an element of $L^*$ if and only if
$$\exists n \geq 0, \ \exists w_1, \ ..., \ w_n \in L \ \text{s.t.} \ w = w_1 ... w_n$$

$L^*$ has infinite cardinality except for $L = \{\varepsilon\}$.

*Observation*: $L^* = (L \setminus \{\varepsilon\})^*$ as concatenating with the empty word has no effect.