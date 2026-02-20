# Definitions
An **alphabet** $A$ is a finite non-empty set of letters

---

A **word** $w$ or **string** is a finite sequence of letters in $A$.
Thus, $w \in A^*$, where $A^*$ is the set of all finite sequences of elements in $A$, including the empty sequence $\varepsilon$.
- The words of length $n$ are denoted $A^n$.
- $A^0 = \{\varepsilon\}$
- For $n > 0$, $A^n = \{f \ | \ f : \{0, \ ..., \ n - 1\} \rightarrow A\}$ which is a function describing what letters there are and in which order.
<br>
- Words are *equal* if they have the same lengths and the same letters in the same order.

---

A **language** $L$ is a set of words, possibly empty, possibly infinite, $L \subseteq A^*$

<br>

# More on Words
Inductive representation in Scala: #scala-example 
```scala
sealed abstract class List[A] { // A is the alphabet
	def ::(t:A): List[A] = Cons(t, this)
	
	def length: BigInt = this match {
		case Nil() => BigInt(0)
		case Cons(h, t) => 1 + t.length
	}

	def apply(index: BigInt): A = {
		this match {
			case Cons(h,t) =>
				if (index == BigInt(0)) h
				else t(index-1)
			}
		}
	}
	
case class Nil[A]() extends List[A]
case class Cons[A](h: A, t: List[A]) extends List[A]

val w = 1 :: 0 :: 1 :: 1 :: Nil[Int]() // 1011
val n = w.length // 4
val z = w(1) // 0
```


## Properties and Operations
- Words are finite, the set of all words is countably infinite.
- The **concatenation** of words $u$ and $v$ is denoted $u \cdot v$ or $uv$ for short.

<br>

- The structure $\left(A^*, \ \cdot, \ \varepsilon\right)$ is an algebraic monoid, called the *free monoid*. It satisfies the left and right cancellation laws:
	- $wu = wv \ \Leftrightarrow \ u = v$
	- $uw = vw \ \Leftrightarrow \ u = v$

<br>

- The **reversal** of a word is denoted $w^{-1}$.
	- $\varepsilon^{-1} = \varepsilon$
	- $a^{-1} = a$ for all $a \in A^1$
	- $(uv)^{-1} = v^{-1}u^{-1}$

<br>

- A **$[p, q)$-slice** of $w$ is the word $u$ containing the letters of $w$ between positions $p$ and $q$ exclusive.
