
### Recursive Functions and Algebraic Data Types

#### 1. Recursion in Isabelle/HOL:
- **No imperative constructs**: There are no loops (`while`, `for`), no mutable arrays, or pointers.
- **Recursion is key**: All functions and complex types in Isabelle/HOL are defined using recursion.
- **Equivalence**: The expressive power of recursive languages is equivalent to imperative languages.
  
#### 2. Recursive Functions:
- **Direct recursion**: A function that calls itself directly.
  Example: 
  ```isabelle
  fun contains:: "'a => 'a list => bool"
  where
  "contains e [] = False" |
  "contains e (x#xs) = (e=x \/ (contains e xs))"
  ```
- **Indirect recursion**: Functions that call each other.
  Example:
  ```isabelle
  fun even:: "nat => bool" and odd:: "nat => bool"
  where
  "even 0 = True" |
  "even (Suc x) = odd x" |
  "odd 0 = False" |
  "odd (Suc x) = even x"
  ```

#### 3. Terminating Recursive Functions:
- **Termination conditions**:
  - At least one base case.
  - Recursive calls must reduce a parameter toward the base case.
- **Measure function**: Prove the termination of recursive calls by showing that a measure (e.g., length) decreases with every call.
  
#### 4. Proving Termination:
- In Isabelle/HOL, recursive functions must be terminating.
  - Automatic termination proof is attempted using the `fun` keyword.
  - If automatic proof fails, use `function` and provide a measure manually.
  - Example using `function`:
  ```isabelle
  function (sequential) contains:: "'a => 'a list => bool"
  where
  "contains e [] = False" |
  "contains e (x#xs)= (if e=x then True else (contains e xs))"
  apply pat_completeness
  apply auto
  done
  ```

#### 5. Recursive Algebraic Data Types:
- **Definition**: Used to define enumerated types, unions, and structured types.
  Example of polymorphic lists:
  ```isabelle
  datatype 'a list = Nil | Cons 'a "'a list"
  ```
- **Pattern Matching**: Objects of algebraic data types can be matched using case expressions.
  
#### 6. Type Abbreviations:
- **Type synonyms**: Used to introduce shorthand for complex types.
  Example:
  ```isabelle
  type_synonym name = "(string * string)"
  ```

