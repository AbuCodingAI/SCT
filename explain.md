What is SCT? (The Version for Everyone)
By Abu Shariff, age 12 — 2026

The Simple Version
Have you ever tried to learn a new programming language and thought "why is this so hard?" Or looked at two colleges and couldn't figure out which one was actually better for you?
SCT — the Shariff Complexity Theorem — is a way to give that feeling a number.
Not just for programming languages. For anything.

Start Here: What Makes Something Hard to Learn?
Imagine you're trying to learn something new. Two things make it hard:
1. How weird is it?
Is it close to something you already understand — like plain English, or the way computers obviously work — or is it floating somewhere in the middle, belonging to neither world?
2. How much work are YOU doing that something else should be doing?
When you write Python, the computer figures out a lot of stuff for you. When you write Assembly, YOU have to figure out almost everything yourself. That's extra work being pushed onto you.
SCT measures both of these things and combines them into one score.
Low score = easier.
High score = harder.

The Two Axes (Don't Panic, It's Just a Graph)
Picture a graph. Like the ones you drew in math class.

The bottom axis goes from left to right: how close is this thing to the way computers actually work at the hardware level?
The side axis goes up and down: how close is this thing to plain English?

Assembly language sits at the origin — bottom left corner. It's as close to raw computer instructions as you can get while still being readable by humans.
Python sits near the top of the side axis — it reads almost like English. for item in list is basically just saying "for each item in the list."
Malbolge — a programming language literally designed to be impossible — sits near the top right corner of the diagonal line running through the middle. It's not close to computers AND it's not close to English. It belongs to neither world.
That diagonal line? Languages near it are the weirdest. Languages near either axis are the most normal.
The formula for weirdness (we call it Esotericosity, or E) is just:

E = the smaller of your two coordinates

If you're close to either axis, E is low. If you're floating near the diagonal, E is high. Simple.

The Employer Analogy (This One's Good)
Imagine three people:

The Computer is the boss. It decides what needs to happen.
The Compiler (the thing that translates your code) is an experienced employee. It does the complicated, fast, technical work.
You, the Programmer are the new employee. You do the simpler, slower work.

In Python, the compiler does enormous amounts of work for you. You just write what you want and it figures out the rest. Easy job.
In Assembly, YOU do almost everything yourself. You tell the computer exactly which memory address to use, exactly which register to store things in, exactly how to move every byte. That's exhausting.
Work Distribution (WD) measures how much of the experienced employee's job gets dumped on you.

Low WD = the compiler handles it. Easy.
High WD = you handle it. Hard.
Assembly is defined as WD = 100. The maximum.

The Weird Case: When BOTH of You Are Struggling
Sometimes a language is so confusing that YOU struggle AND the compiler struggles to figure out what you meant at the same time. Both employees are suffering simultaneously.
When that happens, normal numbers aren't enough. We use imaginary numbers — specifically, we write WD as something like 90 + 80i, where the imaginary part represents the compiler's suffering. Then we use the Pythagorean theorem to find the total "distance" from zero suffering.
This sounds wild. But it's mathematically coherent, and it correctly identifies languages like Malbolge as genuinely pathological — scoring over 100 on the SCT scale.

The Formula
SCT=0.7(E)+0.3(WD)SCT = 0.7(E) + 0.3(WD)SCT=0.7(E)+0.3(WD)
Weirdness matters more than work distribution — that's why E gets 70% of the weight. A language that's alien to both humans and machines is harder to learn than one that just makes you do a lot of work.
That's it. That's the core of SCT.

But Wait — It Works for Anything
Here's where it gets interesting.
The same framework works for colleges. Milk. Water. Workplaces. Anything where:

Two natural extremes exist
Someone has to do work that could be done for them

For colleges:

One extreme: pure trade school (totally practical, learn a specific job)
Other extreme: pure research university (totally theoretical, discover new knowledge)
Work Distribution: how much does the institution guide you vs. how much do you figure it out yourself?

A school that's neither practical nor research-focused — vague interdisciplinary programs, no clear direction — sits near the diagonal. High E. Confusing.
For milk:

One extreme: pure water
Other extreme: pure cream
A heavily processed oat milk with 47 ingredients? Near the diagonal. High E. What even is it?

We call this generalized version GSCT — General SCT.

The Barrier Tax
One more idea: some things are hard to access, not because they're complicated, but because they're expensive or require special equipment.
MATLAB costs money. Heavy Minecraft redstone setups require specific hardware. These aren't harder to understand — they're harder to access.
If something can't reasonably run on a normal computer without extra cost, its Work Distribution score goes up by 20.
That's the Barrier Tax.
Python has no barrier tax. It runs on anything, for free.

Types of SCT (It Goes Deeper)
Branched GSCT
The same college might be easy to access for one student (full scholarship) and hard for another (paying full price). Same college, different scores, different people.
Branched GSCT adjusts the barrier tax based on who's asking.
Multi-Dimensional GSCT
Why rate colleges on just one thing? You could rate them on:

Academic quality
Financial accessibility
Career outcomes
Geographic location
Campus culture

Each one is its own complete evaluation. Combine them with weights based on what YOU care about.
A student who cares about money weights financial higher. A student who cares about research weights academic higher. Same framework, different priorities, legitimately different results.
You can have 2 dimensions. 4. 11. 26. As many as you need. We call this Multi-Dimensional GSCT.
Fractional Planes (The Really Interesting Part)
Here's the problem: you can fully research a college's tuition. You cannot fully know its culture until you actually live there.
So what if a dimension is only partially observable?
You give it a confidence coefficient between 0 and 1. A dimension you know perfectly gets 1.0. A dimension you can only partially observe gets 0.7, or 0.5, or whatever reflects your uncertainty.
The fractional dimension contributes partially to the score.
The beautiful side effect: uncertainty dampens extreme scores. If you're not sure about a college's culture, an extreme culture score (good or bad) counts less. Which is exactly right — you shouldn't be fully penalized or fully rewarded for something you can't fully measure.
As you gather more information, fractional dimensions become whole dimensions. Your score converges toward truth.
This is exactly how quantum mechanics works — a wavefunction is a superposition of possibilities that collapses toward a definite state as you observe it.

The Master Equation
All of these ideas combine into one equation:
GSCT=∑i=1ndi⋅wi⋅DiGSCT = \sum_{i=1}^{n} d_i \cdot w_i \cdot D_iGSCT=i=1∑n​di​⋅wi​⋅Di​
In plain English:

For each dimension you care about, multiply its score by how much you trust your measurement of it, and by how much that dimension matters to you. Add them all up.


n = how many dimensions you're evaluating on (can be fractional)
d = how confident you are in each dimension (0 to 1)
w = how much each dimension matters to you (all weights add to 1)
D = the actual SCT score for that dimension

If n = 0 — you're evaluating on zero dimensions — the answer is undefined. You haven't actually evaluated anything. That's not a score of zero. That's a refusal to look.
Preposterous: NoDimensions — cannot evaluate on zero planes.

Why Does This Matter?
Most ranking systems — for colleges, for programming languages, for anything — hide their assumptions. US News rankings have weights baked in that you never see. Someone decided research reputation matters more than financial aid, and you never get to disagree.
SCT makes everything explicit:

Here are the dimensions I'm evaluating on
Here are my confidence levels for each
Here are my weights
Here is the score

Change your weights, get a different ranking. That's not a bug. That's honesty.
The framework doesn't tell you what to value. It tells you what you get if you value what you say you value.

One Last Thing
SCT was invented to rate programming languages. Then it rated colleges. Then milk. Then it got imaginary numbers for pathological cases. Then fractional dimensions for uncertainty. Then a master equation that subsumes all of it.
None of that was planned. Each extension fell out of asking "what if we push this a little further?"
That's what math does when you let it.

Abu Shariff, age 12. BASIS Chandler. 2026.
Full technical paper: github.com/AbuCodingAI/SCT
Language family: github.com/AbuCodingAI/aclang
Query language: github.com/AbuCodingAI/JaSQL
