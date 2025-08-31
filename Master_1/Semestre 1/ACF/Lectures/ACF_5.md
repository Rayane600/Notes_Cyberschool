

## Lesson 6: Certified Programming
### Outline
1. **Certified Program Production Lines**:
   - Examples and their weak links.
   - Certification of compilers, static analyzers, and proofs.
2. **Methodology for Formal Definitions**:
   - Simplify program proofs.
   - Generalize properties and minimize the trusted base.

### Key Concepts
- **Certified Code Production Lines**:
  - Example: Metro 14 automatic train control using Scade, Astree, and CompCert.
  - Airbus: Flight control code verified for safety-critical conditions.
- **Tools and Techniques**:
  - **Scade**: Model-checking for program verification.
  - **Astree**: Static analysis for error detection.
  - **CompCert**: Verified C compiler.
  - **Isabelle to Scala**: Specification and verification of software like seL4 kernel.

### Challenges in Certification
- Weak links:
  - Algorithm and property choice.
  - Verification tools (proof assistants, analyzers).
  - Code generators and compilers.
- Solutions:
  - Certify compilers and analyzers.
  - Ensure proof verification.
  - Limit the trusted base through clear methodology.

---

##   Program Verification Methods
### Outline
1. **Testing**:
   - Testing fundamentals and principles.
   - Random and white-box testing.
   - Tools: Klee, SAGE, PathCrawler, Evosuite.
2. **Model-Checking**:
   - Principles of finite-state exploration.
   - Tools: SPIN, CBMC, SLAM.
3. **Assisted Proof**:
   - Proving software properties using tools like Coq, Isabelle/HOL.
4. **Static Analysis**:
   - Abstract interpretation for property proof.
   - Tools: Astree, Polyspace, Infer.

### Key Techniques
- **Testing**:
  - Strengths: Finds real bugs, test coverage as quality metric.
  - Weaknesses: Cannot prove properties, only checks finite scenarios.
- **Model-Checking**:
  - Strengths: Can prove properties and find bugs in finite models.
  - Weaknesses: Limited to finite abstractions.
- **Assisted Proof**:
  - Strengths: Proves properties with certification, counterexample finders.
  - Weaknesses: Requires manual assistance, limited to prototypes.
- **Static Analysis**:
  - Strengths: Proves properties automatically on source code.
  - Weaknesses: Not designed for bug-finding.

### Summary of Verification Techniques
- **Applicability**:
  - Testing: Works on executables.
  - Static Analysis: Source code.
  - Model-Checking and Assisted Proof: Models/prototypes.
- **Comparison**:
  - Static analysis: Fully automatic.
  - Model-checking: Efficient but limited to finite models.
  - Assisted proof: Strong but needs human intervention.

### Insight on Formal Methods
- Essential for secure software development.
- Hackers exploit formal methods to find vulnerabilities (e.g., Chip-and-PIN flaws).

### Case Studies and Tools
- SAGE: Found 1/3 of security bugs in Windows 7.
- Astree: Used in Airbus A380 flight control systems.
- CompCert: Certified C compiler for high-assurance systems.

---

### Reflection on Formal Methods
- **Advantages**:
  - Strong assurance through proofs.
  - Reduction of manual errors in verification.
- **Challenges**:
  - Complexity in formalizing properties and managing large models.
  - Reliance on tool accuracy and completeness.

---