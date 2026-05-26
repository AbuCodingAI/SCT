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

## 2. Esotericosity (E) — Revised Model

### 2.1 Axis Definitions

Esotericosity is measured on a 2D cartesian plane with two axes representing the two natural poles of programming language design:

- **X axis — Machine/ASM-likeness:** How much does the language express machine-level concepts directly? This includes pointer arithmetic, manual memory management, register-level operators (`^`, `|`, `&`, `<<`, `>>`), explicit type sizes, and direct hardware access. ASM sits near the top of this axis because it is the most machine-viable language a human can actually write. Raw binary is excluded as an axis anchor because it is not a viable human-writable language — it is a theoretical limit, not a real pole.

- **Y axis — Conventional readable syntax:** How close is the language to what humans consider normal, readable code? This means familiar keywords, readable structure, standard operators, and conventional programming patterns. No language reaches 100 — Python at 95 is the practical ceiling. This axis is NOT natural English — it is readable programming syntax. A language like COBOL scores high here because `MOVE SALARY TO DISPLAY` is readable conventional code, not because it is English prose.

- **Origin (0, 0) — The void:** A language with zero machine concepts AND zero readable syntax. This is the theoretical maximum esotericosity — a language that belongs to neither world. Malbolge and Whitespace approach this point. It is not a perfect language; it is a non-language. Attempting to evaluate a language at exactly (0,0) is undefined — *Preposterous: NoLanguage.*

### 2.2 Why Binary Is Not an Axis

Early versions of this model used binary/machine code as the X axis anchor. This was incorrect. Binary is not a language humans write — it is what languages compile to. Including it as an axis distorts the plane because no human-writable language actually approaches it. ASM is the correct anchor: it is the closest a human can get to machine semantics while still writing something readable.

### 2.3 Formula

$$E = 100\left(1 - \frac{\sqrt{x^2 + y^2}}{100\sqrt{2}}\right)$$

Where:
- $(x, y)$ is the language's coordinate on the esotericosity plane
- $100\sqrt{2} \approx 141.4$ is the maximum possible distance from origin (at coordinate (100,100))
- E is bounded $0 \leq E \leq 100$

**Key properties:**

| Condition | E value | Intuition |
|---|---|---|
| Origin (0,0) | 100 | Maximum esotericosity — belongs to nothing |
| On X axis at (100,0) | 29.3 | Machine-like but not human — low-moderate E |
| On Y axis at (0,100) | 29.3 | Human-like but not machine — low-moderate E |
| Corner (100,100) | 0 | Perfect language — maximally both |
| ASM ≈ (95,18) | 31.6 | Close to X axis — low E ✓ |
| Python ≈ (8,95) | 32.6 | Close to Y axis — low E ✓ |
| Malbolge ≈ (3,3) | 97.0 | Near origin — maximum E ✓ |

**Why distance from origin, not min(x,y):**

The previous formula E = min(x,y) collapsed unfairly — a language at (5,95) and one at (5,5) both got E=5, despite being in completely different regions of the plane. (5,5) is near the void and should have HIGH E. (5,95) is close to the Y axis and should have LOW E. Distance from origin correctly separates these cases because both coordinates contribute simultaneously.

**The Python/ASM symmetry:**

Python (E≈32.6) and ASM (E≈31.6) score nearly identically — for opposite reasons. Python is maximally readable-syntax and minimally machine-like. ASM is maximally machine-like and minimally readable-syntax. Both are close to their respective axes, far from the void. The model correctly gives them similar E scores because both are genuinely non-esoteric — they each belong firmly to one pole.

### 2.4 Language Coordinates and E Scores

*[Figure 1: Esotericosity plane — see Desmos diagram]*

| Language | X (machine-like) | Y (readable syntax) | E |
|---|---|---|---|
| Malbolge | 3 | 3 | **97.0** |
| Whitespace | 5 | 5 | **96.5** |
| Lambda Calculus | 5 | 8 | **93.4** |
| INTERCAL | 12 | 50 | **63.4** |
| Java | 30 | 68 | **49.8** |
| MATLAB | 18 | 65 | **50.8** |
| Go | 45 | 70 | **41.8** |
| Rust | 68 | 50 | **41.8** |
| C | 75 | 30 | **42.9** |
| C++ | 78 | 32 | **40.4** |
| COBOL | 10 | 92 | **34.6** |
| Python | 8 | 95 | **32.6** |
| ASM | 95 | 18 | **31.6** |
| AC | 20 | 85 | **38.3** |
| AI language | 15 | 82 | **40.1** |
| JaSQL | 5 | 90 | **35.1** |

### 2.5 The Void and the Perfect Language

Two theoretical extremes bound the model:

**The void (0,0):** Zero machine concepts, zero readable syntax. Belongs to nothing. Maximum E=100. Not a language — a hole in the space of languages. *Preposterous: NoLanguage.*

**The perfect language (100,100):** Maximally machine-expressive AND maximally readable. E=0. This language does not exist and may be impossible — being fully machine-expressive typically requires sacrificing readability and vice versa. It is the unreachable ideal that all language design moves toward.

---

## 5. Worked Examples — Revised

### 5.1 Python
- Coordinate: (8, 95)
- X=8: bitwise relics (`^`, `|`, `&`, `<<`, `>>`) inherited from C are the only machine-level constructs
- Y=95: most readable conventional syntax of any general-purpose language
- E = 100(1 - √(64+9025)/141.4) = 100(1 - 95.3/141.4) = **32.6**
- WD = 10 (compiler absorbs almost everything)

$$SCT = 0.7(32.6) + 0.3(10) = 22.8 + 3 = \mathbf{25.8}$$

### 5.2 ASM (x86-64)
- Coordinate: (95, 18)
- X=95: direct register manipulation, explicit memory addresses, hardware instructions
- Y=18: mnemonics like `mov`, `rax`, `rdi` are barely readable — functional but not conventional syntax
- E = 100(1 - √(9025+324)/141.4) = 100(1 - 96.7/141.4) = **31.6**
- WD = 100 (defined anchor)

$$SCT = 0.7(31.6) + 0.3(100) = 22.1 + 30 = \mathbf{52.1}$$

### 5.3 C
- Coordinate: (75, 30)
- X=75: pointers, manual memory, bitwise operators, explicit types
- Y=30: recognizable keywords but sparse and unforgiving
- E = 100(1 - √(5625+900)/141.4) = 100(1 - 80.8/141.4) = **42.9**
- WD = 45 (programmer handles memory, types, all edge cases)

$$SCT = 0.7(42.9) + 0.3(45) = 30.0 + 13.5 = \mathbf{43.5}$$

### 5.4 C++
- Coordinate: (78, 32)
- X=78: everything C has plus templates, operator overloading
- Y=32: slightly more readable than C but denser
- E = 100(1 - √(6084+1024)/141.4) = 100(1 - 84.3/141.4) = **40.4**
- WD = 55 (all of C's burden plus template metaprogramming complexity)

$$SCT = 0.7(40.4) + 0.3(55) = 28.3 + 16.5 = \mathbf{44.8}$$

### 5.5 Rust
- Coordinate: (68, 50)
- X=68: explicit ownership, lifetimes, unsafe blocks, bitwise operators
- Y=50: more readable than C, less than Go — ownership syntax is unfamiliar but structured
- E = 100(1 - √(4624+2500)/141.4) = 100(1 - 84.4/141.4) = **40.3**
- WD = 55 (borrow checker pushes compiler-grade reasoning onto programmer)

$$SCT = 0.7(40.3) + 0.3(55) = 28.2 + 16.5 = \mathbf{44.7}$$

### 5.6 Go
- Coordinate: (45, 70)
- X=45: explicit types, some pointers, goroutine primitives
- Y=70: clean conventional syntax, readable but strict
- E = 100(1 - √(2025+4900)/141.4) = 100(1 - 83.2/141.4) = **41.2**
- WD = 25 (compiler handles a lot, garbage collected, strict but not punishing)

$$SCT = 0.7(41.2) + 0.3(25) = 28.8 + 7.5 = \mathbf{36.3}$$

### 5.7 Java
- Coordinate: (30, 68)
- X=30: typed, some low-level concepts, verbose boilerplate
- Y=68: readable keywords but verbose ceremony (`public static void main`)
- E = 100(1 - √(900+4624)/141.4) = 100(1 - 74.3/141.4) = **47.5**
- WD = 30 (JVM absorbs a lot but programmer writes enormous boilerplate)

$$SCT = 0.7(47.5) + 0.3(30) = 33.3 + 9 = \mathbf{42.3}$$

### 5.8 MATLAB
- Coordinate: (18, 65)
- X=18: matrix operators, some low-level indexing concepts
- Y=65: readable but domain-specific, unusual syntax for non-math people
- E = 100(1 - √(324+4225)/141.4) = 100(1 - 67.5/141.4) = **52.3**
- WD = 35, +20 barrier tax → **55**

$$SCT = 0.7(52.3) + 0.3(55) = 36.6 + 16.5 = \mathbf{53.1}$$

### 5.9 Lambda Calculus
- Coordinate: (5, 8)
- X=5: no machine concepts whatsoever — pure mathematical abstraction
- Y=8: `λx.λy.x` is not conventional readable syntax by any measure
- E = 100(1 - √(25+64)/141.4) = 100(1 - 9.4/141.4) = **93.4**
- WD = 70 (programmer manually reasons about reduction, substitution, Church encodings)

$$SCT = 0.7(93.4) + 0.3(70) = 65.4 + 21 = \mathbf{86.4}$$

### 5.10 COBOL
- Coordinate: (10, 92)
- X=10: `END-IF`, `END-PERFORM` delimiters have slight machine-like block management quality
- Y=92: `MOVE SALARY TO DISPLAY` reads as close to conventional readable code as anything ever written — designed explicitly for business manager readability in 1959
- E = 100(1 - √(100+8464)/141.4) = 100(1 - 92.5/141.4) = **34.6**
- WD = 20 (runtime absorbs most complexity; programmer writes verbose but simple statements)

$$SCT = 0.7(34.6) + 0.3(20) = 24.2 + 6 = \mathbf{30.2}$$

COBOL scores **30.2** — nearly identical to Python (25.8) and ASM (52.1 — wait, ASM WD pulls it up significantly). COBOL's reputation as a painful language is an ecosystem and tooling problem, not a language design problem. SCT correctly identifies this.

### 5.11 INTERCAL
- Coordinate: (12, 50)
- X=12: has a unary OR and binary XOR but presented so abstractly they barely count
- Y=50: `PLEASE DO FORGET` uses English words in conventional statement structure — genuinely more readable than Lambda Calculus despite being a joke language
- E = 100(1 - √(144+2500)/141.4) = 100(1 - 51.4/141.4) = **63.7**
- WD = 80 (programmer must manage PLEASE frequency or compiler refuses — genuinely pathological labor)
- Complex WD: the compiler actively sabotages you. WD = 75 + 40i → |WD| = √(5625+1600) = **84.9**

$$SCT = 0.7(63.7) + 0.3(84.9) = 44.6 + 25.5 = \mathbf{70.1}$$

INTERCAL scores 70.1 — in the extreme category, which is correct. The PLEASE mechanism alone makes WD complex.

### 5.12 Malbolge
- Coordinate: (3, 3)
- E = 100(1 - √(9+9)/141.4) = 100(1 - 4.2/141.4) = **97.0**
- Complex WD = 90 + 80i → |WD| = **120.4**

$$SCT = 0.7(97.0) + 0.3(120.4) = 67.9 + 36.1 = \mathbf{104.0}$$

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

For most languages, WD is a real number where 0 ≤ WD ≤ 120.

ASM is defined as WD = 100, the fixed upper anchor of the real-valued scale.

### 3.3 Barrier Tax

If a language cannot reasonably run on a ten-year-old machine without additional cost, its WD receives a **+20 penalty**:

$$WD \mathrel{+}= 20$$

This models practical learnability — a language requiring paid software or specialized hardware is less accessible regardless of its syntax.

**Examples:**
- MATLAB → +20 (closed-source, paid license)
- Heavy Minecraft redstone → +20 (hardware-dependent)
- Python → +0 (runs anywhere)
- COBOL → +0 (freely available)

### 3.4 Complex WD

*[Figure 2: Complex WD plane]*

When a language forces the programmer to do compiler-grade work *and* forces the compiler into heavy decoding work simultaneously, both parties suffer redundantly. This pathological case exits the real number line.

For such languages:

$$WD = a + bi$$

Where:
- **a** = programmer labor (real component)
- **b** = shared/compiler suffering (imaginary component)
- **b** can theoretically approach infinity for sufficiently pathological languages

The effective WD magnitude is computed via the Pythagorean theorem:

$$|WD| = \sqrt{a^2 + b^2}$$

This is geometrically meaningful: pathological languages *leave the normal abstraction line* and their complexity must be measured as a distance in 2D labor-space rather than a scalar.

---

## 4. SCT Formula

$$\boxed{SCT = 0.7(E) + 0.3(|WD|)}$$

Where:
- E is computed as min(x, y) from the esotericosity plane
- $|WD|$ = WD for real-valued cases, $\sqrt{a^2+b^2}$ for complex cases
- The 0.7/0.3 weighting reflects that syntactic alienness (E) is the dominant driver of learnability difficulty, while labor distribution (WD) is a significant but secondary factor
- SCT may exceed 100 for pathological languages — this is intentional and meaningful

---


## 6. Summary Table — Revised

| Language | E | \|WD\| | SCT |
|---|---|---|---|
| Python | 32.6 | 10 | **25.8** |
| COBOL | 34.6 | 20 | **30.2** |
| ASM | 31.6 | 100 | **52.1** |
| Go | 41.2 | 25 | **36.3** |
| Java | 47.5 | 30 | **42.3** |
| C | 42.9 | 45 | **43.5** |
| Rust | 40.3 | 55 | **44.7** |
| C++ | 40.4 | 55 | **44.8** |
| MATLAB | 52.3 | 55 | **53.1** |
| Lambda Calculus | 93.4 | 70 | **86.4** |
| INTERCAL | 63.7 | 84.9 | **70.1** |
| Malbolge | 97.0 | 120.4 | **104.0** |


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

---

## 9. General SCT (GSCT)

### 9.1 Overview

SCT was developed using programming languages as its domain, but the underlying framework is domain-agnostic. Any system where:

- Two natural "anchor" extremes exist
- A measurable amount of expert-level effort is pushed onto the user

...can be rated with General SCT (GSCT). The formula is identical:

$$GSCT = 0.7(E) + 0.3(|WD|)$$

The only thing that changes between domains is how E and WD are *interpreted*.

### 9.2 Defining E and WD for Any Domain

For any domain, ask two questions:

**For E:**
> What are the two most "natural" or "intuitive" poles of this domain? Things close to either pole have low E. Things equidistant from both — belonging fully to neither — have high E.

**For WD:**
> Who does the expert-level work — the system/institution, or the user/participant? When the system absorbs complexity, WD is low. When the user must supply skilled judgment the system doesn't handle, WD is high. Complex WD applies when both sides are simultaneously doing heavy work on the same input.

**For Barrier Tax:**
> Does accessing or using this thing require unusual cost, equipment, or prerequisite that a typical person wouldn't have? If yes, +20.

### 9.3 Domain Examples

#### Programming Languages *(original domain)*
| Axis | Pole |
|---|---|
| E anchor 1 | Machine code / ASM |
| E anchor 2 | Natural human language |
| WD = 0 | Compiler handles everything |
| WD = 100 | Programmer handles everything (ASM) |

---

#### Colleges & Universities
| Axis | Pole |
|---|---|
| E anchor 1 | Pure vocational / trade school |
| E anchor 2 | Pure theoretical / research institution |
| WD = 0 | Fully structured — institution tells you exactly what to do |
| WD = 100 | Fully self-directed — student designs everything |

A purely vocational school (electrician training) sits on one axis — low E. A pure research university like Caltech sits on the other — also low E. A school that's neither structured nor research-focused, offering vague interdisciplinary programs with no clear outcome, drifts toward the diagonal — high E.

Barrier tax applies if tuition exceeds what a typical family can access without significant debt restructuring.

---

#### Workplaces
| Axis | Pole |
|---|---|
| E anchor 1 | Fully automated / process-driven |
| E anchor 2 | Fully human / purely creative |
| WD = 0 | Systems handle all complex judgment calls |
| WD = 100 | Employee supplies all expert judgment with no tooling support |

A factory assembly line sits near the automated pole — low E, low WD. A freelance artist studio sits near the human/creative pole — low E, high WD. A workplace that's neither automated nor creatively free — bureaucratic, manual, and arbitrary — drifts toward the diagonal.

Complex WD applies when both the employee AND the organization's systems are simultaneously doing redundant heavy work on the same problem — classic over-engineered enterprise software environments.

---

#### Milk Types
| Axis | Pole |
|---|---|
| E anchor 1 | Pure water (zero fat, zero processing) |
| E anchor 2 | Pure cream (maximum fat, minimum processing) |
| WD = 0 | Requires no preparation — drink directly |
| WD = 100 | Requires significant preparation before consumption |

Whole milk sits near the cream axis — low E. Skim milk sits near the water axis — low E. A heavily processed oat milk analog with 47 ingredients sits near the diagonal — high E, and arguably moderate WD since the consumer still has to figure out what it actually is.

Barrier tax applies if refrigeration or special storage is required that a typical household wouldn't have.

---

#### Water Sources
| Axis | Pole |
|---|---|
| E anchor 1 | Pure H2O (distilled, zero minerals) |
| E anchor 2 | Natural spring water (mineral-rich, unprocessed) |
| WD = 0 | Ready to drink with no effort |
| WD = 100 | Requires significant treatment before safe consumption |

Tap water sits near the pure H2O axis in most developed cities — low E, low WD. Natural spring water sits near the other axis — low E, low WD. Brackish water that's neither pure nor mineral-rich in any useful way sits near the diagonal — high E. Floodwater has high WD (must be treated) and moderate E.

---

### 9.4 The Universal Barrier Tax

Across all domains, the barrier tax rule generalizes as:

> If accessing or meaningfully using this thing requires resources, tools, credentials, or prerequisites that a typical person in the relevant population would not have by default, WD += 20.

"Typical person in the relevant population" is intentionally context-dependent — a barrier tax for professional compilers differs from one for consumer milk.

### 9.5 GSCT is a Framework, Not a Fixed Scale

The key insight of GSCT is that **E and WD are always relative to domain-defined anchors.** The math never changes. What changes is what you're measuring distance *from*.

This means GSCT scores are only comparable *within* a domain, not across domains. A college with GSCT=40 cannot be directly compared to a programming language with SCT=40 — the anchors are different. But two colleges can be meaningfully compared against each other, and two languages can be meaningfully compared against each other.

Future work should establish canonical anchor definitions for common domains to enable consistent cross-researcher scoring.

---

*GSCT extension authored alongside SCT v1.0, May 2026.*

---

## 10. Types of GSCT

GSCT is not a single model but a family of models. As the evaluation problem grows more complex, GSCT extends into more powerful forms. This section catalogs the known types.

### 10.1 Taxonomy Overview

| Type | Formula | When to use |
|---|---|---|
| SCT | 0.7(E) + 0.3(\|WD\|) | Single domain, programming languages |
| GSCT | 0.7(E) + 0.3(\|WD\|) with domain-defined anchors | Any single-plane domain |
| Branched GSCT | GSCT(subject, P) with profile-conditional barrier tax | Same evaluation, different inputs per evaluator |
| Multi-Dimensional GSCT | $\sum_{i=1}^{n} w_i \cdot D_i(E_i, \|WD_i\|)$ | Multiple independent planes of evaluation |

---

### 10.2 SCT (Base)

The original model. Two anchors (machine code, human language), one plane, one score. Defined in sections 2–4. All other types are extensions of this.

---

### 10.3 GSCT (Generalized)

SCT applied to any domain by redefining what E and WD measure. The formula is identical — only the anchor definitions change. Defined in section 9.

A college, a milk type, a workplace, a water source — anything with two natural poles and a measurable labor distribution can be rated with GSCT.

---

### 10.4 Branched GSCT

The same GSCT evaluation applied with **profile-conditional barrier tax**. The subject being rated doesn't change — but the evaluator's profile changes which barrier tax applies.

Formally:

$$GSCT(S, P) = 0.7(E_S) + 0.3(|WD_S| + BT(S, P))$$

Where:
- $S$ = subject being rated (e.g. a college)
- $P$ = evaluator profile (e.g. merit scholar, need-based, full sticker)
- $BT(S, P)$ = barrier tax, which varies by profile

**Example — MIT vs Harvard for three student profiles:**

| College | Merit BT | Need BT | Sticker BT | GSCT Merit | GSCT Need | GSCT Sticker | Swing |
|---|---|---|---|---|---|---|---|
| MIT | +0 | +0 | +0 | 21.5 | 21.5 | 21.5 | **0** |
| Harvard | +0 | +0 | +20 | 34.5 | 34.5 | 40.5 | **6** |
| Carnegie Mellon | +0 | +10 | +20 | 22.4 | 25.4 | 28.4 | **6** |

MIT's swing is zero — its financial aid policy is unconditional, so the barrier tax never applies regardless of profile. Institutions with high swing are less accessible in practice than their academic reputation suggests.

**Key distinction from Multi-Dimensional GSCT:** Branched GSCT is still one plane. It doesn't add new axes of evaluation — it adjusts the existing score based on who is asking.

---

### 10.5 Multi-Dimensional GSCT

The most general form. Instead of one evaluation plane with two anchors, Multi-Dimensional GSCT uses **n independent planes**, each with its own domain-specific E and WD definition. The final score is a weighted sum across all planes.

$$GSCT_{MD} = \sum_{i=1}^{n} w_i \cdot D_i(E_i, |WD_i|) \quad \text{where} \quad \sum_{i=1}^{n} w_i = 1$$

Where each dimension $D_i$ is its own full GSCT evaluation:

$$D_i = 0.7(E_i) + 0.3(|WD_i|)$$

**n can be any positive integer.** 2 planes, 4 planes, 11 planes, 26 planes — the framework imposes no upper bound. In principle, a sufficiently complex subject could be evaluated across as many dimensions as needed, analogous to how Calabi-Yau manifolds in string theory encode extra compactified dimensions beyond the four we perceive — real, measurable, and affecting the outcome, but invisible unless deliberately sought.

**Example — college ranking across 4 planes:**

| Plane | What it measures | Example weight |
|---|---|---|
| D1: Academic | Research vs vocational identity, self-direction vs structure | 0.40 |
| D2: Financial | Accessibility, aid generosity, barrier tax profile | 0.30 |
| D3: Career | Placement structure vs self-directed job search | 0.20 |
| D4: Geographic | Urban opportunity access vs campus isolation | 0.10 |

$$GSCT_{MD} = 0.40 \cdot D_1 + 0.30 \cdot D_2 + 0.20 \cdot D_3 + 0.10 \cdot D_4$$

**Resulting ranking (Abu Shariff profile, 2026):**

| College | D1 | D2 | D3 | D4 | GSCT_MD |
|---|---|---|---|---|---|
| MIT | 21.5 | 8 | 12 | 9 | **15.9** |
| Caltech | 23.1 | 8 | 18 | 22 | **18.7** |
| Carnegie Mellon | 22.4 | 22 | 10 | 16 | **19.1** |
| Princeton | 23.5 | 10 | 22 | 28 | **20.9** |
| Harvard | 34.5 | 10 | 15 | 9 | **23.6** |
| Cornell | 33 | 12 | 14 | 32 | **24.8** |
| UPenn | 30 | 28 | 11 | 8 | **24.0** |
| Columbia | 35 | 32 | 14 | 5 | **27.1** |
| Yale | 40 | 10 | 20 | 24 | **29.2** |
| Brown | 35.5 | 26 | 28 | 30 | **31.4** |

**Weights are evaluator-defined.** A student prioritizing financial accessibility would increase $w_2$; a student prioritizing career placement would increase $w_3$. The same framework, different weights, produces a legitimately different ranking — and both are correct for their respective evaluator.

This is the fundamental advantage of Multi-Dimensional GSCT over single-axis rankings: it makes the evaluator's priorities explicit and adjustable rather than hidden inside an opaque aggregate score.

**Key distinction from Branched GSCT:** Multi-Dimensional GSCT adds genuinely new axes of evaluation. Branched GSCT is one plane with a conditional input. Multi-Dimensional GSCT is n planes with independent evaluations combined.

---

### 10.6 Combining Types

Branched and Multi-Dimensional GSCT are not mutually exclusive. Each dimension $D_i$ in a Multi-Dimensional evaluation can itself be branched — meaning barrier tax within D2 (financial) varies by student profile while D1 (academic) remains fixed.

This produces the most general form:

$$GSCT_{full}(S, P) = \sum_{i=1}^{n} w_i \cdot D_i(E_i, |WD_i| + BT_i(S, P))$$

Where $BT_i(S, P)$ is a dimension-specific, profile-conditional barrier tax. Most dimensions will have $BT_i = 0$ — barrier tax is primarily a financial and geographic phenomenon.

---

### 10.7 Future Types

The taxonomy is open. Future extensions might include:

- **Temporal GSCT** — scores that change over time as a subject evolves (a college's research culture shifting, a language gaining popularity)
- **Comparative GSCT** — relative scoring between two subjects rather than absolute scoring against anchors
- **Adversarial GSCT** — scoring a subject that actively resists evaluation (a language designed to confuse, an institution designed to obscure its true nature)

---

*Section 10 authored May 2026. Multi-Dimensional GSCT and Branched GSCT introduced as extensions of the base SCT framework.*

---

## 11. Fractional Planes — GSCT Under Incomplete Information

### 11.1 Motivation

Integer planes assume complete observability — you can fully evaluate every dimension you include. But real evaluation is rarely complete. You can research a college's academic reputation exhaustively. You cannot fully know its culture until you live it.

Fractional planes model this. A dimension you can partially observe contributes partially to the score.

This is not a flaw in the framework. It is the framework correctly modeling epistemic reality.

---

### 11.2 Fractional Dimensions in Mathematics

Fractional dimensions are not invented here. In fractal geometry, the Hausdorff dimension of a coastline is approximately 1.25 — more than a line (1), less than a plane (2). The coastline occupies a fractional dimensional space because it is too complex to be a line but too sparse to fill a plane.

GSCT borrows this concept. A fractional plane $d \in (0, 1)$ represents a dimension that exists and matters but cannot be fully observed. It contributes $d$ of its full weight to the final score.

---

### 11.3 Formal Definition

For a fractional plane at confidence $d_i \in (0, 1]$:

$$GSCT_{n}(S, P) = \sum_{i=1}^{\lfloor n \rfloor} w_i \cdot D_i + d_{frac} \cdot w_{frac} \cdot D_{frac}$$

Where:
- $\lfloor n \rfloor$ = number of full integer planes
- $d_{frac}$ = fractional confidence of the partial plane (e.g. 0.7 for n=2.7)
- $w_{frac}$ = weight assigned to the fractional dimension
- All weights still sum to 1: $\sum w_i + w_{frac} = 1$

More compactly, treating each plane's effective contribution as $d_i \cdot w_i$ where $d_i = 1$ for full planes:

$$GSCT_{n}(S, P) = \sum_{i=1}^{n} d_i \cdot w_i \cdot D_i(E_i, |WD_i| + BT_i(S, P))$$

This is the **true master equation** — it subsumes all previous forms:

| Conditions | Reduces to |
|---|---|
| $n=1$, $d_1=1$, $BT=0$ | Base SCT/GSCT |
| $n=1$, $d_1=1$, $BT(S,P)$ varies | Branched GSCT |
| $n>1$, all $d_i=1$, $BT=0$ | Multi-Dimensional GSCT |
| $n>1$, all $d_i=1$, $BT$ varies | Full GSCT |
| $n$ fractional, $d_i \in (0,1]$ | Fractional GSCT |

---

### 11.4 The Confidence Coefficient

$d_i$ is the **confidence coefficient** for plane i — how well you can actually observe and measure this dimension.

| $d_i$ | Interpretation |
|---|---|
| 1.0 | Fully observable — public data, verifiable facts |
| 0.7–0.9 | Mostly observable — visited, researched, but uncertain |
| 0.4–0.6 | Partially observable — secondhand information only |
| 0.1–0.3 | Barely observable — exists but almost unmeasurable |
| 0.0 | Unobserved — equivalent to excluding the plane |

As you gather more information, $d_i$ increases toward 1.0. The score **converges toward truth** as uncertainty collapses — exactly analogous to a quantum wavefunction collapsing on observation.

---

### 11.5 Key Property — Uncertainty Dampens Extremes

A fractional plane with $d_i < 1$ dampens extreme scores in that dimension. A subject with a very bad culture score (D3 = 42) is penalized less under fractional evaluation than integer evaluation, because you're not certain the score is accurate.

This is correct behavior. Extreme claims under high uncertainty should carry less weight.

Formally: as $d_i \to 0$, that dimension's contribution vanishes regardless of its score. As $d_i \to 1$, full contribution is restored.

---

### 11.6 Worked Example — College Ranking at n=2.7

Three dimensions, D3 fractional at confidence 0.7:

- **D1 Academic** — $d_1 = 1.0$, $w_1 = 0.45$ (fully observable)
- **D2 Financial** — $d_2 = 1.0$, $w_2 = 0.35$ (fully observable)
- **D3 Culture/Fit** — $d_3 = 0.7$, $w_3 = 0.20$ (researchable but not fully knowable until enrollment)

$$GSCT_{2.7} = 0.45 \cdot D_1 + 0.35 \cdot D_2 + 0.7 \times 0.20 \cdot D_3$$
$$= 0.45(D_1) + 0.35(D_2) + 0.14(D_3)$$

| College | D1 | D2 | D3 | GSCT_2.7 |
|---|---|---|---|---|
| MIT | 21.5 | 8 | 14 | **11.6** |
| Caltech | 23.1 | 8 | 16 | **13.2** |
| Princeton | 23.5 | 10 | 35 | **19.1** |
| Carnegie Mellon | 22.4 | 22 | 22 | **20.9** |
| Cornell | 33 | 12 | 28 | **23.7** |
| Harvard | 34.5 | 10 | 38 | **24.1** |
| Yale | 40 | 10 | 42 | **27.9** |
| UPenn | 30 | 28 | 40 | **29.2** |
| Brown | 35.5 | 26 | 36 | **29.9** |
| Columbia | 35 | 32 | 30 | **31.5** |

**What the fractional plane reveals:**

Princeton jumps from 4th to 3rd compared to integer n=4 ranking. Its culture score is extreme (high E — neither pure research nor pure social) but the 0.7 confidence dampens that penalty. Under full certainty Princeton's culture would hurt it more.

Carnegie Mellon drops slightly for the same reason — its bad culture score is also dampened, but not enough to overcome Princeton's stronger academic and financial scores.

**The fundamental insight:** fractional planes make the ranking more honest. You know MIT is academically and financially superior with high confidence. You are less certain about culture. The framework respects that uncertainty rather than pretending you know things you don't.

---

### 11.7 Zero Planes — The Undefined Case

If $n = 0$, the sum is empty and evaluates to 0. But this is not a score of zero complexity — it is an undefined evaluation. You have chosen no dimensions to measure on.

Formally:

$$GSCT_{full}(S,P)\bigg|_{n=0} = \text{undefined}$$

Evaluating something on zero criteria is not an evaluation. It is the computational equivalent of dividing by zero — and should be treated as such. In implementation, $n = 0$ should raise an error.

*Preposterous: NoDimensions — cannot evaluate on zero planes.*

---

### 11.8 Connection to Quantum Mechanics

The fractional plane framework has structural parallels to quantum mechanics that are more than superficial:

- **Wavefunction** — a superposition of states, each with a probability amplitude. Collapses to a definite state on observation. Analogous to a fractional plane collapsing to $d_i = 1$ as you gather information.

- **Schrödinger equation** — describes how quantum states evolve over time. Analogous to how GSCT scores evolve as confidence coefficients increase toward 1.

- **Dirac equation** — extended Schrödinger to be relativistically consistent, accidentally predicting antimatter as a mathematical side effect. The fractional plane extension similarly revealed that uncertainty dampens extremes as an emergent property of the math, not a deliberate design decision.

The master equation with fractional planes is therefore not just a complexity metric. It is a **framework for evaluation under uncertainty** — which is the condition under which all real decisions are made.

---

*Section 11 authored May 2026. Fractional plane extension introduced as the final generalization of the SCT framework.*
