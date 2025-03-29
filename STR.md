# Dagger Standard Textual Representation (STR)

## Introduction

The Standard Textual Representation (STR) is the human-readable syntax for Dagger's tree-based computational model. While Dagger uses a Standard Binary Representation (SBR) internally, STR provides an interface for viewing and editing tree structures in a textual format.

STR unifies the representation of both data and code as trees, reflecting Dagger's fundamental philosophy of treating all computational elements as part of a single coherent structure. The syntax is designed to be concise yet expressive, enabling direct manipulation of Dagger's tree structures while remaining readable for human users.

## Core Concepts

1. **Single Node Program**: An STR program describes a single node, as Dagger's model does not support multiple nodes without a parent.

2. **Tree Structure**: All entities are represented in a unified tree structure with labeled branches.

3. **References and Dereferences**: Names resolve to indices in the tree, while dereferences retrieve values at those locations.

4. **Metadata**: Metadata is a first-class part of the tree, accessible via the special `_` label.

5. **Function Calls**: Function calls use indentation-based delimitation rather than explicit terminators.

6. **Scoping**: Names resolve to the closest definition found upstream in the tree hierarchy.

## Formal Syntax (BNF)

```
<program> ::= <node>

<node> ::= <number>
         | <name>
         | <labeled-node>
         | <tree>
         | <dereference>
         | <function-call>
         | <string>

<number> ::= [+-]?[0-9]+("."[0-9]+)?([eE][+-]?[0-9]+)?

<name> ::= [a-zA-Z_][a-zA-Z0-9_]*

<labeled-node> ::= <label-sequence> <node>

<label-sequence> ::= <label>+

<label> ::= <name>":" <whitespace>*
          | <name>":" <newline> <whitespace>*

<tree> ::= "{" <whitespace>* <tree-content>? <whitespace>* "}"

<tree-content> ::= <node> (<whitespace>+ <node>)*

<whitespace> ::= [ \t\n\r]

<newline> ::= "\n" | "\r\n"

<reference> ::= <name>
              | <reference>"/"<name>
              | <reference>"/"<number-literal>
              | "."
              | "."<reference>

<number-literal> ::= [0-9]+

<dereference> ::= "@"<reference>

<function-call> ::= <name> <inline-arguments>? <newline>? <indented-arguments>?

<inline-arguments> ::= <whitespace>+ <function-argument> (<whitespace>+ <function-argument>)*

<indented-arguments> ::= <indented-argument>+

<indented-argument> ::= <indent> <function-argument> <newline>?

<indent> ::= <whitespace-no-newline>+

<whitespace-no-newline> ::= [ \t]+

<function-argument> ::= <labeled-node>
                      | <node>

<string> ::= "\"" <string-character>* "\""

<string-character> ::= <any-unicode-character-except-quote-or-backslash>
                     | "\\" <escape-sequence>

<escape-sequence> ::= "\"" | "\\" | "n" | "r" | "t" | "b" | "f"
                    | "u" <hex-digit> <hex-digit> <hex-digit> <hex-digit>

<hex-digit> ::= [0-9a-fA-F]

<comment> ::= "//" <any-character-except-newline>* <newline>
```

## Syntactic Decisions

### Multiple Labels

STR allows multiple labels to refer to the same node:

```
x: y: z: 123

longLabel:
anotherLabel:
    add 1 2
```

Each label in the sequence creates a reference to the same node, enabling flexibility in naming and organization.

### Indentation-Based Function Call Termination

Function calls are terminated by indentation rather than explicit delimiters:

```
add 1 2             // Inline arguments
multiply            // Function name followed by indented arguments
    3
    4
```

A function call terminates when:
- A line with indentation less than or equal to the function name's indentation is encountered
- End of file is reached (in case the STR only describes a single function call)

### Label-Value Decoupling

Labels can be placed on lines preceding the value they label, with arbitrary whitespace (including newlines) between the colon and the value:

```
myVeryLongLabel:
    123

multiLine:
    {
      1
      2
      3
    }
```

This allows for more flexible formatting, especially with long labels or complex values.

### Trees and Whitespace

Within trees, whitespace separates siblings:

```
{ 1 2 3 }           // Compact form
{                   // Expanded form
  1
  2
  3
}
```

Both forms are semantically identical.

### Metadata

Metadata is accessed via the special `_` label:

```
value: {
  _: { kind: int8 }
  42
}
```

### Scope and References

Names resolve to the closest definition upstream in the tree. Dereferences with `@` retrieve values:

```
x: 5
y: @x              // Contains 5
z: {
  x: 10
  value: @x        // Contains 10 (closest upstream x)
  outer: @../x     // Contains 5 (explicitly references outer x)
}
```

## Examples

### Data Structure
```
person: {
  name: "John Doe"
  age: 30
  address: {
    street: "123 Main St"
    city: "Anytown"
  }
}
```

### Function Call
```
calculate
  x: 10
  y: 20
  operation: "multiply"
```

### Complex Structure with Multiple Labels
```
data:
results:
  {
    _: { kind: record }
    values: {
      first: 10
      second: 20
    }
    summary:
      calculate
        @data/values/first
        @data/values/second
  }
```

This STR syntax provides a clean, flexible way to view and edit Dagger's tree structures while maintaining the unified conceptual model that makes Dagger powerful.

END OF STR DOC