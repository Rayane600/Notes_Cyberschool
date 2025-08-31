
## 1. Computational Models

### Finite Automata
- **Components**: Alphabet, set of states (with initial and final states), transitions
  - Example: Automaton that recognizes words ending with ‘1’ and an even number of ‘0’s after the first ‘1’.
- **Language Recognized**: $( L(M) = \{w \in \Sigma^* \mid M \text{ accepts } w\} ), where (\Sigma^*)$ represents all possible strings.

### Regular Languages and Finite Automata
- A language is **regular** if a finite automaton recognizes it.
- Example: Pattern matching for strings containing ‘001’.

### Non-Deterministic Finite Automata (NFA)
- Allows multiple paths and ε-transitions (jumps without reading input).
- **Deterministic Finite Automaton (DFA)**: Can be constructed from an NFA, but may have exponentially more states.

### Regular Expressions
- Used to describe regular languages (e.g., \((0+1)0^*\)).

---

## 2. Non-Regular Languages and Context-Free Grammars (CFG)

### Non-Regular Languages
- Example: The language $( B = \{0^n1^n \mid n \geq 0\} )$ cannot be recognized by a finite automaton.
- **Proof by Pumping Lemma**: Splitting a word and showing it leads to contradictions.

### Context-Free Grammars (CFG)
- Defines languages with **substitution rules** (productions).
  - Example: $( G = \{ A \rightarrow 0A1, A \rightarrow B, B \rightarrow \# \} )$.
  - Derivation example: $( A \Rightarrow 0A1 \Rightarrow 00A11 \Rightarrow 000B111 \Rightarrow 000\#111 )$.

### Pushdown Automata
- Machines that recognize context-free languages using a stack.

---

## 3. Turing Machines (TM)

### Turing Machine Overview
- A more powerful computational model than finite automata.
- Can read/write on an infinite tape and use a set of states for computation.
- **Configurations**: The state of the machine combined with the content of the tape (e.g., \( 1011q701111 \)).

### Recognizable vs Decidable Languages
- **Recognizable**: A TM halts on accepting inputs but may loop on others.
- **Decidable**: A TM halts on all inputs, either accepting or rejecting.
- **Church-Turing Thesis**: Any intuitive algorithm can be simulated by a Turing machine.
- **Undecidable Problems**: Problems like the Halting Problem cannot be solved by any TM.

---

## 4. RAM Model (Random Access Machine)
- **Components**: 
  - Infinite memory (registers), each storing a natural number.
  - Instruction set includes increment, decrement, copy, jump, etc.
  - Special **instruction register** to point to the current instruction.
- **Equivalence**: The RAM model is equivalent to a Turing machine but more practical for real-world computing.

---

## 5. Reductions and Complexity

### Reductions
- **Definition**: A problem \( A \) reduces to \( B \) if solving \( B \) helps solve \( A \).
- **Significance**: If \( A \) reduces to \( B \), then solving \( A \) is at most as hard as solving \( B \).

### Time Complexity
- **Time complexity**: Measures the amount of time an algorithm takes to solve a problem based on input size.
- **Key Classes**:
  - **P**: Problems that can be solved in polynomial time.
  - **NP**: Problems where solutions can be verified in polynomial time.
  - **P vs NP**: Open question—whether every problem whose solution can be verified quickly (in NP) can also be solved quickly (in P).

---
