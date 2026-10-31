# Shariff Complexity Theorem (SCT)
### A Formal Model for Programming Language Complexity
**Abu Shariff** · 2026

---

## Abstract

Existing complexity metrics such as cyclomatic complexity and Kolmogorov complexity measure the complexity of *code*. No widely accepted metric measures the complexity of a *language's design* itself: the cognitive burden it places on a learner, and how the labor is divided between programmer and compiler. The Shariff Complexity Theorem (SCT) fills this gap with three quantities. Esotericosity (E) is a geometric measure of how far a language sits from both machine semantics and conventional human-readable syntax. Work Distribution (WD) measures how much compiler-grade labor falls on the programmer, and extends into the complex plane for languages where programmer and compiler both struggle. A flat Barrier Tax (BT) accounts for practical inaccessibility. SCT combines them into one scalar, $SCT = 0.7E + 0.3|WD| + BT$, with a provable maximum of 118.4. The model generalizes to any domain (GSCT) and supports profile-conditional, multi-dimensional and fractional-confidence variants, and an optional verbosity extension. We rate 122 languages and formats; lower scores mean easier to learn.

---

## 1. Motivation

When a programmer asks "how hard is this language to learn?", existing tools give no satisfying answer. Cyclomatic complexity describes branching in a particular program. Kolmogorov complexity describes the compressibility of a string. Neither says whether a language forces you to manage memory by hand, reason about reduction rules, or treat whitespace as executable syntax.

SCT answers a different question: **given a language's design, how much does it demand of its programmer, and how alien is it to both machine and human thought?**

---

## 2. Esotericosity (E)

### 2.1 The plane

Esotericosity is measured on a 2D Cartesian plane with 0 ≤ x, y ≤ 100:

- The **X axis** is *machine-likeness*: closeness to raw machine semantics (registers, addresses, opcodes).
- The **Y axis** is *conventional readable syntax*: closeness to the English-like, structured syntax most programmers already read comfortably.
- The **origin (0, 0)** is **the void**: a point that is neither machine-like nor conventionally readable, the furthest anything can be from both anchors at once.

A language is plotted at (x, y) by the author's judgment (see Section 9 on subjectivity).

*[Figure 1: The esotericosity plane showing the four corners and representative languages. To be regenerated for the final layout.]*

### 2.2 Formula

$$E = 100\left(1 - \frac{\sqrt{x^2 + y^2}}{100\sqrt{2}}\right)$$

E is the normalized distance *from* the farthest corner: it equals 100 at the origin and 0 at (100, 100). Languages that are strongly machine-like (large x) or strongly readable (large y), or both, score low; languages that are neither score high. Python at (8, 95) has E = 32.6; Whitespace at (1, 1) has E = 99.0.

### 2.3 The four corners

| Corner | Meaning | Status |
|---|---|---|
| (0, 0) | The void. Occupied by **Lambda Calculus**, which is by definition neither machine-like nor conventionally readable | `Preposterous: OriginCannotRateItself` |
| (100, 100) | A language that is simultaneously maximally machine-like and maximally readable: E = 0, WD = 0, SCT = 0 | `Preposterous: ContradictoryLanguage` |
| (100, 0) | Pure binary | `Excluded: NotHumanWritable` |
| (0, 100) | Pure natural-language English | `Excluded: NotALanguage` |

A real esoteric language *near* the origin (Malbolge, Whitespace) is rateable and very esoteric. A language *exactly* at (0, 0) is not representable in this plane at all, in the same way a removable singularity has a limit but no value at the point itself.

---

## 3. Work Distribution (WD)

### 3.1 The employer analogy

Three parties are involved in any programming task:

- **The computer**, the employer, who defines what must ultimately happen.
- **The compiler/runtime**, an experienced employee who handles complex, fast, low-level work.
- **The programmer**, a rookie employee who does slower, higher-level work.

WD measures how much *compiler-grade labor* is pushed onto the programmer.

- **Low WD:** the compiler absorbs the complexity (Python, AC).
- **High WD:** the programmer does compiler-grade work by hand. Assembly is defined as WD = 100.
- **Complex WD:** programmer and compiler struggle simultaneously, producing redundant suffering on both sides.

### 3.2 Real-valued WD

For most languages WD is a real number:

$$0 \leq WD \leq 100$$

Assembly is **defined** as WD = 100, the fixed upper anchor of the real scale. Barrier tax is *not* part of WD (see Section 4).

### 3.3 Complex WD

When a language forces the programmer to do compiler-grade work *and* forces the compiler into heavy decoding work, both parties suffer redundantly and the case leaves the real line:

$$WD = a + bi$$

- **a** is programmer labor (the real component).
- **b** is shared/compiler suffering (the imaginary component).

The effective magnitude is

$$|WD| = \sqrt{a^2 + b^2}$$

Pathological languages leave the normal abstraction line, so their complexity is a *distance* in 2D labor space rather than a scalar. With a ≤ 100 and b ≤ 100, the maximum magnitude is $100\sqrt{2} \approx 141.4$.

### 3.4 The WD holes

- **(0, 0), `Preposterous: NoCode`.** Zero programmer work *and* zero compiler work means the computer turns on, finds nothing, and refuses to proceed. This is an absence of a language, not a perfect one.
- **The diagonal ceiling (100, 100).** Maximum on both components, |WD| = 141.4. Only Whitespace and Lambda Calculus sit here (Section 5.3).

---

## 4. Barrier Tax (BT)

Some languages are hard to *reach* regardless of their design. SCT adds a **flat** tax to the final score, outside WD:

$$BT = \begin{cases} 0 & \text{free and accessible} \\ +6 & \text{paid, special input, required prerequisites, or machinery that runs poorly on ordinary hardware} \end{cases}$$

Examples:

- **+6:** MATLAB (paid license), LabVIEW (paid license), HolyC (requires TempleOS), APL (special input method), Minecraft Redstone (a VM inside a game), Enigma ASM, Punchcard Binary (physical equipment), Lambda Calculus (Section 5.3).
- **0:** Python, COBOL, C, AC (all freely available on ordinary hardware).

Two further statuses are not scores but verdicts:

- `Preposterous: LanguageExtinct`: the required machinery is extinct and undocumented.
- `Preposterous: Classified`: the language is legally inaccessible.

The value 6 is equivalent to a 20-point penalty on WD after the 0.3 weighting (20 × 0.3 = 6). Applying it flat, after weighting, keeps WD a pure measure of labor and is easier to reason about. Intermediate values (for example +3) appear only in the Branched variant (Section 10.2).

---

## 5. The SCT Formula

$$\boxed{SCT = 0.7(E) + 0.3(|WD|) + BT}$$

where

- E = 100(1 − √(x² + y²)/(100√2)),
- |WD| = WD for real values, √(a² + b²) for complex values,
- BT ∈ {0, +6}.

The 0.7/0.3 weighting reflects the working hypothesis that syntactic alienness (E) is the dominant driver of learnability difficulty and labor distribution a significant but secondary one. Lower scores mean easier. SCT may exceed 100 for pathological languages; this is intentional.

### 5.1 The ceiling

The maximum possible score occurs at E = 100, |WD| = 100√2, BT = 6:

$$SCT_{max} = 0.7(100) + 0.3(141.4) + 6 = 70 + 42.4 + 6 = \textbf{118.4}$$

### 5.2 Who holds it

Only Lambda Calculus can reach the ceiling, because (0, 0) is by definition its location. Its status label remains `Preposterous: OriginCannotRateItself`; the 118.4 is the formal upper bound of the scale, not a ranking claim.

### 5.3 Why Lambda Calculus carries BT = +6

Lambda Calculus incurs the tax on two independent grounds. First, its notation (λ) requires a special input method. Second, using it requires prerequisites a typical learner lacks: formal logic, Church encoding and beta reduction. Either ground alone suffices.

**Whitespace** is the useful comparison: it sits at (1, 1) with maximal complex WD (100 + 100i), but it is free and interpretable on any machine, so BT = 0. Its score of **111.7** is Lambda Calculus *without* the barrier tax. The 6.7-point gap between them is 0.7 from E (99.0 vs 100.0) and 6.0 from BT.

As a *theoretical computation model* evaluated under GSCT (Section 10.1) rather than as a programming language, Lambda Calculus scores 34.0.

---

## 6. Worked Examples

### 6.1 Python
- Coordinate: (8, 95). E = 100(1 − √(8² + 95²)/141.4) = **32.6**
- WD: The compiler absorbs almost everything. WD = **10.0**
- Barrier tax: none (BT = 0).

$$SCT = 0.7(32.6) + 0.3(10.0) + 0 = 22.8 + 3.0 + 0 = \textbf{25.8}$$

Python is the baseline of the readable end of the scale.

### 6.2 COBOL
- Coordinate: (10, 92). E = 100(1 − √(10² + 92²)/141.4) = **34.6**
- WD: `MOVE x TO y` is nearly Python-level readable; the programmer carries only the explicit block-closing tax (`END-IF`, `END-WHILE`). WD = **20.0**
- Barrier tax: none (BT = 0).

$$SCT = 0.7(34.6) + 0.3(20.0) + 0 = 24.2 + 6.0 + 0 = \textbf{30.2}$$

COBOL's reputation for difficulty is an ecosystem and tooling problem, not a language-design one; SCT measures design only.

### 6.3 AC
- Coordinate: (20, 85). E = 100(1 − √(20² + 85²)/141.4) = **38.3**
- WD: The compiler handles 14 output targets automatically, a large labor absorption. WD = **20.0**
- Barrier tax: none (BT = 0).

$$SCT = 0.7(38.3) + 0.3(20.0) + 0 = 26.8 + 6.0 + 0 = \textbf{32.8}$$

AC is the author's own language. It is included as a worked example and disclosed as a conflict of interest in Section 11.

### 6.4 Assembly (x86)
- Coordinate: (95, 18). E = 100(1 − √(95² + 18²)/141.4) = **31.6**
- WD: Defined anchor. WD = **100.0**
- Barrier tax: none (BT = 0).

$$SCT = 0.7(31.6) + 0.3(100.0) + 0 = 22.1 + 30.0 + 0 = \textbf{52.1}$$

### 6.5 MATLAB
- Coordinate: (18, 65). E = 100(1 − √(18² + 65²)/141.4) = **52.3**
- WD: Moderate programmer effort. WD = **35.0**
- Barrier tax: **+6**. Closed-source, paid license.

$$SCT = 0.7(52.3) + 0.3(35.0) + 6 = 36.6 + 10.5 + 6 = \textbf{53.1}$$

### 6.6 INTERCAL
- Coordinate: (12, 50). E = 100(1 − √(12² + 50²)/141.4) = **63.6**
- WD: The programmer does heavy manual work while the compiler must also decode the `PLEASE` politeness mechanism. Complex WD = 75 + 40i, so |WD| = √(75² + 40²) = **85.0**
- Barrier tax: none (BT = 0).

$$SCT = 0.7(63.6) + 0.3(85.0) + 0 = 44.5 + 25.5 + 0 = \textbf{70.0}$$

### 6.7 Malbolge
- Coordinate: (1, 2). E = 100(1 − √(1² + 2²)/141.4) = **98.4**
- WD: Programmer and interpreter both suffer; the language is ternary rather than binary. Complex WD = 90 + 80i, so |WD| = √(90² + 80²) = **120.4**
- Barrier tax: none (BT = 0).

$$SCT = 0.7(98.4) + 0.3(120.4) + 0 = 68.9 + 36.1 + 0 = \textbf{105.0}$$

Malbolge exceeds 100 because it is pathological on both E and WD at once.

### 6.8 Whitespace
- Coordinate: (1, 1). E = 100(1 − √(1² + 1²)/141.4) = **99.0**
- WD: Maximal on both components: the programmer must mentally parse invisible syntax and the interpreter must decode it. Complex WD = 100 + 100i, so |WD| = √(100² + 100²) = **141.4**
- Barrier tax: none (BT = 0).

$$SCT = 0.7(99.0) + 0.3(141.4) + 0 = 69.3 + 42.4 + 0 = \textbf{111.7}$$

### 6.9 Lambda Calculus
- Coordinate: (0, 0). E = 100(1 − √(0² + 0²)/141.4) = **100.0**
- WD: Maximal on both components. Complex WD = 100 + 100i, so |WD| = √(100² + 100²) = **141.4**
- Barrier tax: **+6**. Both grounds of Section 5.3 apply.

$$SCT = 0.7(100.0) + 0.3(141.4) + 6 = 70.0 + 42.4 + 6 = \textbf{118.4}$$

This is the ceiling of Section 5.1.

---

## 7. Summary Table

| Language | (x, y) | E | \|WD\| | BT | SCT |
|---|---|---|---|---|---|
| Python | (8, 95) | 32.6 | 10.0 | +0 | **25.8** |
| COBOL | (10, 92) | 34.6 | 20.0 | +0 | **30.2** |
| **AC** | (20, 85) | 38.3 | 20.0 | +0 | **32.8** |
| Go | (45, 70) | 41.2 | 25.0 | +0 | **36.3** |
| Java | (30, 68) | 47.4 | 30.0 | +0 | **42.2** |
| C | (75, 30) | 42.9 | 45.0 | +0 | **43.5** |
| Rust | (68, 50) | 40.3 | 55.0 | +0 | **44.7** |
| Assembly x86 | (95, 18) | 31.6 | 100.0 | +0 | **52.1** |
| MATLAB | (18, 65) | 52.3 | 35.0 | +6 | **53.1** |
| INTERCAL | (12, 50) | 63.6 | 85.0 | +0 | **70.0** |
| Malbolge | (1, 2) | 98.4 | 120.4 | +0 | **105.0** |
| Whitespace | (1, 1) | 99.0 | 141.4 | +0 | **111.7** |
| Lambda Calculus | (0, 0) | 100.0 | 141.4 | +6 | **118.4** |

The full 122-entry ranking is in Appendix A.

---

## 8. The Landscape

Across the 122 rated entries, scores run from 22.1 (Scratch Jr) to 118.4 (Lambda Calculus).

| Band | Count | Character |
|---|---|---|
| below 30 | 7 | Beginner and high-level languages |
| 30 to 45 | 43 | Mainstream general-purpose languages |
| 45 to 60 | 40 | Systems, hardware-description and low-level languages |
| 60 to 90 | 22 | Functional, logic and esoteric languages |
| 90 and above | 10 | Extreme esolangs and the ceiling cases |

Two results are worth noting. Assembly (52.1) scores well below the esolangs because its E is low: it is machine-like, and SCT treats *alien* syntax as harder than *demanding* syntax. And the functional languages (Haskell 66.7, OCaml, Standard ML) land higher than C: their difficulty is largely conceptual distance from both anchors, which SCT's E captures.

---

## 9. Limitations (Part I): The Core Model

**Subjectivity of coordinates.** E depends on the author's placement of each language on the plane. A formal rubric is needed, for example scoring axes by English-keyword presence, direct memory-address exposure and syntactic distance from formal grammars.

**WD estimates.** Real WD values are author estimates anchored to Assembly = 100.

**Weighting (0.7/0.3).** The weights are motivated by intuition and need validation against learner data.

**Scores above 100.** For languages that are pathological on both E and WD, SCT exceeds 100. This is a feature, bounded by the 118.4 ceiling.

**Barrier tax is binary in the core model.** Real accessibility is graded; Section 10.2 relaxes this.

---

## 10. GSCT: Generalizing Beyond Languages

### 10.1 General GSCT

SCT's structure does not depend on programming languages. Redefine the two axes and WD for another domain and the same formula applies. For *colleges*: X = pure-vocational ↔ Y = pure-research; WD = institutional structure ↔ self-direction required of the student; BT = tuition inaccessibility. Other demonstrated domains include workplaces, milk types (X = pure water ↔ Y = pure cream) and water sources.

**Edge case.** With n = 0 evaluation dimensions the sum is empty, which is undefined rather than zero: `Preposterous: NoDimensions`. Evaluating on zero criteria is not an evaluation.

### 10.2 Branched GSCT

Branched GSCT keeps the subject fixed and makes the barrier tax depend on the evaluator's *profile* P:

$$GSCT(S, P) = 0.7E + 0.3|WD| + BT(S, P)$$

A subject whose accessibility does not depend on the person has a *swing* of zero. Illustration with MATLAB (E = 52.3, |WD| = 35.0, base = 47.1):

| Profile | BT | Score |
|---|---|---|
| University license provided | +0 | 47.1 |
| Discounted student license | +3 | 50.1 |
| Individual paid license | +6 | 53.1 |

The +3 row shows why intermediate values appear here but not in the core model: a profile can be *partially* blocked. The core table in Appendix A uses the worst-case profile (+6) for MATLAB.

### 10.3 Multi-Dimensional GSCT

Many subjects need several independent *planes*. With n planes, each itself a full mini-GSCT scoring D_i, and evaluator-chosen weights summing to 1:

$$GSCT = \sum_{i=1}^{n} w_i D_i, \qquad \sum w_i = 1$$

Setting n = 1 recovers base SCT, which verifies backward compatibility (analogous to Newtonian mechanics as the low-velocity limit of relativity).

### 10.4 Fractional planes and confidence

Some dimensions are less observable than others: academic reputation is highly knowable, whereas campus "culture and fit" is only partly knowable. Each plane receives a confidence coefficient d_i ∈ (0, 1]:

$$GSCT_{full}(S, P) = \sum_i d_i \cdot w_i \cdot D_i\big(E_i,\ |WD_i| + BT_i(S, P)\big)$$

Because n may now be non-integer (n = Σd_i), the effect is to *dampen extremes*: an extreme score in a low-confidence dimension contributes less than the same score at full confidence. Setting every d_i = 1 gives the integer case; setting n = 1, d = 1, BT = 0 gives base SCT.

### 10.5 Worked example: ranking ten universities (n = 4)

Four planes, weights 0.40 / 0.30 / 0.20 / 0.10, scored for the author's own profile (lower is better). All D values are the author's estimates.

| University | Academic (0.40) | Financial (0.30) | Career (0.20) | Geographic (0.10) | GSCT |
|---|---|---|---|---|---|
| MIT | 21.5 | 8 | 12 | 9 | **14.3** |
| Caltech | 23.1 | 8 | 18 | 22 | **17.4** |
| Carnegie Mellon | 22.4 | 22 | 10 | 16 | **19.2** |
| Princeton | 23.5 | 10 | 22 | 28 | **19.6** |
| Harvard | 34.5 | 10 | 15 | 9 | **20.7** |
| Cornell | 33 | 12 | 14 | 32 | **22.8** |
| UPenn | 30 | 28 | 11 | 8 | **23.4** |
| Yale | 40 | 10 | 20 | 24 | **25.4** |
| Columbia | 35 | 32 | 14 | 5 | **26.9** |
| Brown | 35.5 | 26 | 28 | 30 | **30.6** |

MIT scores lowest, driven by its financial plane (8), because its aid is need-blind and full. Carnegie Mellon (19.2) edges Princeton (19.6) by 0.4 here.

### 10.6 Worked example: fractional planes (n = 2.7)

Three planes: Academic (w = 0.45, d = 1.0), Financial (w = 0.35, d = 1.0) and Culture/Fit (w = 0.20, d = 0.7), so n = 1.0 + 1.0 + 0.7 = 2.7.

| University | Academic | Financial | Culture/Fit | GSCT (d = 0.7 on Fit) | Same planes at d = 1.0 |
|---|---|---|---|---|---|
| MIT | 21.5 | 8 | 14 | **14.4** | 15.3 |
| Caltech | 23.1 | 8 | 16 | **15.4** | 16.4 |
| Princeton | 23.5 | 10 | 35 | **19.0** | 21.1 |
| Carnegie Mellon | 22.4 | 22 | 22 | **20.9** | 22.2 |
| Cornell | 33 | 12 | 28 | **23.0** | 24.6 |
| Harvard | 34.5 | 10 | 38 | **24.3** | 26.6 |
| Yale | 40 | 10 | 42 | **27.4** | 29.9 |
| UPenn | 30 | 28 | 40 | **28.9** | 31.3 |
| Brown | 35.5 | 26 | 36 | **30.1** | 32.3 |
| Columbia | 35 | 32 | 30 | **31.1** | 33.0 |

Princeton (19.0) ranks ahead of Carnegie Mellon (20.9). Two effects contribute, and they should be kept apart. First, the plane set changes: Princeton's weakness in the n = 4 table was its Career and Geographic scores, which this configuration drops. That alone already puts Princeton ahead (21.1 vs 22.2 at full confidence). Second, confidence dampening widens the gap (from 1.1 to 1.9 points) because Princeton's higher Culture/Fit score (35 vs 22) is discounted more. MIT ranks first in every configuration tested.

### 10.7 Verbosity extension

SCT as defined does not distinguish a concise language from a wordy one at the same level of difficulty. The optional verbosity extension adds a third measure V ∈ [0, 100], where V = 0 is maximally concise (APL-like) and V = 100 is maximally verbose (COBOL-like):

$$SCT_V = 0.6E + 0.25|WD| + 0.15V + BT$$

The weights again sum to 1 (before BT). V values are author estimates.

| Language | E | \|WD\| | V | SCT | SCT_V |
|---|---|---|---|---|---|
| Python | 32.6 | 10.0 | 15 | 25.8 | **24.3** |
| COBOL | 34.6 | 20.0 | 95 | 30.2 | **40.0** |
| Java | 47.4 | 30.0 | 75 | 42.2 | **47.2** |
| Haskell | 65.3 | 70.0 | 15 | 66.7 | **58.9** |
| Julia | 46.1 | 25.0 | 38 | 39.8 | **39.6** |
| INTERCAL | 63.6 | 85.0 | 80 | 70.0 | **71.4** |

Verbosity separates cases SCT alone treats alike. COBOL rises from 30.2 to 40.0 because wordiness is its real cost, while Haskell falls from 66.7 to 58.9 because its conciseness offsets some of its conceptual distance. Python is barely changed.

---

## 11. Limitations (Part II): Extensions and Validation

**College and multi-dimensional scores are subjective.** Every D value in Sections 10.5 and 10.6, and every weight, is the author's estimate for one profile. The tables illustrate the method; they are not a ranking of universities.

**Confidence coefficients are assigned, not measured.** The Bayesian analogy for d_i is a motivation, not a derivation.

**Verbosity values are estimates**, and the 0.6/0.25/0.15 weights are untested.

**Conflict of interest.** AC is the author's own language and appears in the rankings. Its coordinates (20, 85) and WD (20) were set using the same rubric as every other entry, but independent re-rating is advisable.

**Extending to paradigms.** SCT currently rates languages; a natural extension rates *paradigms* (functional, logic, concatenative) independently of syntax and combines the scores.

**Empirical validation.** SCT predictions should be tested against measured time-to-proficiency data for programmers learning new languages. This is the most important piece of future work.

---

## 12. Conclusion

SCT provides a lightweight, geometrically grounded framework for comparing language complexity. By separating syntactic alienness (E, a distance from the farthest corner of the plane) from labor distribution (WD, extended into the complex plane for pathological cases) and treating accessibility as a flat barrier tax, it captures dimensions of difficulty that existing metrics miss. The scale is bounded: the maximum is exactly 118.4, attained only at the origin. The same structure generalizes to any domain as GSCT, with profile-conditional, multi-dimensional and fractional-confidence variants. The model is simple enough to apply by hand yet produces consistent scores across languages as different as Python, Assembly and Malbolge, and it requires empirical validation before its predictions can be called predictive.

---

*Abu Shariff · 2026*
*Reference implementation of AC: github.com/AbuCodingAI/aclang*

---

## Appendix A: Complete Rankings (lowest = easiest)

All 122 entries, computed with SCT = 0.7E + 0.3|WD| + BT. Complex WD entries are marked with their a + bi components in the notes.

| Rank | Language | (x, y) | E | \|WD\| | BT | SCT | Notes |
|---|---|---|---|---|---|---|---|
| 1 | Scratch Jr | (1, 98) | 30.7 | 2.0 | +0 | 22.1 |  |
| 2 | Scratch | (3, 95) | 32.8 | 4.0 | +0 | 24.2 |  |
| 3 | Python | (8, 95) | 32.6 | 10.0 | +0 | 25.8 |  |
| 4 | MakeCode Arcade | (5, 90) | 36.3 | 6.0 | +0 | 27.2 |  |
| 5 | Grace | (8, 88) | 37.5 | 8.0 | +0 | 28.6 |  |
| 6 | Logo | (5, 88) | 37.7 | 8.0 | +0 | 28.8 |  |
| 7 | Ruby | (8, 90) | 36.1 | 12.0 | +0 | 28.9 |  |
| 8 | COBOL | (10, 92) | 34.6 | 20.0 | +0 | 30.2 |  |
| 9 | QBASIC | (12, 85) | 39.3 | 14.0 | +0 | 31.7 |  |
| 10 | HTML | (5, 82) | 41.9 | 8.0 | +0 | 31.7 |  |
| 11 | YAML | (5, 82) | 41.9 | 8.0 | +0 | 31.7 |  |
| 12 | BASIC | (12, 85) | 39.3 | 15.0 | +0 | 32.0 |  |
| 13 | **AC** | (20, 85) | 38.3 | 20.0 | +0 | 32.8 | author's own language; 14 compile targets |
| 14 | Crystal | (28, 80) | 40.1 | 20.0 | +0 | 34.1 |  |
| 15 | Visual Basic 6 | (15, 82) | 41.1 | 18.0 | +0 | 34.2 |  |
| 16 | XML | (5, 78) | 44.7 | 10.0 | +0 | 34.3 |  |
| 17 | SQL | (8, 80) | 43.1 | 15.0 | +0 | 34.7 |  |
| 18 | VB.NET | (20, 82) | 40.3 | 22.0 | +0 | 34.8 |  |
| 19 | Groovy | (25, 78) | 42.1 | 20.0 | +0 | 35.5 |  |
| 20 | Fish Shell | (10, 80) | 43.0 | 18.0 | +0 | 35.5 |  |
| 21 | Lua | (20, 78) | 43.1 | 18.0 | +0 | 35.6 |  |
| 22 | V language | (40, 72) | 41.8 | 22.0 | +0 | 35.9 |  |
| 23 | Go | (45, 70) | 41.2 | 25.0 | +0 | 36.3 |  |
| 24 | Dart | (18, 78) | 43.4 | 20.0 | +0 | 36.4 |  |
| 25 | MySQL | (10, 78) | 44.4 | 18.0 | +0 | 36.5 |  |
| 26 | Kotlin | (28, 75) | 43.4 | 22.0 | +0 | 37.0 |  |
| 27 | JSONiq | (8, 75) | 46.7 | 15.0 | +0 | 37.2 |  |
| 28 | JavaScript | (12, 78) | 44.2 | 22.0 | +0 | 37.5 |  |
| 29 | TypeScript | (15, 76) | 45.2 | 20.0 | +0 | 37.6 |  |
| 30 | GraphQL | (10, 75) | 46.5 | 18.0 | +0 | 37.9 |  |
| 31 | C# | (35, 72) | 43.4 | 28.0 | +0 | 38.8 |  |
| 32 | Nim | (35, 70) | 44.7 | 25.0 | +0 | 38.8 |  |
| 33 | CSS | (5, 70) | 50.4 | 12.0 | +0 | 38.9 |  |
| 34 | Tkinter | (8, 78) | 44.6 | 28.0 | +0 | 39.6 | Python wrapper |
| 35 | Swift | (30, 72) | 44.8 | 28.0 | +0 | 39.8 |  |
| 36 | Julia | (25, 72) | 46.1 | 25.0 | +0 | 39.8 |  |
| 37 | T-SQL | (16, 74) | 46.5 | 26.0 | +0 | 40.3 |  |
| 38 | Delphi | (32, 70) | 45.6 | 32.0 | +0 | 41.5 |  |
| 39 | PL/SQL | (18, 72) | 47.5 | 28.0 | +0 | 41.6 |  |
| 40 | Java | (30, 68) | 47.4 | 30.0 | +0 | 42.2 |  |
| 41 | PowerShell | (18, 72) | 47.5 | 30.0 | +0 | 42.2 |  |
| 42 | Elm | (12, 72) | 48.4 | 30.0 | +0 | 42.9 |  |
| 43 | R | (15, 70) | 49.4 | 28.0 | +0 | 43.0 |  |
| 44 | PHP | (15, 68) | 50.8 | 25.0 | +0 | 43.1 |  |
| 45 | C | (75, 30) | 42.9 | 45.0 | +0 | 43.5 |  |
| 46 | Pascal | (30, 68) | 47.4 | 35.0 | +0 | 43.7 |  |
| 47 | Rust | (68, 50) | 40.3 | 55.0 | +0 | 44.7 |  |
| 48 | C++ | (78, 32) | 40.4 | 55.0 | +0 | 44.8 |  |
| 49 | Zig | (80, 35) | 38.3 | 60.0 | +0 | 44.8 |  |
| 50 | Modula-2 | (35, 65) | 47.8 | 38.0 | +0 | 44.9 | Pascal successor by Wirth |
| 51 | Z# (AstroSam) | (28, 72) | 45.4 | 44.7 | +0 | 45.2 | WD = 40 + 20i; compiler crashes unpredictably, no docs |
| 52 | D | (60, 45) | 47.0 | 45.0 | +0 | 46.4 |  |
| 53 | Solidity | (30, 65) | 49.4 | 40.0 | +0 | 46.6 |  |
| 54 | Zsh | (20, 65) | 51.9 | 35.0 | +0 | 46.8 |  |
| 55 | Octave | (18, 65) | 52.3 | 35.0 | +0 | 47.1 |  |
| 56 | HolyC | (70, 60) | 34.8 | 58.0 | +6 | 47.8 | BT+6, requires TempleOS |
| 57 | Eiffel | (25, 65) | 50.8 | 42.0 | +0 | 48.2 |  |
| 58 | BCPL | (72, 28) | 45.4 | 55.0 | +0 | 48.3 | grandfather of C, 1967 |
| 59 | Elixir | (18, 65) | 52.3 | 40.0 | +0 | 48.6 |  |
| 60 | Move | (35, 60) | 50.9 | 45.0 | +0 | 49.1 |  |
| 61 | Ada | (45, 55) | 49.8 | 50.0 | +0 | 49.9 |  |
| 62 | Scala | (30, 60) | 52.6 | 45.0 | +0 | 50.3 |  |
| 63 | Tcl | (15, 60) | 56.3 | 38.0 | +0 | 50.8 |  |
| 64 | WebAssembly | (85, 20) | 38.3 | 80.0 | +0 | 50.8 |  |
| 65 | VC++ | (80, 25) | 40.7 | 74.3 | +0 | 50.8 | WD = 68 + 30i; MS extensions vs ISO |
| 66 | Bash | (22, 60) | 54.8 | 42.0 | +0 | 51.0 |  |
| 67 | Objective-C | (50, 45) | 52.4 | 48.0 | +0 | 51.1 |  |
| 68 | RISC-V ASM | (88, 25) | 35.3 | 88.0 | +0 | 51.1 | cleaner than x86 |
| 69 | LLVM IR | (88, 15) | 36.9 | 85.0 | +0 | 51.3 |  |
| 70 | Assembly x86 | (95, 18) | 31.6 | 100.0 | +0 | 52.1 | WD anchor = 100 |
| 71 | NASM | (94, 20) | 32.0 | 99.0 | +0 | 52.1 |  |
| 72 | MASM | (93, 22) | 32.4 | 98.0 | +0 | 52.1 |  |
| 73 | ARM Assembly | (93, 18) | 33.0 | 98.0 | +0 | 52.5 |  |
| 74 | F# | (25, 60) | 54.0 | 50.0 | +0 | 52.8 |  |
| 75 | Debian Bash | (22, 60) | 54.8 | 48.0 | +0 | 52.8 |  |
| 76 | ELF wrapper | (92, 5) | 34.9 | 95.0 | +0 | 52.9 | hand-writable binary |
| 77 | PE wrapper | (92, 5) | 34.9 | 95.0 | +0 | 52.9 | hand-writable binary |
| 78 | Mach-O wrapper | (92, 5) | 34.9 | 95.0 | +0 | 52.9 | Apple executable format |
| 79 | Awk | (20, 55) | 58.6 | 40.0 | +0 | 53.0 |  |
| 80 | MATLAB | (18, 65) | 52.3 | 35.0 | +6 | 53.1 | BT+6, paid license |
| 81 | Smalltalk | (12, 60) | 56.7 | 45.0 | +0 | 53.2 |  |
| 82 | Fortran | (40, 45) | 57.4 | 48.0 | +0 | 54.6 |  |
| 83 | Clojure | (12, 55) | 60.2 | 48.0 | +0 | 56.5 |  |
| 84 | Punchcard Binary | (100, 0) | 29.3 | 100.0 | +6 | 56.5 | BT+6, requires punchcard equipment |
| 85 | SystemVerilog | (52, 40) | 53.6 | 65.0 | +0 | 57.0 |  |
| 86 | Self | (10, 55) | 60.5 | 50.0 | +0 | 57.3 |  |
| 87 | Forth | (55, 30) | 55.7 | 65.0 | +0 | 58.5 |  |
| 88 | Erlang | (20, 52) | 60.6 | 55.0 | +0 | 58.9 |  |
| 89 | Factor | (45, 35) | 59.7 | 58.0 | +0 | 59.2 |  |
| 90 | Verilog | (50, 38) | 55.6 | 68.0 | +0 | 59.3 |  |
| 91 | Perl | (20, 55) | 58.6 | 64.0 | +0 | 60.2 | WD = 50 + 40i |
| 92 | Tcl/Tk ecosystem | (10, 45) | 67.4 | 45.0 | +0 | 60.7 | encountered via Python errors |
| 93 | OCaml | (18, 52) | 61.1 | 60.0 | +0 | 60.8 |  |
| 94 | Racket | (12, 48) | 65.0 | 52.0 | +0 | 61.1 |  |
| 95 | Standard ML | (15, 50) | 63.1 | 62.0 | +0 | 62.8 |  |
| 96 | Makefile | (30, 40) | 64.6 | 60.0 | +0 | 63.2 |  |
| 97 | VHDL | (45, 35) | 59.7 | 72.0 | +0 | 63.4 |  |
| 98 | Common Lisp | (12, 45) | 67.1 | 55.0 | +0 | 63.5 |  |
| 99 | LabVIEW | (15, 50) | 63.1 | 45.0 | +6 | 63.7 | BT+6, requires NI license |
| 100 | PostScript | (50, 20) | 61.9 | 70.0 | +0 | 64.3 |  |
| 101 | Lisp | (10, 42) | 69.5 | 58.0 | +0 | 66.0 |  |
| 102 | Scheme | (10, 42) | 69.5 | 58.0 | +0 | 66.0 |  |
| 103 | Windows Batch | (18, 42) | 67.7 | 62.6 | +0 | 66.2 | WD = 55 + 30i; % quoting |
| 104 | Haskell | (10, 48) | 65.3 | 70.0 | +0 | 66.7 |  |
| 105 | INTERCAL | (12, 50) | 63.6 | 85.0 | +0 | 70.0 | WD = 75 + 40i; PLEASE mechanism |
| 106 | Sed | (25, 35) | 69.6 | 71.6 | +0 | 70.2 | WD = 65 + 30i |
| 107 | Perl5 regex soup | (20, 30) | 74.5 | 64.0 | +0 | 71.3 | WD = 50 + 40i; write-only |
| 108 | Chef | (5, 40) | 71.5 | 72.8 | +0 | 71.9 | WD = 70 + 20i |
| 109 | Prolog | (8, 35) | 74.6 | 72.0 | +0 | 73.8 |  |
| 110 | Shakespeare | (3, 35) | 75.2 | 73.5 | +0 | 74.7 | WD = 72 + 15i |
| 111 | Mercury | (10, 32) | 76.3 | 75.0 | +0 | 75.9 |  |
| 112 | Befunge | (10, 15) | 87.3 | 85.5 | +0 | 86.8 | WD = 78 + 35i |
| 113 | Unlambda | (5, 8) | 93.3 | 94.3 | +0 | 93.6 | WD = 80 + 50i |
| 114 | Ook! | (3, 8) | 94.0 | 101.2 | +0 | 96.2 | WD = 85 + 55i |
| 115 | Piet | (3, 2) | 97.5 | 96.0 | +0 | 97.0 | WD = 82 + 50i |
| 116 | brain** **ck | (5, 5) | 95.0 | 104.0 | +0 | 97.7 | WD = 85 + 60i |
| 117 | APL | (5, 5) | 95.0 | 87.5 | +6 | 98.8 | WD = 75 + 45i; BT+6 special input |
| 118 | Minecraft Redstone | (8, 12) | 89.8 | 110.1 | +6 | 101.9 | WD = 85 + 70i; BT+6, complex WD, VM-in-VM |
| 119 | Enigma ASM | (2, 8) | 94.2 | 106.3 | +6 | 103.8 | WD = 80 + 70i; BT+6 |
| 120 | Malbolge | (1, 2) | 98.4 | 120.4 | +0 | 105.0 | WD = 90 + 80i; ternary not binary |
| 121 | Whitespace | (1, 1) | 99.0 | 141.4 | +0 | 111.7 | WD = 100 + 100i; Lambda Calculus without the barrier tax |
| 122 | Lambda Calculus | (0, 0) | 100.0 | 141.4 | +6 | 118.4 | WD = 100 + 100i; Preposterous: OriginCannotRateItself; GSCT 34.0 as a computation model |
