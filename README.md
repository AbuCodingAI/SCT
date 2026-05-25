# Shariff Complexity Theorem (SCT)
### A Formal Model for Programming Language Complexity
**Abu Shariff** · 2026

---

## Abstract

Existing complexity metrics such as Cyclomatic complexity and Kolmogorov complexity measure the complexity of *code*. No widely accepted metric measures the complexity of a *language's design* itself — the cognitive burden it places on a learner and the labor distribution between programmer and compiler. The Shariff Complexity Theorem (SCT) fills this gap by modeling language complexity on two independent axes: Esotericosity (E), a geometric measure of how far a language sits from both machine and human semantics, and Work Distribution (WD), a measure of how compiler-grade labor is divided between the programmer and the runtime. SCT produces a single scalar score that predicts learnability and cognitive load for a given programming language. Notably, SCT scores can exceed 100 for sufficiently pathological languages — this is intentional, not a flaw.

---

## 1. Motivation

When a programmer asks "how hard is this language to learn?", existing tools give no satisfying answer. Cyclomatic complexity tells you about branching in a specific program. Kolmogorov complexity tells you about the compressibility of a string. Neither tells you whether a language forces you to manually manage memory, reason about reduction rules, or interpret whitespace as executable syntax.

SCT is designed to answer a different question: **given a language's design, how much does it demand of its programmer, and how alien is its syntax to both human thought and machine execution?**

---

## 2. Esotericosity (E)

### 2.1 Definition

Esotericosity measures how far a language is from *both* machine semantics and human language semantics simultaneously.

We define a 2D cartesian plane where:

- The **X axis** represents closeness to machine/binary semantics
- The **Y axis** represents closeness to human language semantics
- The **origin (0, 0)** is defined as Assembly (ASM) — the closest a human-readable language gets to raw binary, and the fixed anchor of the entire model

Languages are plotted as coordinates (x, y) on this plane, bounded 0 ≤ x, y ≤ 100. A language close to the X axis is machine-like. A language close to the Y axis is human-like. A language near the diagonal y=x — equidistant from both axes — is maximally esoteric.

### 2.2 Formula

E = min(x,y)

Where (x, y) is the language's coordinate on the esotericosity plane.

This formula has an elegant property: **languages on the diagonal y=x have E equal to their coordinate value, making the diagonal the natural scale of esotericosity itself.** A language at (50, 50) has E=50; at (75, 75) has E=75; at (100, 100) has E=100. The diagonal is not just where high-E languages live — it is the E scale.

| Language position | E value | Intuition |
|---|---|---|
| On the X axis (machine-like) | 0                              | Close to machine semantics, not esoteric  |
| On the Y axis (human-like)   | 0                              | Close to human semantics, not esoteric    |
| On the diagonal y=x          | equals coordinate value        | Equally far from both anchors             |
| ASM at (0, 0)                | 0                              | Defined anchor of the model               |
| Malbolge at (100, 100)       | 100                            | Maximum esotericosity                     |

### 2.3 Example Coordinates

<img width="982" height="833" alt="Screenshot from 2026-05-25 16-08-56" src="https://github.com/user-attachments/assets/72d41c60-c018-495f-b431-5396bcc3b8bf" />

*[Figure 1: Esotericosity plane — see diagram]*

| Language | (x, y) | E = min(x,y) |
|---|---|---|
| ASM | (0, 0) | 0 |
| Python | (10, 90) | 10 |
| AC | (15, 75) | 15 |
| AI | (20, 70) | 20 |
| C | (60, 30) | 30 |
| Lambda Calculus | (20, 20) | 20 |
| Whitespace | (95, 95) | 95 |
| Malbolge | (100, 100) | 100 |

---

## 3. Work Distribution (WD)

### 3.1 The Employer Analogy

In any programming task, three parties are involved:

- **The Computer** — the employer, who defines what must ultimately happen
- **The Compiler/Runtime** — an experienced employee who handles complex, fast, low-level work
- **The Programmer** — a rookie employee who does slower, higher-level work

WD measures how much *compiler-grade labor* is pushed onto the programmer.

- **Low WD** — the compiler absorbs complexity. The programmer writes simple instructions. (Python, AC)
- **High WD** — the programmer must do compiler-grade work manually. ASM is defined as WD = 100, the upper anchor for real-valued WD.
- **Complex WD** — both the programmer and compiler struggle simultaneously, creating redundant suffering on both sides.

### 3.2 Real-Valued WD

For most languages, WD is a real number
  
ASM is defined as WD = 100, the fixed upper anchor of the real-valued scale.

### 3.3 barrier Tax

If a language cannot reasonably run on a ten-year-old machine without additional cost, its WD receives a **+20 penalty**:

WD += 20

This models practical learnability — a language requiring paid software or specialized hardware is less accessible regardless of its syntax.

**Examples:**
- MATLAB → +20 (closed-source, paid license)
- Heavy Minecraft redstone → +20 (hardware-dependent)
- Python → +0 (runs anywhere)
- COBOL → +0 (freely available)

### 3.4 Complex WD

<img width="723" height="777" alt="Screenshot from 2026-05-25 16-27-30" src="https://github.com/user-attachments/assets/3355e321-c439-4d26-ae58-54c2fe8dad6e" />

*[Figure 2: WD plane — see diagram]*

When a language forces the programmer to do compiler-grade work *and* forces the compiler into heavy decoding work simultaneously, both parties suffer redundantly. This pathological case exits the real number line.

For such languages:

  WD = a + bi  

Where:
- **a** = programmer labor (real component)
- **b** = shared/compiler suffering (imaginary component)
- **b** can theoretically approach infinity for sufficiently pathological languages

The effective WD magnitude is computed via the Pythagorean theorem:

  |WD| = sqrt{a^2 + b^2}  

This is geometrically meaningful: pathological languages *leave the normal abstraction line* and their complexity must be measured as a distance in 2D labor-space rather than a scalar.

---

## 4. SCT Formula

SCT = 0.7(E) + 0.3(|WD|)  

Where:
- E is computed as min(x, y) from the esotericosity plane
- |WD| = WD for real-valued cases, √(a²+b²) for complex cases
- The 0.7/0.3 weighting reflects that syntactic alienness (E) is the dominant driver of learnability difficulty, while labor distribution (WD) is a significant but secondary factor
- SCT may exceed 100 for pathological languages — this is intentional and meaningful

---

## 5. Worked Examples
barrier
### 5.1 Python
- Coordinate: (10, 90). E = **10**
- WD: Compiler absorbs almost everything. WD = **10**
- No barrier tax.

  SCT = 0.7(10) + 0.3(10) = 7 + 3 = textbf{10}  

### 5.2 ASM (x86-64)
- Coordinate: (0, 0). E = **0**
- WD: Defined anchor. WD = **100**
- No barrier tax.

  SCT = 0.7(0) + 0.3(100) = 0 + 30 = textbf{30}  

### 5.3 AC (A language family — compiled, 14 targets)
- Coordinate: (15, 75). E = **15**
- WD: Compiler handles 14 output targets automatically, massive labor absorption. WD = **20**
- No barrier tax.

  SCT = 0.7(15) + 0.3(20) = 10.5 + 6 = textbf{16.5}  

### 5.4 AI (A language family — interpreted, exact decimal, AIVM)
- Coordinate: (20, 70). E = **20**
- WD: AIVM absorbs all runtime complexity. WD = **16**
- No barrier tax.

  SCT = 0.7(20) + 0.3(16) = 14 + 4.8 = textbf{18.8}  

### 5.5 Lambda Calculus
- Coordinate: (20, 20). E = **20**
- WD: Programmer manually reasons about reduction, substitution, recursion encoding. WD = **70**
- No barrier tax.

  SCT = 0.7(20) + 0.3(70) = 14 + 21 = textbf{35}  

### 5.6 MATLAB
- Coordinate: (15, 70). E = **15**
- WD: Moderate programmer effort. WD = 40, +20 barrier tax → **60**

  SCT = 0.7(15) + 0.3(60) = 10.5 + 18 = textbf{28.5}  

### 5.7 Malbolge
- Coordinate: (100, 100). E = **100** (maximum)
- WD: Programmer and interpreter both suffer maximally. Complex WD = 90 + 80i
- |WD| = √(90² + 80²) = √14500 ≈ **120.4**
- No barrier tax.

  SCT = 0.7(100) + 0.3(120.4) = 70 + 36.1 = textbf{106.1}  

Malbolge intentionally exceeds 100 — it is the canonical example of a language designed to be impossible.

### 5.8 Whitespace
- Coordinate: (95, 95). E = **95**
- WD: Programmer must mentally parse invisible syntax. Complex WD = 80 + 60i
- |WD| = √(80² + 60²) = √10000 = **100**

  SCT = 0.7(95) + 0.3(100) = 66.5 + 30 = textbf{96.5}  

### 5.9 SCT rating itself
- Coordinate: (5, 80). E = **5**
- WD: Human must classify, estimate coordinates, reason geometrically. Compiler does nothing — SCT is conceptual. WD = **40**

  SCT = 0.7(5) + 0.3(40) = 3.5 + 12 = textbf{15.5}  

SCT rates itself slightly easier than Python to understand, which feels correct.

---

## 6. Summary Table

| Language | E |    |WD  | | SCT |
|---|---|---|---|
| Python | 10 | 10 | **10** |
| SCT itself | 5 | 40 | **15.5** |
| AC | 15 | 20 | **16.5** |
| AI | 20 | 16 | **18.8** |
| MATLAB | 15 | 60 | **28.5** |
| ASM | 0 | 100 | **30** |
| Lambda Calculus | 20 | 70 | **35** |
| Whitespace | 95 | 100 | **96.5** |
| Malbolge | 100 | 120.4 | **106.1** |

---

## 7. Limitations and Future Work

**Subjectivity of coordinate placement.** E relies on the author's judgment when placing a language on the (x, y) plane. Future work should develop a formal rubric — for example, scoring axes based on presence of English keywords, direct memory address exposure, and syntactic distance from formal grammars.

**WD weighting (0.7/0.3).** The current weighting is motivated by intuition — syntactic alienness feels like the dominant driver of learnability. Empirical validation through learner studies would strengthen this claim.

**SCT exceeding 100.** For languages with complex WD and high E simultaneously, SCT naturally exceeds 100. This is treated as a feature — it correctly identifies languages that are pathological on both dimensions simultaneously.

**Extending to paradigms.** SCT currently rates languages. A natural extension would rate *paradigms* (functional, logic, concatenative) independently of syntax, then combine scores.

**Empirical validation.** SCT predictions should be tested against measured time-to-proficiency data for programmers learning new languages.

---

## 8. Conclusion

SCT provides a lightweight, geometrically grounded framework for comparing programming language complexity. By separating syntactic alienness (E) from labor distribution (WD), and extending WD into the complex plane for pathological cases, SCT captures dimensions of language difficulty that existing metrics miss entirely. The diagonal of the esotericosity plane serves double duty as both the region of maximum E and the natural scale of esotericosity itself — a property that emerges directly from the min(x,y) formula. The model is simple enough to apply by hand yet rigorous enough to produce consistent, defensible scores across languages as different as Python, ASM, and Malbolge.

---

*Abu Shariff, age 12. BASIS Chandler. 2026.*
*Contact: abu.shariffaiml@gmail.com*
*Language family referenced: AC/AI/AC+ — github.com/AbuCodingAI*
  
