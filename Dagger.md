# Dagger

Dagger is a computing system designed from the ground up to be a fast and elegant self descripting integrated development environment for solving algorithmically solvable problems.

## Basic concepts

The basic idea behind Dagger is *using a computer is to program a computer*. The goal is to design an application where all the components needed for practical application are carefully designed to constitute a fast and coherent system that is exactly as complex as it needs to be. To achieve this goal we are limiting the base concepts to a sufficient minimum.

  - Memory Model
  - Type System
  - Serialization
  - Runtime model
  - User Interface

### The Tree

Dagger's memory model is a *labeled tree of numbers*. Everithing existing inside a Dagger system lives on the tree. Both data and program code is expressed and stored on this labeled tree of numbers. The tree is somewhat analogous to the concept of UNIX's filesystem, but it extends havily on that idea.

Not only everything lives on the tree but everyting *is* a tree from a programming perspective, which means that accessing things in the system is unifromized.

The main idea here is that all the data types that Dagger intends to provide can be expressed as a labeled tree. Single values, lists, structs, maps, functions (directed acyclic graphs) all can be interpreted as specialized forms of a labeled tree. In Dagger instead of saying that this thing is a *list of numbers* we constraint a tree to hold only plain numbers and not nodes. This allows the system to optimize that part of the system both during processing and during storage operations.

There are two kinds of nodes on the tree: trees and numbers. Trees can have both metadata and children while numbers can only have metadata and no other children.

### Type system

Dagger doesn't have a traditional type system instead it has constraints and interpretations defined in the node's metadata. We don't say that this value is an `int8` we say that this number node's value is constrained to the range expressible by `int8`. We don't say that this tree is a `string` but we say this tree is *intended* to be interpreted as a `string`. Interpreations may include their own constraints, for example in a string's case that the tree can only hold numbers as its children and their values is constrained to let any unicode rune to be selected.

Like trees in their structure, numbers are fairly general from a programming perspective they are arbitrary precision floating point numbers. Again, they can be practical because we can define constraints on their precision that allows the system optimize their processing and storage.

### Serialization

Dagger provides one standard way to serialize trees for both storage and communication purposes. Serialization is designed to store both values and metadata, which means that the serialized form contains interpretations and constraints. Persistent storage is simply a serialization of the tree that has all the information to recreate the tree exactly after a power cycle.

### Runtime Model

Program code is also stored on the tree. Functions are simply interpretations of the numbers stored in a tree. Functions can be executed in *goroutine-like* lightwieght threads.

In the case of functions a tree is interpreted as an AST. How the AST is presented is detached from the actual data structure. The user interface can present and edit them as text (possibly in more than one syntax) or graphically or any other mean that seems practical.

Since the programs and their data both lives on the tree we can manipulate code just like we can manipulate data. In a sense Dagger is a modern reinterpretation of the Von Neumann architecture.

The interesting aspect of the system is that it is a tool designed for the development of itself while running. Since every computer is a tool designed for the development of itself we can thingk of Dagger as an abstraction of the computer.

### User Interface

We can look at Dagger as a *fully integrated development environment*, where data, code, storage, compiler and the editor is all integrated into a coherent whole. This way the UI becomes an *universal application*. For example if you add functions to the system to interpret trees as images you can turn it into an image manupulation program.
