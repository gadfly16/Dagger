# Tree

Tree is a generic, variable length extensible binary encoding system of arbitrary number of arbitrary precision numbers organized into a tree structure. Dagger is a monotype system where everything is of type tree.

## Purpose

To be useful a computing system must be able to store values, to describe what those values are and to tell what to do with those values. In the context of Dagger we call these components *persistent storage*, *type system* and *programming*. One of the basic (not too) original idea of Dagger is that these tree capabilities are so interrelated that their implementation should be designed together. If we can design a system that's able to efficiently and reliably store values, tell what those values are and define programs by those values that manipulates (mostly) other values that would dramatically simplify sytem complexity. We intend to prove that such a system can not only be simple but extremely efficient as well.

Dagger's memory model is a tree of numbers. The tree consists of two types of nodes: 'tree' and 'number'. A number doesn't have children. Every node can have metadata. In a technical sense metadata is the 0th child of a node. In this sense even number nodes can have only one child, the metadata. Metadata can store the information about how to interpret the tree or number it refers to. For example with this mechanism a tree node can be interpreted as a string, which not only allow us to handle the numbers stored under the tree as text but also can instuct the storage system to store these numbers effectively.

We have only one number type which can express arbitrary size and arbitrary precision numbers. To make the system efficient we can limit our trees (or numbers) through interpretations. For example we can say that this tree holds only values of 0s and 1s, allowing the system to optimize its storage and computations.

## Number

Standard numbers are encoded by a variable length binary encoding.
