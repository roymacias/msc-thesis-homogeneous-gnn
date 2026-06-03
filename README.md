# Graph Neural Networks for Homogeneous Graphs Using Message Passing

LaTeX source for a master's thesis introducing the *message passing* paradigm on
homogeneous graphs and implementing three canonical GNN architectures: GCN (node
classification, Cora), GAE (link prediction, Amazon Computers), and GIN (graph
classification, NCI1). This work is intended as a didactic and foundational
reference for the institute.

Typeset using the Tufte-Book LaTeX class.

## Repository Structure

```
.
├── chapters/            # One .tex file per chapter
├── figures/             # Source figures referenced in the document
├── files/               # Supporting files such as: preamble, title page, references.bib
├── tufte-book.cls       # Tufte-Book LaTeX class
├── thesis.tex           # Main LaTeX document
└── thesis.pdf           # Final document
```

## Style and Writing Conventions

Notation standards, writing guidelines, and figure
conventions are documented in [`STYLEGUIDE.md`](./STYLEGUIDE.md).
