
###   Proofs with a Proof Assistant

  
**Counterexample Finders:**
- **Nitpick:** 
  - Uses finite model enumeration, effective for any logic formula but may fail on larger domains.
  - Example command: `nitpick [timeout=120, show_all, card=3-5]`
- **Quickcheck:**
  - Uses random, exhaustive, or narrowing methods.
  - Example command: `quickcheck [tester=narrowing, size=6]`

**Proof Techniques:**
- **Proof by Cases:**
  - Splitting proofs into multiple cases.
  - Example: `apply (case tac "F :\: bool")`
  
- **Proof by Induction:**
  - Best for recursive functions.
  - Example: `apply (induct x)`
  
- **Combining Decision Procedures:**
  - Using `apply auto` and `apply simp` to simplify and solve subgoals automatically.

**Sledgehammer:**
- Automatically calls external theorem provers (e.g., SPASS, E, Vampire).
- Example usage: `sledgehammer [timeout=120]`
---

  
