# pi-lisp-cubical

A Visual Studio Code extension that adds language support for **pi-lisp-cubical** (`.pic`), a dependently-typed surface language with cubical type theory extensions: path types, interval expressions, Glue types, and higher inductive types.

---

## Features

### Syntax Highlighting

Full syntax highlighting for:

- **Declarations** — `def` (value definitions) and `data` (datatype declarations), with constructor names highlighted separately
- **Universes** — `U0`, `U1`, `U2`, …, and the `Type` alias
- **Keywords** — `fun`, `λ`, `Pi` / `Π`, `Sigma` / `Σ`, `fst`, `snd`, `elim`
- **Cubical primitives** — `transport`, `hcomp`, `Path`, `ua`, `Equiv`, `mkEquiv`, `equivFwd`, `Glue`, `glueElem`, `unglue`
- **Interval** — the interval type `I` / `𝕀`, endpoints `i0` / `i1`, and operators `/\` (`∧`), `\/` (`∨`), `~` (`¬`)
- **Operators** — `->`, `=>`, `@` (path application), `,` (pair), `.` (binder dot), `|` (constructor pipe)
- **Comments** — `--` line comments
- **Strings** — double-quoted string literals with escape sequences

### Bracket Matching & Auto-Closing

Matched pairs: `()`, `{}`, `[]`, `<>` / `⟨⟩` (path binders).  
Auto-closing is active for all bracket types and double-quoted strings, with `<>`/`⟨⟩` suppressed inside strings and comments to avoid conflicts with comparison operators.

### Smart Indentation

- Indentation increases after `{` and `data … =` lines.
- Pressing Enter on a `| constructor` line automatically pre-fills the next `| ` for ergonomic datatype authoring.

### Unicode Support

All ASCII operators have accepted Unicode aliases:

| Unicode | ASCII |
|---------|-------|
| `λ` | `\` (lambda) |
| `Π` | `Pi` |
| `Σ` | `Sigma` |
| `𝕀` | `I` (interval type) |
| `⟨` / `⟩` | `<` / `>` (path binder) |
| `×` | `*` (product) |
| `∧` | `/\` (meet) |
| `∨` | `\/` (join) |
| `¬` | `~` (negation) |

---

## Requirements

No external dependencies. The extension activates automatically for files with the `.pic` extension.

---

## Extension Settings

This extension does not contribute any configurable VS Code settings at this time.

---

## Known Issues

- `<` / `>` are highlighted as path binder punctuation; they may conflict visually when used in type-level comparisons if any are added to the language in future.
- Semantic highlighting (e.g. distinguishing local variables from constructors) is not yet implemented — the grammar resolves names syntactically only.

---

## Release Notes

### 1.0.0

Initial release. Includes:
- Syntax highlighting for all declarations, keywords, cubical primitives, interval expressions, and operators
- Unicode alias support
- Bracket matching and auto-closing for `()`, `{}`, `[]`, `<>`/`⟨⟩`
- Smart indentation for `data` declarations and `elim` case branches
- `--` line comment support

---

## Language Quick Reference

```
-- Value definition
def id : (A : U0) -> A -> A = \A x. x

-- Datatype with path constructor
data S1 =
  | base : S1
  | loop : S1 [ base , base ]

-- Path type and abstraction
def refl : (A : U0) -> (x : A) -> Path A x x =
  \A x. <i> x

-- Transport along a path
def subst : (A : U0) -> (B : A -> U0) -> (x y : A) -> Path A x y -> B x -> B y =
  \A B x y p bx. transport (<i> B (p @ i)) bx
```

For the full syntax reference see the [dedicated language document](syntaxes/dedicated_cubical_document.md).