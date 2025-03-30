# Dagger Standard Textual Representation (STR)

The Standard Textual Representation (STR) is Dagger's human-readable syntax for representing its tree-based computational model. While Dagger uses a Standard Binary Representation (SBR) internally, STR provides the interface for viewing and editing tree structures textually.

## Formal Syntax (BNF)

```
<STR> ::= <node>

<node> ::= <number>
         | <name>
         | <tree>

<number> ::= [+-]?[0-9]+("."[0-9]+)?([eE][+-]?[0-9]+)?

<name> ::= [a-zA-Z_][a-zA-Z0-9_]*

<tree> ::= <tree-literal>
         | <call>
         | <path>
         | <eval-reference>
         | <value-reference>
         | <string>

<tree-literal> ::= "{" <whitespace>* <tree-content>? <whitespace>* "}"

<tree-content> ::= <labeled-node> (<whitespace>+ <labeled-node>)*
                 | <node> (<whitespace>+ <node>)*

<labeled-node> ::= <label-sequence> <node>

<label-sequence> ::= <label>+

<label> ::= <name>":" <whitespace>*
          | <name>":" <newline> <whitespace>*

<path> ::= <bracket-path>
         | <dot-path>

<bracket-path> ::= "[" <path-element> (<whitespace>+ <path-element>)* "]"

<path-element> ::= <name>
                 | <number-literal>
                 | <ancestor>

<dot-path> ::= <name> ("." <name>)*
             | <ancestor> ("." <name>)*

<ancestor> ::= "." <dot>*

<dot> ::= "."

<number-literal> ::= [0-9]+

<eval-reference> ::= "@" <path>

<value-reference> ::= "^" <path>

<call> ::= <name> <inline-arguments>? <newline>? <indented-arguments>?

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

<whitespace> ::= [ \t\n\r]

<newline> ::= "\n" | "\r\n"

<comment> ::= "//" <any-character-except-newline>* <newline>
```

## Comprehensive Example

The following example demonstrates the various syntactic elements of Dagger's STR that we've explicitly covered:

```
// A comprehensive example of Dagger's STR
data: {
  // Basic node types
  number_examples: {
    integer: 42
    negative: -17
    decimal: 3.14159
    scientific: 6.02e23
  }

  // String examples
  string_examples: {
    simple: "Hello, world!"
    escaped: "Quotes \"inside\" a string"
    multiline: "This string
spans multiple
lines"
  }

  // Tree with unlabeled nodes
  tags: { "important" "urgent" "critical" }

  // Trees with labeled nodes
  person: {
    name: "John Doe"
    age: 30
    contact: {
      email: "john@example.com"
      phone: "555-1234"
    }
  }

  // Multiple labels for the same node
  primary:
  main:
  first: {
    value: 100
    description: "Node with multiple labels"
  }

  // Metadata using the special _ label
  config: {
    _: {
      kind: "settings"
      version: 2
    }
    timeout: 30
    maxRetries: 3
  }

  // Path examples
  paths: {
    // Bracket notation (canonical form)
    canonical_path: [person name]
    with_index: [array 2]
    parent_ref: [.. number_examples]
    root_ref: [. string_examples]
    ancestor_ref: [... some_property]

    // Dot notation (sugar for name-only paths)
    dot_path: person.contact.email
    parent_dot: ..number_examples.integer
    root_dot: .data.person.age
  }

  // Array-like structure
  array: {
    0: "First"
    1: "Second"
    2: "Third"
  }

  // References
  references: {
    // Eval references - retrieve and evaluate
    eval_bracket: @[person age]
    eval_dot: @person.contact.phone
    eval_parent: @..array.1
    eval_root: @.data.config.timeout

    // Value references - retrieve without evaluation
    value_bracket: ^[paths canonical_path]
    value_dot: ^paths.dot_path
  }

  // Function calls
  calculations: {
    // Inline arguments
    sum: add 10 20

    // Indented arguments
    product:
      multiply
        5
        6

    // Mixed style with labeled arguments
    complex:
      calculate
        x: 15
        y: 7
        operation: "divide"

    // Using references as arguments
    reference_calc: add @[number_examples integer] @[number_examples decimal]

    // Nested function calls
    nested:
      process
        input: {
          value: 42
          type: "number"
        }
        options: {
          normalize: true
          scale: 2
        }
  }
}
```

This example demonstrates:
- Basic node types (numbers, names, trees)
- Strings with various formats
- Tree literals with labeled and unlabeled nodes
- Multiple labels for the same node
- Metadata using the special `_` label
- Paths using both bracket and dot notation
- References (both eval and value)
- Function calls in various forms (inline, indented, and mixed)
- Comments

The example showcases the syntax elements we've explicitly covered, providing a comprehensive illustration of Dagger's STR without introducing undefined concepts.
