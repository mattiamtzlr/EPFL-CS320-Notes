#implementation 

In the following, the below context-free grammar is used in examples:
```scala
program ::= statmt*

statmt ::= println(stringConst, ident)
			| ident = expr
			| if (expr) statmt (else statmt)?
			| while (expr) statmt
			| { statmt* }
			
expr ::= intLit | ident
			| expr (&& | < | == | + | - | * | / | % ) expr
			| !expr | -expr
			
intLit ::= digit+
ident ::= (digit | letter)+
```

# Abstract Syntax Trees
In contrast to the [[03b_grammars#Parse Tree|parse tree]] where the leaves correspond precisely to the RHS of the grammar rules, the **abstract syntax tree** (**AST**) of a *grammar* contains only the useful information, stripping away for example punctuation symbols.

A *compiler* often builds the AST for a program directly instead of first building the parse tree and then converting it.

<br>

**Example**: parse tree vs. AST for the statement
```scala
while (x > 0) x = x + 1
```
*parse tree*
```
			program
			   |
			statmt
		   /  /| \ \____
		  /  / |  \     \
	  while ( expr )     statmt
			 / | \       |    \  \
		 expr  >  expr  ident  =  expr
		/          |              |  \  \
ident(x)       intLit(0)         expr - expr
								  |        \
								 ident(x)  intLit(1)

```

*AST*
```
				while
			   /     \
	 greaterThan     assign
	  /     \          |  \
ident(x)  intLit(0)    x   minus
						  /     \
					ident(x)  intLit(1)
```

<br>

## Ambiguous Grammars
If some token sequence has multiple parse trees (and thus multiple ASTs), the grammar is said to be **ambiguous**.

An example for this is the token sequence
```scala
ident + intLit / ident
```
which can be parsed into the following trees:
```
    add                       divide
   /   \                      /   \
ident  divide               add   ident
        /   \              /   \
    intLit  ident       ident  intLit
```

The right one is invalid following arithmetic priority rules.
This can be fixed by adjusting the grammar such that it's impossible to derive the right tree. Generally this is done by adding *more fine-grained rules and non-terminals*.

See slides 19 - 22 to see the fix for the above example.

<br>

## Recursive Descent Parsing
**Recursive Descent** parsing also known as **LL(1)** parsing is a pretty good parsing technique which can be easily implemented manually on the grammar and is *linear* in the size of the token sequence.

### Transforming the Grammar
To make LL(1) parsing work, we usually have to *rewrite the grammar* so that it is suitable for recursive parsing. This means there cannot be any ambiguity and all recursions have to have a base case.

The *rough idea* is that we transform a rule where the RHS has multiple alternatives (unions) into an if-else statement where the *first element* of each alternative is checked against. Then the corresponding alternative is recursively parsed.

**Remark**: by first element, we mean the '$\text{first}$' *predicate* defined on grammars as
$$\text{first}(B_1 ... B_p) = \{a \in \Sigma \ | \ B_1 ... B_p \ \Rightarrow\ \ ... \ \Rightarrow \ aw\}$$
for some word $w$, meaning it is possible to [[04_grammar_derivation|derive]] the sequence $B_1 ... B_p$ to $aw$.
The first elements for each case should be distinct sets of tokens.

### Abstracting into Constraints
After having found all the first elements, we can abstract the grammar into just these first elements as **constraints** for the RHS of each rule, reducing complexity.
For nice grammars, these will be non-recursive and can be *solved directly by substitution*.

If the grammar still contains recursion, we have to solve the constraints *iteratively* until they stabilize, which will be in the worst case when every RHS is the union of all tokens.
Below is an example of this:
![[iterative_constraint_solving.png]]