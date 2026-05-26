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

$$E = \min(x,\ y)$$

Where (x, y) is the language's coordinate on the esotericosity plane.

This formula has an elegant property: **languages on the diagonal y=x have E equal to their coordinate value, making the diagonal the natural scale of esotericosity itself.** A language at (50, 50) has E=50; at (75, 75) has E=75; at (100, 100) has E=100. The diagonal is not just where high-E languages live — it is the E scale.

| Language position | E value | Intuition |
|---|---|---|
| On the X axis (machine-like) | 0 | Close to machine semantics, not esoteric |
| On the Y axis (human-like) | 0 | Close to human semantics, not esoteric |
| On the diagonal y=x | equals coordinate value | Equally far from both anchors |
| ASM at (0, 0) | 0 | Defined anchor of the model |
| Malbolge at (100, 100) | 100 | Maximum esotericosity |

### 2.3 Example Coordinates

<img width="982" height="833" alt="Screenshot from 2026-05-25 16-08-56" src="https://github.com/user-attachments/assets/d141ed46-2db1-4a7b-a92e-049d97c62cb4" />

*[Figure 1: Esotericosity plane]*

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

<img width="723" height="777" alt="Screenshot from 2026-05-25 16-27-30" src="https://github.com/user-attachments/assets/81d92237-1f5e-49c9-a020-f93bc7dd9a7a" />

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

## 5. Worked Examples

### 5.1 Python
- Coordinate: (10, 90). E = **10**
- WD: Compiler absorbs almost everything. WD = **10**
- No barrier tax.

$$SCT = 0.7(10) + 0.3(10) = 7 + 3 = \mathbf{10}$$

### 5.2 ASM (x86-64)
- Coordinate: (0, 0). E = **0**
- WD: Defined anchor. WD = **100**
- No barrier tax.

$$SCT = 0.7(0) + 0.3(100) = 0 + 30 = \mathbf{30}$$

### 5.3 AC (A language family — compiled, 14 targets)
- Coordinate: (15, 75). E = **15**
- WD: Compiler handles 14 output targets automatically, massive labor absorption. WD = **20**
- No barrier tax.

$$SCT = 0.7(15) + 0.3(20) = 10.5 + 6 = \mathbf{16.5}$$

### 5.4 AI (A language family — interpreted, exact decimal, AIVM)
- Coordinate: (20, 70). E = **20**
- WD: AIVM absorbs all runtime complexity. WD = **16**
- No barrier tax.

$$SCT = 0.7(20) + 0.3(16) = 14 + 4.8 = \mathbf{18.8}$$

### 5.5 Lambda Calculus
- Coordinate: (20, 20). E = **20**
- WD: Programmer manually reasons about reduction, substitution, recursion encoding. WD = **70**
- No barrier tax.

$$SCT = 0.7(20) + 0.3(70) = 14 + 21 = \mathbf{35}$$

### 5.6 MATLAB
- Coordinate: (15, 70). E = **15**
- WD: Moderate programmer effort. WD = 40, +20 barrier tax → **60**

$$SCT = 0.7(15) + 0.3(60) = 10.5 + 18 = \mathbf{28.5}$$

### 5.7 Malbolge
- Coordinate: (100, 100). E = **100** (maximum)
- WD: Programmer and interpreter both suffer maximally. Complex WD = 90 + 80i
- $|WD| = \sqrt{90^2 + 80^2} = \sqrt{14500} \approx$ **120.4**
- No barrier tax.

$$SCT = 0.7(100) + 0.3(120.4) = 70 + 36.1 = \mathbf{106.1}$$

Malbolge intentionally exceeds 100 — it is the canonical example of a language designed to be impossible.

### 5.8 Whitespace
- Coordinate: (95, 95). E = **95**
- WD: Programmer must mentally parse invisible syntax. Complex WD = 80 + 60i
- $|WD| = \sqrt{80^2 + 60^2} = \sqrt{10000} =$ **100**

$$SCT = 0.7(95) + 0.3(100) = 66.5 + 30 = \mathbf{96.5}$$

### 5.9 SCT rating itself
- Coordinate: (5, 80). E = **5**
- WD: Human must classify, estimate coordinates, reason geometrically. Compiler does nothing — SCT is conceptual. WD = **40**

$$SCT = 0.7(5) + 0.3(40) = 3.5 + 12 = \mathbf{15.5}$$

SCT rates itself slightly easier than Python to understand, which feels correct.

---

## 6. Summary Table

| Language | E | \|WD\| | SCT |
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



### Glossary
## Glossary

| Term | Definition |
|---|---|
| **SCT** | Shariff Complexity Theorem — base model for rating programming language complexity using Esotericosity and Work Distribution |
| **GSCT** | General SCT — SCT extended to any domain by redefining E and WD anchors |
| **Branched GSCT** | GSCT with profile-conditional barrier tax — same subject, different score depending on who is evaluating |
| **Multi-Dimensional GSCT** | GSCT across n independent evaluation planes, weighted and combined into one score |
| **E (Esotericosity)** | Distance from both natural anchors — computed as min(x, y) on the cartesian plane |
| **WD (Work Distribution)** | How much expert-level labor is pushed onto the user vs absorbed by the system |
| **Barrier Tax** | +20 WD penalty when accessing a subject requires resources a typical person wouldn't have by default |
| **Complex WD** | WD as a+bi when both user and system suffer simultaneously — magnitude via Pythagorean theorem |
| **AC** | A compiled language targeting 14 output formats — [github.com/AbuCodingAI/aclang](https://github.com/AbuCodingAI/aclang) |
| **AI** | Interpreted member of the A language family — exact decimal arithmetic, runs on AIVM. Not Artificial Intelligence |
| **AC+** | Bare-metal systems language — targets x86-64 assembly, used for kernels and bootloaders |
| **AIVM** | AI Virtual Machine — runtime that executes compiled AI bytecode (.aibc) |
| **AOS** | Abu Operating System — kernel built on AC+ |
| **JaSQL** | Readable query language for AbuDB — [github.com/AbuCodingAI/JaSQL](https://github.com/AbuCodingAI/JaSQL) |
| **datac** | JaSQL's native file format — typed, multi-table, human-readable |
| **deltat** | Duration type (hh:mm:ss) — elapsed time, not a point in time |
| **Preposterous:** | Standard error prefix across the Abu Ecosystem, inherited from AC |
