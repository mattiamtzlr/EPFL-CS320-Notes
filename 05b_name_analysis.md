#implementation

# Name Analysis
A **name analyser** takes in the AST from the [[05a_ll1_parsing|parser]] and adds mappings between variables and declarations.
It throws *unknown identifier* errors if it finds a variable name which has no declaration.

In addition, name analysis can report for example:
- undeclared identifiers
- redefined identifiers, functions parameters
- ill-formed type definitions (e.g. circular)

<br>

## Symbol Tables
To avoid searching the entire tree at each occurrence of an identifier, maintain a map from identifiers to declaration information (= **symbol**) at each point in the tree. This is the **symbol table** which can be cached or even integrated into the tree.

The symbol table may include information to give meaning to the following scenarios:
- declaration of a *value or variable* typically introduces its type and initial value
- variable inside a *pattern* introduces the name for part of the value in pattern matching
- definition of a *function* specifies its *signature* (arguments and their types) and its body
- definition of an *algebraic data type* (e.g., case class) specifies its alternatives and fields of each alternative

<br>

## Scope and Scoping Rules
In this class, we only consider **static (lexical) scoping**, used in many compiled languages. When we introduce an identifier, scoping tells us where it is visible, e.g. only inside the function body for a parameter.

Scoping rules specify the correct memory location or definition for an identifier.
