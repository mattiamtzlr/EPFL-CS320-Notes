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