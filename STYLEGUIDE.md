# STYLEGUIDE

Writing conventions, typographic rules, mathematical notation standards, and
LaTeX usage guidelines for this thesis. Apply these rules consistently across
all chapters. Exceptions arising from specific content are handled at the
chapter level and documented inline with a comment.

---

## 1. Chapter Structure

Every chapter file follows this opening template:

```latex
\chapter{Chapter Title}
\label{ch:short-name}
\minitoc
```

`\minitoc` generates the per-chapter mini table of contents and must appear
immediately after `\chapter{}`. Omitting it breaks the minitoc layout.

Heading depth: maximum two levels — `\section` and `\subsection` only.
`\subsubsection` and deeper trigger a compilation error in `tufte-book`.

Heading numbering is enabled up to subsection depth via
`\setcounter{secnumdepth}{2}` in the preamble. Do not modify this counter.

Heading colors follow the institutional palette defined in the preamble
(ITESO blue scale). Do not override chapter, section, or subsection formatting.

---

## 2. Typography and Emphasis

| Use case | Command |
|---|---|
| First mention of a technical term or concept | `\textit{term}` |
| Contextual distinction of a word | `\textit{word}` |
| All inline math symbols and variables | `$...$` |

Do not use `\emph{}` for technical terms — use `\textit{}` explicitly.

Latin abbreviations use the predefined preamble macros:

- `\ie` → *i.e.*
- `\eg` → *e.g.*

Every mathematical symbol appearing in running text must be in math mode,
even single-letter variables: write `$x$`, never plain `x`.

When a mathematical object is introduced for the first time, state its type and role explicitly in the surrounding text immediately before or after the equation. Previously defined objects need not be reintroduced.

### 2.1 Acronyms and Abbreviations

An acronym is declared in text at the point where it first enters sustained
use: the full term is written out, immediately followed by the acronym in
parentheses, and from that point onward only the acronym is used. The
declaration is performative — it is placed where the short form actually takes
over, not merely where the term is first mentioned.

Concretely:

- **GNN** is declared in `\ref{ch:applications}` (Chapter 1); thereafter "graph
  neural network" is never written in full — only GNN.
- **MLP**, **CNN**, and **RNN** are declared together in
  `\ref{ch:inductive-bias}` (Chapter 5), at the point where the three
  architectures are introduced as a family to contrast their inductive biases.
  `\ref{ch:nn-limitations}` (Chapter 4), which develops the multilayer
  perceptron before that declaration, writes the term in full throughout and
  does not use the acronym.
- **GCN**, **GAE**, and **GIN** are each declared at their point of
  introduction in the architecture and implementation chapters, and used only
  in acronym form thereafter.

A chapter that develops a concept in full prose before its acronym is declared
retains the full term throughout; the acronym does not appear retroactively.
The declaration governs forward, never backward.

Also do not use `\newthought{}`.

---

## 3. Mathematical Notation

### 3.1 Base Types

| Object | Convention | LaTeX command | Examples |
|---|---|---|---|
| Scalar | Lowercase italic | `$x$` | $x$, $\alpha$, $\lambda$ |
| Vector | Lowercase bold roman | `$\mathbf{x}$` | $\mathbf{x}$, $\mathbf{h}$ |
| Matrix | Uppercase bold roman | `$\mathbf{A}$` | $\mathbf{A}$, $\mathbf{W}$ |

All vectors are column vectors by default unless explicitly stated otherwise.

### 3.2 Operations and Special Objects

| Notation | Meaning | LaTeX |
|---|---|---|
| $\mathbf{x}^\top$ | Transpose (vector or matrix) | `$\mathbf{x}^\top$` |
| $A_{ij}$ | Element $(i,j)$ of matrix $\mathbf{A}$ | `$A_{ij}$` |
| $x_i$ | $i$-th element of vector $\mathbf{x}$ | `$x_i$` |
| $\mathbf{I}$ | Identity matrix | `$\mathbf{I}$` or `$\mathbf{I}_n$` |
| $\mathbf{1}$ | Column vector of all ones | `$\mathbf{1}$` |
| $\mathbf{a} \oplus \mathbf{b}$ | Concatenation of vectors | `$\mathbf{a} \oplus \mathbf{b}$` |
| $k$ | Layer index (not $l$ or $\ell$) | `$k$` |

---

## 4. Equations

Relations, definitions, and properties — anything later text, equations, or
chapters may reference — use the `equation` environment, numbered and
referenceable. Concrete instances and worked illustrations (e.g., the entries
of a specific matrix for an example graph) use an unnumbered display,
`\[ ... \]` or `equation*`, with no `\label`: they illustrate rather than
state, and nothing refers back to them by number.

```latex
\begin{equation}
  \mathbf{h}^{(k+1)}_v = \sigma\!\left(
    \mathbf{W}^{(k)} \sum_{u \in \mathcal{N}(v)} \mathbf{h}^{(k)}_u
  \right)
  \label{eq:gcn-update}
\end{equation}
```

For multi-line derivations, use `align`. Apply `\notag` to intermediate lines
that do not require a number; label only key results.

```latex
\begin{align}
  \mathbf{H}^{(k+1)}
    &= \sigma\!\left(
         \tilde{\mathbf{D}}^{-1/2} \tilde{\mathbf{A}}
         \tilde{\mathbf{D}}^{-1/2} \mathbf{H}^{(k)} \mathbf{W}^{(k)}
       \right) \notag \\
    &= \sigma\!\left(
         \hat{\mathbf{A}} \mathbf{H}^{(k)} \mathbf{W}^{(k)}
       \right)
  \label{eq:gcn-matrix}
\end{align}
```

Cross-reference equations with `\eqref{}`, which automatically adds
parentheses:

```latex
As shown in~\eqref{eq:gcn-update}, the update rule...
```

Numbering scope: equations are numbered globally by default — `(1)`,
`(2)`, ... — and figures and tables are numbered per chapter (`Figure 3.1`,
`Table 3.1`) via the underlying `book` class.

---

## 5. Margin Notes

Two distinct commands with different rendering. Use them precisely:

| Command | Renders as | Use for |
|---|---|---|
| `\marginnote{text}` | No number, no superscript in body | Ancillary notes, clarifications |
| `\cite{key}` | Numbered superscript + sidenote | All bibliographic citations |

Do not use `\sidenote{}` — it adds an unwanted superscript number.  
Do not use `\footnote{}` — it auto-converts to a numbered sidenote.

Offset: omit by default. Add only if notes overlap after compilation.

---

## 6. Figures

Three environments, ordered by rendered width:

| Environment | Width | Default? |
|---|---|---|
| `marginfigure` | Margin column only | No |
| `figure` | Main text block | Yes |
| `figure*` | Full page width (text + margin) | No |

All figure sources live in `figures/`. The preamble sets
`\graphicspath{{figures/}}`, so filenames are referenced without path prefix:

```latex
\begin{figure}
  \includegraphics{gcn-architecture}
  \caption[GCN layer architecture.]{%
    GCN layer architecture. Node features are aggregated from the
    one-hop neighborhood and transformed by a learnable weight
    matrix $\mathbf{W}^{(k)}$.
  }
  \label{fig:gcn-architecture}
\end{figure}
```

Caption form: always `\caption[Short title.]{Long descriptive caption.}`.
The short title always ends with a period. It appears in the List of
Figures; the long caption appears in the document body.

Caption offset: omit by default. Add only if alignment issues arise
after compilation.

Caption position: below the graphic.

---

## 7. Tables

Use `booktabs` (already loaded in preamble). No vertical rules.

| Environment | Width | Default? |
|---|---|---|
| `margintable` | Margin column only | No |
| `table` | Main text block | Yes |
| `table*` | Full page width | No |

```latex
\begin{table}
  \caption[Dataset statistics.]{%
    Statistics for the three experimental benchmarks.
  }
  \label{tab:dataset-stats}
  \begin{center}
    \small
    \begin{tabular}{lccc}
      \toprule
      Dataset          & Nodes  & Edges   & Classes \\
      \midrule
      Cora             & 2{,}708  & 5{,}429   & 7  \\
      Amazon Computers & 13{,}752 & 491{,}722 & 10 \\
      NCI1             & 4{,}110 graphs & ---  & 2  \\
      \bottomrule
    \end{tabular}
  \end{center}
\end{table}
```

Caption form: same `\caption[short.]{long}` pattern as figures, including
the final period in the short title.

Caption offset: same offset configuration as figures.

Caption position: above the tabular (opposite of figures).

---

## 8. Citations and Bibliography

Cite with `\cite{key}` anywhere in the text. The Tufte class automatically
renders the reference as a numbered sidenote at the citation point.

```latex
The GCN architecture~\cite{Kipf2017} defines...
```

Multiple citations: `\cite{key1,key2}`.

Bibliography file: `files/references.bib`. Add every new reference to this
file before compilation. Build style: `plainnat`.

### 8.1 Required Fields and Locators
 
Each entry carries the fields that are **necessary and sufficient** to identify
the work without ambiguity, and no others. The governing rule is not "every
entry of a given type carries the same fields," but "the same rule is applied
to every entry": each work brings exactly the fields its own nature requires.
A monograph with no series and a single relevant edition needs only
`author`, `title`, `publisher`, `year`; a numbered volume in a series needs
`series`, `volume`, and, where multiple editions exist, `edition`. Fields that
resolve no ambiguity (for example `address` on a book already fixed by
publisher and year) are omitted.
 
**Locators.** Every entry carries exactly one locator, in the form its
publication assigns. A second locator is never added when the first is present.
 
- **Articles and conference papers.** The locator is the `doi`. If the work has
  no DOI (for example, ICLR and other proceedings that assign none), the
  locator is a stable identifier given in `note` — typically the arXiv URL via
  `\url{...}`. If neither exists, the locator is dropped and the work is
  identified by its remaining necessary-and-sufficient fields (`booktitle`,
  `year`, `pages`). Page numbers and DOIs are never invented to match the shape
  of another entry: a venue without paginated proceedings simply has no
  `pages`.
- **Books.** The locator is the `isbn`, mandatory whenever the work has one
  (the Tufte template prints it). When a work carries several ISBNs — hardcover,
  paperback, e-book, or distinct editions — use the **hardcover ISBN of the
  edition cited**; if no hardcover exists, use the ISBN of the edition actually
  consulted. A book without any ISBN (older proceedings, technical reports)
  drops the field and is identified by its remaining fields. ISBN is the
  primary locator for books even when the publisher also assigns a DOI; the DOI
  is not added alongside it.
The asymmetry between an article carrying a DOI and a book carrying an ISBN is
intended: each work bears the locator its own medium defines. Uniformity is in
the rule applied, not in the fields displayed.

---

## 9. Cross-References and Labels

Label naming convention — prefix:kebab-case-name:

```latex
\label{ch:foundations}        % chapter
\label{sec:message-passing}   % section
\label{fig:gcn-architecture}  % figure
\label{tab:dataset-stats}     % table
\label{eq:gcn-update}         % equation
```

Reference commands:

```latex
Chapter~\ref{ch:foundations}
Section~\ref{sec:message-passing}
Figure~\ref{fig:gcn-architecture}
Table~\ref{tab:dataset-stats}
\eqref{eq:gcn-update}         
```

Always use a non-breaking space (`~`) between the label word and `\ref{}`
to prevent a line break.

---

## 10. Preamble Cleanup Notes

The following items in `files/preamble.tex` are inherited from the Tufte
template documentation and are not required for the thesis:

- Tufte book title shortcuts such as: `\VDQI`, `\EI`,
  `\VE`, `\BE`, `\TL`
- Documentation typesetting macros such as: `\doccmd`, `\doccmddef`,
  `\docenv`, `\docenvdef`, `\docpkg`, `\doccls`, `\docclsopt`, `\docclsoptdef`

These do not affect compilation and can be left in place until the thesis
is finalized.