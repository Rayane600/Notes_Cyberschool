# Types, terms and functions

## Types: Syntax

- *τ* ::= (τ)  
    | bool | nat | char | ... (base types)  
    | 'a | 'b | ... (type variables)  
    | τ ⇒ τ (functions)  
    | τ × ... × τ (tuples, * notation)  
    | τ list (lists)  
    | ... (user-defined types)
    
- **Operator** ⇒ is **right-associative**, e.g.,  
    `nat ⇒ nat ⇒ bool` is equivalent to `nat ⇒ (nat ⇒ bool)`.

---

## Typed Terms: Syntax
- **term** ::= (term)  
    | **a** (where **a** ∈ 𝔉 or **a** ∈ 𝔛) — function application  
    | **term term** — function application  
    | **λy. term** — function definition, where **y** ∈ 𝔛  
    | **(term, ..., term)** — tuples  
    | **[term, ..., term]** — lists  
    | **(term : : τ)** — type annotation  
    | ... — a lot of syntactic sugar
    
- **Function application** is **left-associative**.  
    For instance:  
    `f a b c` is equivalent to `((f a) b) c`.

## Semantics and Verification

- **To verify programs**, formal reasoning on their semantics is crucial!
- To prove a property **φ** on a program **P**, we need to **precisely** and **exactly** understand **P**'s behavior.
---
### Semantics given by the compiler:
For many languages, the semantics is given by the **compiler (version)**!
- Examples: C, Flash/ActionScript, JavaScript, Python, Ruby, ...
---
### Languages with a (written) formal semantics:
- **Java**, subsets of **C** (hundreds of pages)
- Proofs are hard due to semantic complexity (e.g., KeY for Java).
---
### Languages with a small formal semantics:
- **Functional languages**: Haskell, subsets of (OCaml, F#, Scala)
- Proofs are easier since semantics consists essentially of **a single rule**.
## Constructor Terms
- Isabelle distinguishes between **constructor** and **function** symbols:
    - A **function symbol** is associated with a (computable) function:
        - All predefined functions, e.g., `append`
        - All user-defined functions, e.g., `addNc` and `add` (see Exercise 2)
    - A **constructor symbol** is **not** associated with a function.
---
### Constructor Term Definition
- A term containing only constructor symbols is a **constructor term**.
- A **constructor term** does **not** contain function symbols.
- All **data** are built using **constructor terms** without variables, even if the representation is generally hidden by Isabelle/HOL.
/!\\ Constructor symbols have types even if they do not "compute."
#### Some functions on lists
Some functions from *Lists.thy*:
- `append :: 'a list ⇒ 'a list ⇒ 'a list`
- `rev :: 'a list ⇒ 'a list`
- `length :: 'a list ⇒ nat`
- `List.member :: 'a list ⇒ 'a ⇒ bool`
- `map :: ('a ⇒ 'b) ⇒ 'a list ⇒ 'b list`