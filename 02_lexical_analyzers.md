#implementation

# Manual Lexers
While it is generally preferred to generate lexical analysers, or short *lexers*, automatically (see [[#Automatically Constructed Lexers]]), it can be done manually which is what this section focuses on.

We can convert simple [[02_regular_expressions|regular expressions]] into a lexer as described below:

![[regex_to_lexer.png]]
(this syntax is kinda weird)

<br>

## More Complex Cases
Often it is not sufficient to only *look ahead* a few upcoming characters to determine the following token.
Take for example integers and floats which both start the same, floats have a decimal point somewhere in the middle.

Thus we have to process and store characters until we have enough information to decide on the current token.

A partial implementation of a simple manual lexer is given below.

#scala-example 
```scala
ch.current match {
	case '(' => {current = OPAREN; ch.next; return}
	case ')' => {current = CPAREN; ch.next; return}
	case '+' => {current = PLUS; ch.next; return}
	case '/' => {current = DIV; ch.next; return}
	case '*' => {current = MUL; ch.next; return}
	
	case '=' => { // more tricky because there can be =, ==
		ch.next
		if (ch.current == '=') {ch.next; current = CompareEQ; return}
		else {current = AssignEQ; return}
	}
	
	case '<' => { // more tricky because there can be <, <=
		ch.next
		if (ch.current == '=') {ch.next; current = LEQ; return}
		else {current = LESS; return}
	}
}
```

<br>

## Whitespace and Comments
By convention, the lexer *removes* whitespace and comments from its output.

This could be done with the following implementation for example:

#scala-example 
```scala
if (ch.current='/') {
	ch.next
	if (ch.current='/') {
		while (!isEOL && !isEOF)
			ch.next
	} else {
		ch.current = DIV
	}
}
```
One should also include a counter to allow for nesting comments.

<br>

## Longest Match Rule
Also known as the *maximal munch rule* which is fucking hilarious. 

A lexer should always be *greedy*, as to always get the longest possible token. Otherwise there would be ambiguity in how the input should be split into tokens.

### Example
Consider the language with the only tokens being
- `ID`: an alphanumerical identifier
- `LE`: the operator `<=`
- `LT`: the operator `<`
- `EQ`: the operator `=`

We want to analyse the following input:
```
interpreters <= compilers
```
If our lexer doesn't follow the maximal munch rule, it could incorrectly split the input into
```
ID(inter) ID(preters) LT EQ ID(compilers)
```
which is clearly not intended.

### Limitations
In case where the lexer fails to identify a token because of the maximal munch rule, it is expected to report an *unknown token error*.

We must group and order all tokens by **priority** as there could be overlaps between the groups, e.g. for keywords which are identifiers and constants which could be integers or floats.
In case of an overlap, the lexer should choose the token with higher priority.
These priorities must be specified in the language definition.

<br>

# Automatically Constructed Lexers
In general, tools to automatically generate a lexer require us to only specify tokens using regular expressions, from which the lexer is then generated.

This can be done either as access to a library at runtime or by pre-generating the lexer and then including it in the compilation.

<br>

## Language derivatives
To automate lexers, it is useful to use the **derivative** of a [[02_languages#Definition|language]] $L \subseteq A^*$ which is defined as follows for any letter $x \in A$:
$$\partial_xL = \{w \in A^* \ | \ xw \in L\}$$
Thus, the derivative $\partial_x$ ignores all words that don't begin with the letter $x$ and collects the *suffixes* of those that do.

### Language Decomposition
This lets us decompose $L$ into disjoint sets, for example if $A = \{a,\ b\}$, we get:
$$L = (a\partial_aL) \ \cup \ (b\partial_bL) \ \cup \ \{\varepsilon \ | \ \text{nullable}(L)\}$$
where the last part is needed as derivatives ignore the empty string if it is present, i.e. if $L$ is nullable.