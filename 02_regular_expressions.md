#math
# Definition
**Regular expressions** are mathematical expressions used to denote finite and infinite [[02_languages|languages]].
A regular expression over the language $L \subseteq A^*$ is built inductively from the following base structures:
- $\emptyset$ => the **empty set** of strings,
- $\varepsilon$ => $\{\varepsilon\}$, the language containing only the **empty word**,
- $a$ => $\{a\}$ for $a \in A$, the language of only one word of length one,

and the following inductive structures:
- $r_1 | r_2$ => the union of languages $r_1$ and $r_2$,
- $r_1r_2$ => the concatenation of languages $r_1$ and $r_2$,
- $r*$ => the [[02_languages#Repetition (Kleene Star)|Kleene star]] of language $r$

If $e$ is a regular expression, then the language described by it is denoted $L(e)$.

<br>

## Examples
- $(a|ab)*$ denotes the language $\{a, \ ab\}*$
- $(a|b|c) \ (a|b|c|0|1)*$ denotes $\{a, \ b, \ c\}\{a, \ b, \ c, \ 0, \ 1\}*$ which are all strings starting with any of the letters $a$, $b$ or $c$, followed by any sequence of those letters or the digits $0$ and $1$.

These can be used for example in *grep* with the `-E` flag.

<br>

## Functions
For a lack of a better word I refer to these as functions, even though *attributes* might be better.

### Nullable
For any language $L \subseteq A^*$ we define the function `nullable` as
$$\text{nullable}(L) \ \mathrel{\mathop:}= \ \varepsilon \in L$$
i.e. $\text{nullable}(L)$ = `true` if the *language contains the empty string*.

For a *regular expression* $e$, we define $\text{nullable}(e) \ \mathrel{\mathop:}= \text{nullable}(L(e))$, and we get the following inductive rules:
- $\text{nullable}(e_1 | e_2) = \text{nullable}(e_1) \ \lor \ \text{nullable}(e_2)$
- $\text{nullable}(e_1 e_2) = \text{nullable}(e_1) \ \land \ \text{nullable}(e_2)$
- $\text{nullable}(e*) = \text{true}$

### First
For any language $L \subseteq A^*$ we define the function `first` as
$$ \text{first}(L) = \{a \in A \ | \ \exists v \in A^* \ s.t. \ av \in L\}$$
i.e. we get *the set of first letters* for all words in $L$.

Again, for a *regular expression* $e$, we define $\text{first}(e) \ \mathrel{\mathop:}= \text{first}(L(e))$, and we get the following inductive rules:
- $\text{first}(e_1 | e_2) = \text{first}(e_1) \ \cup \ \text{first}(e_2)$
  <br>
- $\text{first}(e_1 e_2) = \text{if} \ \ \text{nullable}(e_1) \ \ \text{then} \ \ [\text{first}(e_1) \ \cup \ \text{first}(e_2)] \ \ \text{else} \ \ \text{first}(e_1)$
  as the word might begin with an element of $e_2$ if $e_1$ is nullable
  <br>
- $\text{first}(e*) = \text{first}(e)$
  as any element in $e*$ will always begin with an element in $e$.