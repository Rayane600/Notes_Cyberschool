# Propositional and first order logic


## Propositional Logic 

- **Propositional formula** -> Let P be a set of propositional variable. the set of propositional formula is defined by : 
$$ \phi ::= p \mid \neg \phi \mid \phi_1 \lor \phi_2 \mid \phi_1 \land \phi_2 \mid \phi_1 \to \phi_2 \quad \text{where} \quad p \in P
$$

- **Propositional model** -> I is a model of $\phi$ , denoted by $I \models \phi$  if $I  [[ \phi]] = True$ 
- **Valid formula** -> A formula $\phi$ is valid, denote by $\models\phi$ , if for all I we have $I \models \phi$
____
### Tools in isabelle/HOL
- To automatically prove that $\models \phi$ we use `apply auto`
- To build I such as $I \not \models \phi$ ($I \models \neg \phi$) we use `nitpick`
- To close a proof `done`
- To abandon the proof of an unprovable formula `oops`
- to abandon the proof of a (potentially) provable formula `sorry`
- To evaluate a term t in Isabelle `value "t"`

| Symbol | ASCII notation |
| ------ | -------------- |
| True   | True           |
| False  | False          |
| ∧      | /\|            |
| ∨      | /              |
| ¬      | ~              |
| ≠      | !=             |
| →      | -->            |
| ↔      | =              |
| ∀      | ALL            |
| ∃      | ?              |
| λ      | %              |
___
## First order logic
- **Term** -> Let $\mathcal{F}$ be a set of symbols and ar a function such that $ar : \mathcal{F} \to \mathbb{N}$ associating each symbol of $\mathcal{F}$ to its arity (the number of parameters).**Let $\mathcal{X}$ be a variable set**. The set $\mathcal{T}(\mathcal{F}, \mathcal{X})$, the set of terms built on $\mathcal{F}$ and $\mathcal{X}$, is defined by:
$$
\mathcal{T}(\mathcal{F}, \mathcal{X}) = \mathcal{X} \cup \{ f(t_1, \dots, t_n) \mid ar(f) = n \text{ and } t_1, \dots, t_n \in \mathcal{T}(\mathcal{F}, \mathcal{X}) \}.
$$
- **Formulas** -> Let $\mathcal{P}$ be a set of predicate symbols all having an arity, i.e. $ar : \mathcal{P} \to \mathbb{N}$.       The set of formulas defined on $\mathcal{F}, \mathcal{X}$, and $\mathcal{P}$ is:
$$
\phi ::= \neg \phi \mid \phi_1 \lor \phi_2 \mid \phi_1 \land \phi_2 \mid \phi_1 \longrightarrow \phi_2 \mid \forall x.\phi \mid \exists x.\phi \mid p(t_1, \dots, t_n)
$$  
  where $t_1, \dots, t_n \in \mathcal{T}(\mathcal{F}, \mathcal{X})$, $x \in \mathcal{X}$, $p \in \mathcal{P}$, and $ar(p) = n$.c
  
### Anonymous Functions in Lambda Calculus
#### Classical Notation for Functions:

- Function `f : ℕ × ℕ → ℕ`
    - Example: `f(x, y) = x + y`
    - Can be simplified as `f : ℕ² → ℕ`
    - Short form: `f(x, y) = x + y`

#### Lambda Notation of Functions:

- Function `f : ℕ² → ℕ` using Lambda notation:
    - `f = λ(x, y). x + y`

#### Example of an Anonymous Function:

- `λx y. x + y` is an anonymous function that adds two natural numbers.

#### Corresponding Syntax in Other Languages:

- **OCaml/Why3**:  
    `fun x y -> x + y`
    
- **Scala**:  
    `(x: Int, y: Int) => x + y`
### Lambda Calculus in Isabelle/HOL

#### Function Update in Isabelle/HOL:
- Isabelle/HOL uses function updates with `(:=)` as in:
    - `(λx. x)(0 := 1, 1 := 2)`
        - The identity function except for 0 (mapped to 1) and 1 (mapped to 2).
    - `(λx. _)(a := b)`
        - A function that takes one parameter and whose result is unspecified except for value `a`, which is mapped to `b`.
#### Predicates in Isabelle/HOL:
- A predicate is a function mapping values to `{True, False}`.
- Example of a predicate `p` on `{a, b}`:
    - `p = (λx. _)(a := False, b := False)`
___
### First order interpretation
#### First-Order Interpretation:
- Let **φ** be a formula and **𝔻** a domain.
- An interpretation **𝕀** of **φ** on domain **𝔻** associates:
    - A function **fᵢ**: **𝔻ⁿ → 𝔻** to each symbol **f** ∈ **F**, where `ar(f) = n`.
    - A function **pᵢ**: **𝔻ⁿ → {True, False}** to each predicate symbol **p** ∈ **P**, where `ar(p) = n`.
#### Valuation:
- Let **𝔻** be a domain,a **valuation** **𝕍** is a function **𝕍 : X → 𝔻**.
 - **Satisfiability**
	- **I** and **V** satisfy **φ** (denoted by **(I, V) ⊨ φ**) if **(I, V)[φ] = True**.
 - **First-Order Model**
	- An interpretation **I** is a **model** of **φ** (denoted by **I ⊨ φ**) if for all valuations **V** we have **(I, V) ⊨ φ**.
 - **First-Order Tautology**
	- A formula **φ** is a **tautology** if all its interpretations are models, i.e., **(I, V) ⊨ φ** for all interpretations **I** and all valuations **V**.
 - **Remark**
	- Free variables are universally quantified (e.g., **P(x)** is equivalent to **∀x. P(x)**).


### Undecidable Problems
For a formula **φ**, the following are undecidable:
- Is **I ⊨ φ**?
- Is there an interpretation **I** and valuation **V** such that **(I, V) ⊨ φ**?
- Is there an interpretation **I** and valuation **V** such that **(I, V) ⊭ φ**?
#### Isabelle/HOL Tools
- **`apply auto`**: Automatically tries to prove **⊨ φ** using decision procedures (e.g., arithmetic).  
    _Note: Failure doesn’t mean unprovable._
- **`nitpick`**: Tries to find **I** and **V** such that **(I, V) ⊭ φ**.  
    _Note: Failure doesn’t mean there’s no counterexample._

___
- **Logical operators in decreasing priority**:  
    `¬, ∧, ∨, →, ∀, ∃`  
    (_Higher priority operators choose operands first._)
- **Examples**:
    - `¬¬P = P` means `¬(P = P)`
    - `A ∧ B = B ∧ A` means `A ∧ (B = B) ∧ A`
    - `P ∧ ∀x. Q(x)` will be parsed as `(P ∧ ∀x. Q(x))`, so write `P ∧ (∀x. Q(x))` instead.
- **Associativity**:
    - All binary operators associate to the right.  
        Example: `A → B → C` is equivalent to `A → (B → C)`.
- **Shorthand for nested quantifications**:  
    `∀x. ∀y. ∀z. P` can be written as `∀x y z. P`.
- **Free variables**:  
    Free variables are universally quantified.  
    Example: `P(x)` is equivalent to `∀x. P(x)`.
- **Isabelle/HOL preference**:  
    Isabelle/HOL tools will prefer `P(x)` over `∀x. P(x)`.
___
- **Satisfiable formula** ->A formula **φ** is **satisfiable** if there exists an interpretation **I** and a valuation **V** such that **(I, V) ⊨ φ**.
- **Contradiction** ->A formula is contradictory (or unsatisfiable) if it cannot be satisfied, i.e., (I, V) ⊭ φ for all interpretations I and all valuations V.
#### Property:
A formula φ is contradictory if and only if ¬φ is a tautology.


