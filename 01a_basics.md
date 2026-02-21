# Definitions
A **language** can be natural, a computer language or mathematical statements.
- mathematically defined as a *set of strings* which are meaningful
- languages can be *processed*

See [[02_languages#Definition|here]] for a formal definition.

<br>

# Compilers
A compiler processes a Turing-complete programming language (**source code**) and translates it into efficiently executable **target code**

## Compilation Process
characters

&nbsp;  &nbsp;**lexical analysis** (see [[02_lexical_analyzers]])
 
tokens (~= words)

&nbsp;  &nbsp;**parsing** (= **syntax analysis**)

abstract syntax tree (see below)
 
&nbsp;  &nbsp;**name analysis**

graph where variables are mapped to declarations

&nbsp;  &nbsp;**type checking**

graph with added types 
 
&nbsp;  &nbsp;**intermediate code generation**

intermediate code
 
&nbsp;  &nbsp;**compilation**

target code

### Interpreters
Interpreters skip everything after parsing and skip directly to evaluation.

<br>

## Abstract Syntax Trees (AST)
Structure:
- **nodes**: arithmetic operations, statements, blocks
- **leaves**: constants, variables, methods

Representation: classes in OOP, algebraic data types in FP

### AST in Scala

Simple AST definition in Scala: #scala-example

```Scala
abstract class Expression
case class C(n: Int) extends Expression // constant
case class V(s: String) extends Expression // variable
case class Plus(e1: Expression, e2: Expression) extends Expression
case class Times(e1: Expression, e2: Expression) extends Expression

abstract class Statement
case class Assign(id:String, e:Expression) extends Statement
case class Block(s: List[Statement]) extends Statement

val program = Assign("res", Plus(C(14), Times(V("arg"),C(3))))
```
