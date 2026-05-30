# Math for CS - MIT CS

6.042J | Fall 2010 | Undergraduate : https://ocw.mit.edu/courses/6-042j-mathematics-for-computer-science-fall-2010/&#x20;

newer : 6.1200J | Spring 2024 | Undergraduate : https://ocw.mit.edu/courses/6-1200j-mathematics-for-computer-science-spring-2 (This is clearer and has lecture notes already provided by them.)

gnrtd by Z AI.

## Lecture 1: Introduction and Proofs

**Course:** MIT 6.042J Mathematics for Computer Science\
**Topic:** Definitions of Proofs, Propositions, Axioms, and Logical Deductions.

***

### 1. What is a Proof?

#### 1.1 General Definitions of Truth

Before defining a mathematical proof, it is useful to understand how truth is ascertained in other fields. Truth is generally established through various methods depending on the context:

* **Observation/Experiment:** The bedrock of physics. (e.g., dropping chalk proves gravity).
* **Sampling/Counterexamples:** Used in statistics. Finding a counterexample disproves a rule, but sampling cannot always prove a universal rule.
* **Jury/Judge:** Legal truth is established by a verdict (e.g., OJ Simpson is not guilty of murder, but is liable in civil court).
* **Word of God (Religion):** Truth is based on religious texts or authority.
* **Word of Boss/Authority:** In business, "the customer is always right."
* **Inner Conviction:** Common in computer science ("There are no bugs in my program").

**Note on Math:** In mathematics, truth is not established by authority. A student can correct a professor if the logic is sound. This distinguishes math from many other disciplines.

#### 1.2 Mathematical Definition

In mathematics, a higher standard is required.

> **Definition:** A **mathematical proof** is a verification of a proposition by a chain of logical deductions from a set of axioms.

This definition contains three key components:

1. **Propositions**
2. **Logical Deductions**
3. **Axioms**

***

### 2. Propositions

> **Definition:** A **proposition** is a statement that is either true or false. It may not be known which one it is, but it must be one or the other.

#### 2.1 Predicates and Quantifiers

Often, propositions depend on variables. This leads to new terminology:

* **Predicate:** A proposition whose truth depends on the value of a variable (e.g., "$$n^2 + n + 41$$ is prime").
* **Universe of Discourse:** The set of values a variable can take (e.g., Natural numbers $$\mathbb{N} = \{0, 1, 2, \dots\}$$).
* **Quantifiers:**
  * **Universal Quantifier (**$$\forall$$**):** "For all" (upside-down A).
  * **Existential Quantifier (**$$\exists$$**):** "There exists" (backward E).

#### 2.2 Examples and Cautionary Tales

Testing propositions requires rigor. The lecture provided several examples where "obvious" truths turned out to be false.

**Example 1: The Prime Polynomial**

* **Proposition:** $$\forall n \in \mathbb{N}, n^2 + n + 41$$ is a prime number.
* **Testing:** For $$n=0$$ to $$39$$, the expression yields prime numbers ($$41, 43, 47, \dots, 1601$$).
* **Result:** The proposition is **False**.
* **Counterexample:** For $$n=40$$, $$40^2 + 40 + 41 = 1681 = 41^2$$.
* **Lesson:** Checking many cases (even 40) does not constitute a proof.

**Example 2: Euler's Sum of Powers Conjecture**

* **Proposition:** $$a^4 + b^4 + c^4 = d^4$$ has no positive integer solutions.
* **History:** Conjectured by Euler in 1769.
* **Result:** Disproved 218 years later by Noam Elkies.
* **Counterexample:** $$a=95800, b=217519, c=414560, d=422481$$.

**Example 3: Elliptic Curve**

* **Proposition:** $$313(x^3 + y^3) = z^3$$ has no positive integer solutions.
* **Result:** False. The smallest counterexample has over 1,000 digits.
* **Relevance:** These equations (elliptic curves) are central to modern cryptography (RSA) and factoring large integers.

**Example 4: The Four Color Theorem**

* **Proposition:** The regions in any map can be colored with four colors so that adjacent regions have different colors.
* **History:** Conjectured 1853. A flawed proof by Kempe (1879) stood for a decade before a fatal flaw was found.
* **Resolution:** Proved in 1977 by Appel and Haken using a computer to check thousands of cases.
* **Lesson:** Proofs by picture are often convincing but can be wrong.

**Example 5: Goldbach's Conjecture**

* **Proposition:** Every even integer greater than 2 is the sum of two primes.
* **Status:** Unknown. It remains one of the great unsolved problems in mathematics.
* **Warning:** A _Boston Globe_ article illustrating this conjecture listed $$20 = 9 + 11$$, incorrectly treating 9 as a prime number. Do not believe everything you read.

***

### 3. Logical Deductions

Logical deductions are the rules used to derive conclusions from premises.

#### 3.1 Implication ($$\implies$$)

> **Definition:** An implication $$P \implies Q$$ ("P implies Q") is true if $$P$$ is false or $$Q$$ is true.

This can be summarized in a truth table:

| P | Q | $$P \implies Q$$ |
| - | - | ---------------- |
| T | T | **T**            |
| T | F | **F**            |
| F | T | **T**            |
| F | F | **T**            |

**Key Concept:** **False implies anything.**\
If the premise ($$P$$) is false, the implication is technically true regardless of $$Q$$.

* _Example:_ "If pigs could fly, I would be king." This statement is true because "pigs fly" is false.

#### 3.2 If and Only If ($$\iff$$)

> **Definition:** $$P \iff Q$$ means ($$P \implies Q$$) AND ($$Q \implies P$$).

It is true only when $$P$$ and $$Q$$ have the same truth value (both True or both False).

| P | Q | $$P \iff Q$$ |
| - | - | ------------ |
| T | T | **T**        |
| T | F | **F**        |
| F | T | **F**        |
| F | F | **T**        |

**Example Proposition:** For all $$n \in \mathbb{Z}$$, $$n \ge 2 \iff n^2 \ge 4$$.

* This is **False**.
* Check $$n = -3$$: $$n^2 \ge 4$$ is True ($$9 \ge 4$$), but $$n \ge 2$$ is False.
* Since one direction fails, the "iff" statement is false.

#### 3.3 What is Not a Proposition?

Not every sentence is a proposition.

* Example: "This statement is false." (A paradox; cannot be T or F).
* Example: "Hello." (A greeting).

***

### 4. Axioms

> **Definition:** An **axiom** is a proposition that is assumed to be true without proof.

* **Origin:** From Greek, meaning "to think worthy."
* **Usage:** You must have axioms to start the process of deduction. The goal is to make assumptions explicit so anyone who accepts the axioms must accept the conclusion derived from them.

#### 4.1 Conflicting Axioms

Different fields of mathematics can use different, even contradictory, axioms.

* **Euclidean Geometry:** Given a line $$L$$ and point $$P$$ not on $$L$$, there is exactly one line through $$P$$ parallel to $$L$$.
* **Spherical Geometry:** There are no such parallel lines.
* **Hyperbolic Geometry:** There are infinitely many such parallel lines.

All three are valid fields of study; they simply start with different axioms.

#### 4.2 Consistency and Completeness

When choosing a set of axioms, mathematicians look for two properties:

1. **Consistent:** A set of axioms is consistent if no proposition can be proved to be both true and false.
2. **Complete:** A set of axioms is complete if it can be used to prove every proposition is either true or false.

#### 4.3 Gödel's Incompleteness Theorem

In the 1930s, Kurt Gödel proved a revolutionary result:

> **Theorem:** There exists no set of axioms that is both consistent and complete.

**Implications:**

* If you want consistency (which is essential), there will be true facts that you cannot prove.
* This destroyed the hopes of logicians like Russell and Whitehead who spent careers trying to formalize a complete mathematical system.
* _Example:_ Goldbach's Conjecture might be true, but impossible to prove.



***

gnrtd by Z AI.

## Recitation 1: Logic and Proofs

**Course:** MIT 6.042J / 18.062J Mathematics for Computer Science\
**Date:** September 10, 2010\
**Topic:** Logical Precision, Quantifiers, and Proof Structures.

***

### 1. Mathematical Logic

#### 1.1 The Need for Precision

English is often ambiguous, which is unsuitable for mathematics. For example, the statement "You could have cake or ice cream" does not specify if you can have both. To resolve this, mathematicians define precise meanings and symbols for logical connectives.

#### 1.2 Propositional Logic

A **proposition** is a statement that is either true or false. Propositions can be combined using logical connectives. The truth of the resulting statement depends on the truth of its components.

**Key Connectives and Symbols:**

| Connective                 | Symbols                 | Formal Name   |
| -------------------------- | ----------------------- | ------------- |
| Not $$P$$                  | $$\neg P$$, $$\bar{P}$$ | Negation      |
| $$P$$ and $$Q$$            | $$P \land Q$$           | Conjunction   |
| $$P$$ or $$Q$$             | $$P \lor Q$$            | Disjunction   |
| $$P$$ implies $$Q$$        | $$P \Rightarrow Q$$     | Implication   |
| $$P$$ if and only if $$Q$$ | $$P \Leftrightarrow Q$$ | Biconditional |

**Truth Table:**\
The validity of these connectives is defined by the following truth table:

| $$P$$ | $$Q$$ | $$\neg P$$ | $$P \land Q$$ | $$P \lor Q$$ | $$P \Rightarrow Q$$ | $$P \Leftrightarrow Q$$ |
| :---: | :---: | :--------: | :-----------: | :----------: | :-----------------: | :---------------------: |
|   F   |   F   |      T     |       F       |       F      |          T          |            T            |
|   F   |   T   |      T     |       F       |       T      |          T          |            F            |
|   T   |   F   |      F     |       F       |       T      |          F          |            F            |
|   T   |   T   |      F     |       T       |       T      |          T          |            T            |

**Important Definitions:**

* **Inclusive Or:** The statement "$$P$$ or $$Q$$" is true if $$P$$ is true, $$Q$$ is true, or **both** are true. (You can have your cake and ice cream too).
* **Implication (**$$P \Rightarrow Q$$**):** This is only false when $$P$$ is true and $$Q$$ is false. If $$P$$ is false, the implication is considered true ("vacuously true").
  * _Example:_ "If the moon is made of green cheese, then there will be no final in 6.042" is a true statement because the premise is false.

#### 1.3 Quantifiers

Quantifiers allow reasoning about elements in a set. They are always followed by a variable (often with a specified range) and a predicate.

* **Universal Quantifier (**$$\forall$$**):** "For all".
  * _Example:_ $$\forall x \in \mathbb{R}^+ \, e^x < (1+x)^{1+x}$$.
  * _Meaning:_ For every positive real number $$x$$, $$e^x$$ is less than $$(1+x)^{1+x}$$.
* **Existential Quantifier (**$$\exists$$**):** "There exists".
  * _Example:_ $$\exists n \in \mathbb{N} \, 2^n > (100n)^{100}$$.
  * _Meaning:_ There is at least one natural number $$n$$ such that $$2^n$$ is greater than $$(100n)^{100}$$.

**Note:** While useful as shorthand, overusing symbols can make mathematical statements difficult to read. Use them sparingly.

***

### 2. Proving an Implication

#### 2.1 Theorem: Swapping Quantifiers

**Theorem 1:** Let $$P(a, b)$$ be a predicate defined for all $$a \in A$$ and $$b \in B$$. Then:\
$$\exists a \in A \, \forall b \in B \, P(a, b) \implies \forall b \in B \, \exists a \in A \, P(a, b)$$

**Interpretation:** To understand this abstract statement, let's define concrete sets:

* $$A = \{ \text{6.042 students} \}$$
* $$B = \{ \text{6.042 lectures} \}$$
* $$P(a, b) = \text{``student } a \text{ falls asleep during lecture } b\text{''}$$
* **Left Side (LHS):** $$\exists a \in A \, \forall b \in B \, P(a, b)$$
  * _English:_ "There exists a student (let's call him Snoozer) who falls asleep in every lecture."
* **Right Side (RHS):** $$\forall b \in B \, \exists a \in A \, P(a, b)$$
  * _English:_ "In every lecture, there exists some student who falls asleep."

**Intuition:** The LHS implies the RHS. If "Snoozer" sleeps in every lecture, then naturally, in every lecture, _someone_ (Snoozer) is asleep. The converse (RHS implies LHS) is not necessarily true; a different student might sleep in each lecture, meaning no single student sleeps in _all_ of them.

#### 2.2 Definitions of Validity

* **Validity:** A statement that is universally true regardless of the specific predicate $$P$$ or sets $$A, B$$. Theorem 1 is a validity.
* **Tautology:** A validity that involves only propositional logic (no quantifiers).
* **Converse:** The converse of $$P \Rightarrow Q$$ is $$Q \Rightarrow P$$. The converse of Theorem 1 is not a validity because its truth depends on the specific situation.

#### 2.3 Proof Structure

To prove an implication $$P \Rightarrow Q$$:

**Method: Case Analysis**

1. **Case 1:** Assume $$P$$ is false. If $$P$$ is false, the implication $$P \Rightarrow Q$$ is true by default. (No work required).
2. **Case 2:** Assume $$P$$ is true. Show that $$Q$$ must also be true.

**Standard Practice:** Case 1 is trivial. Therefore, standard proofs only write out Case 2.

> **Rule of Thumb:** To prove $$P \Rightarrow Q$$, **assume** $$P$$ **is true** and prove that $$Q$$ is true subject to that assumption.

**Proof of Theorem 1:**\
Assume the LHS is true: $$\exists a_0 \in A$$ such that $$\forall b \in B, P(a_0, b)$$ is true.\
We must show the RHS: $$\forall b \in B, \exists a \in A, P(a, b)$$.

* Let $$b$$ be an arbitrary element in $$B$$.
* We know $$P(a_0, b)$$ is true (from our assumption).
* Therefore, there exists an $$a$$ (specifically $$a_0$$) such that $$P(a, b)$$ is true.
* Since $$b$$ was arbitrary, this holds for all $$b$$.
* Thus, the RHS is true.

***

### 3. Team Problem: A Mystery

#### 3.1 The Problem

A subset of the course staff (a "cabal") is plotting to make the final exam impossible. The roster is encrypted in logic. The staff set is:\
$$S = \{ \text{Oscar, Stav, Darren, Patrice, David, Nick, Martyna, Marten, Tom} \}$$The predicate $$\text{incabal}(x)$$ is true iff $$x$$ is a member.

#### 3.2 Translating the Statements

We translate the logical conditions into English:

**(i)** $$\exists x \exists y \exists z (x \neq y \land x \neq z \land y \neq z \land \text{incabal}(x) \land \text{incabal}(y) \land \text{incabal}(z))$$

* **Translation:** There are at least 3 different people in the cabal.

**(ii)** $$\neg(\text{incabal}(\text{Stav}) \land \text{incabal}(\text{David}))$$

* **Translation:** Stav and David are not both in the cabal. (At least one is out).

**(iii)** $$((\text{incabal}(\text{Martyna}) \lor \text{incabal}(\text{Patrice})) \Rightarrow \forall x \, \text{incabal}(x))$$

* **Translation:** If either Martyna or Patrice is in the cabal, then everyone is in the cabal.

**(iv)** $$\text{incabal}(\text{Stav}) \Rightarrow \text{incabal}(\text{David})$$

* **Translation:** If Stav is in, then David is in.

**(v)** $$\text{incabal}(\text{Darren}) \Rightarrow \text{incabal}(\text{Martyna})$$

* **Translation:** If Darren is in, then Martyna is in.

**(vi)** $$(\text{incabal}(\text{Oscar}) \lor \text{incabal}(\text{Nick})) \Rightarrow \neg \text{incabal}(\text{Tom})$$

* **Translation:** If Oscar or Nick is in, then Tom is not in.

**(vii)** $$(\text{incabal}(\text{Oscar}) \lor \text{incabal}(\text{David})) \Rightarrow \neg \text{incabal}(\text{Marten})$$

* **Translation:** If Oscar or David is in, then Marten is not in.

#### 3.3 Deducing the Solution

We determine who is in the cabal by eliminating impossibilities.

**Step 1: Eliminate Martyna and Patrice**\
From (ii), we know someone is not in the cabal (either Stav or David). Therefore, the cabal does not contain everyone.\
From (iii), if Martyna or Patrice were in, everyone would be in. Since not everyone is in, **Martyna and Patrice are NOT in the cabal.**

**Step 2: Eliminate Darren**\
From (v), if Darren were in, Martyna would be in. Since Martyna is out, **Darren is NOT in the cabal.** (Contrapositive).

**Step 3: Eliminate Stav**\
From (iv), if Stav were in, David would be in. This would mean both Stav and David are in, which contradicts (ii). Therefore, **Stav is NOT in the cabal.**

**Step 4: Eliminate Tom**\
Assume Tom is in the cabal.

* By (vi), if Tom is in, then neither Oscar nor Nick is in. So Oscar and Nick are out.
* Currently out: Martyna, Patrice, Darren, Stav, Oscar, Nick.
* Remaining candidates: Tom, Marten, David.
* By (i), we need at least 3 members. So the cabal must be exactly {Tom, Marten, David}.
* However, (vii) states if David is in, Marten is not. This is a contradiction.
* Therefore, the assumption was false. **Tom is NOT in the cabal.**

**Step 5: Eliminate Marten**\
Assume Marten is in the cabal.

* By (vii), if Marten is in, then neither Oscar nor David is in.
* We already know Martyna, Patrice, Darren, Stav, and Tom are out.
* The only possible members left would be Marten and Nick (size 2).
* This contradicts (i) which requires at least 3 members.
* Therefore, **Marten is NOT in the cabal.**

**Step 6: Conclusion**\
We have eliminated Martyna, Patrice, Darren, Stav, Tom, and Marten.\
The only remaining candidates are **Oscar, David, and Nick**.\
Since the cabal must have at least 3 members (condition i), all three must be members.

#### 3.4 Verification

We must verify that the set $$C = \{ \text{Oscar, David, Nick} \}$$ satisfies all conditions:

* (i) Size is 3. (Satisfied)
* (ii) Stav is not in the set. (Satisfied)
* (iii) Neither Martyna nor Patrice is in the set (Hypothesis false). (Satisfied)
* (iv) Stav is not in the set (Hypothesis false). (Satisfied)
* (v) Darren is not in the set (Hypothesis false). (Satisfied)
* (vi) Oscar is in the set (Hypothesis true). Tom is not in the set (Conclusion true). (Satisfied)
* (vii) Oscar and David are in the set (Hypothesis true). Marten is not in the set (Conclusion true). (Satisfied)

**Final Answer:** The cabal consists of **Oscar, David, and Nick**.

***

gnrtd by Z AI.

## Lecture 2: Induction

**Course:** MIT 6.042J Mathematics for Computer Science\
**Topic:** Proof Techniques (Contradiction, Good vs. Bad Proofs) and Mathematical Induction.

***

### 1. Review of Proof Techniques

In the previous lecture, the components of a proof were defined: propositions, axioms, and logical deductions. This lecture begins by expanding on how these components are assembled into standard proof strategies.

#### 1.1 Direct Proof

A **direct proof** starts with axioms and known theorems, then applies logical deductions step-by-step until the desired conclusion is reached. This is the most straightforward method.

#### 1.2 Proof by Contradiction (Indirect Proof)

A **proof by contradiction** is used when a direct proof is difficult to construct.

> **Method:** To prove a proposition $$P$$ is true:

1. Assume $$P$$ is false (i.e., assume $$\neg P$$ is true).
2. Use logical deductions to derive a falsehood (a contradiction).
3. Conclude that the assumption $$\neg P$$ must be false, meaning $$P$$ is true.

**Logic:** This works because the statement $$(\neg P \implies \text{False})$$ is logically equivalent to $$P$$ being true.

**Example:** $$\sqrt{2}$$ **is Irrational**

**Proposition:** $$\sqrt{2}$$ is irrational.\
**Proof (by contradiction):**

1. Assume $$\sqrt{2}$$ is rational.
2. By definition, $$\sqrt{2} = a/b$$ where $$a$$ and $$b$$ are integers with no common divisors (the fraction is in lowest terms).
3. Square both sides: $$2 = a^2/b^2 \implies 2b^2 = a^2$$.
4. Since $$a^2$$ is even ($$2b^2$$ is even), $$a$$ must be even. Let $$a = 2k$$.
5. Substitute back: $$2b^2 = (2k)^2 = 4k^2 \implies b^2 = 2k^2$$.
6. This implies $$b^2$$ is even, so $$b$$ must be even.
7. **Contradiction:** We have established that both $$a$$ and $$b$$ are even, meaning they share a common divisor of 2. This contradicts the assumption that $$a/b$$ is in lowest terms.
8. Therefore, $$\sqrt{2}$$ is irrational. $$\square$$

**Historical Context:** The Pythagoreans (ancient Greek religious society) believed irrational numbers did not exist because they represented "infinity" (the domain of the "bad god"). The discovery of this proof was a crisis for their belief system, revealing their axioms were inconsistent.

***

### 2. The Dangers of "Proof by Picture"

Visual proofs can be very convincing but are prone to errors, specifically regarding scaling.

**Example:** $$90 > 92$$

A visual "proof" was presented where a $$9 \times 10$$ rectangle (Area 90) was cut and reassembled into two rectangles of dimensions approx $$2 \times 2$$ and $$11 \times 8$$ (Area $$> 92$$).

**The Flaw:** The diagram was not drawn to scale. A segment labeled "2" was drawn longer than a segment labeled "2+" simply due to distortion.\
**Lesson:** Never trust a diagram completely. A rigorous proof must rely on algebraic or logical steps, not visual estimation.

***

### 3. Mathematical Induction

Induction is the most powerful and common proof technique in computer science.

#### 3.1 The Principle of Induction

Let $$P(n)$$ be a predicate. If:

1. **Base Case:** $$P(0)$$ is true.
2. **Inductive Step:** For all natural numbers $$n \geq 0$$, $$P(n) \implies P(n+1)$$ is true.

Then:

* $$P(n)$$ is true for all natural numbers $$n$$.

**Analogy:** Think of an infinite line of dominoes.

* $$P(0)$$ is true: You knock over the first domino.
* $$P(n) \implies P(n+1)$$: If the $$n$$-th domino falls, it knocks over the $$(n+1)$$-th domino.
* Result: All dominoes fall.

#### 3.2 The Structure of an Induction Proof

1. **State the method:** "Proof by induction."
2. **Define the Predicate:** Identify $$P(n)$$ (the inductive hypothesis).
3. **Prove the Base Case:** Verify $$P(0)$$ (or the starting value $$b$$) is true.
4. **Prove the Inductive Step:** Show that $$P(n) \implies P(n+1)$$.
   * _Technique:_ To prove an implication ($$A \implies B$$), assume $$A$$ is true and derive $$B$$. Therefore, assume $$P(n)$$ is true and show $$P(n+1)$$ must be true.

#### 3.3 Example 1: Sum of Integers

**Theorem:** $$\sum_{i=1}^n i = 1 + 2 + \dots + n = \frac{n(n+1)}{2}$$.

**Proof:**

1. **Predicate:** Let $$P(n)$$ be the proposition that $$\sum_{i=1}^n i = \frac{n(n+1)}{2}$$.
2. **Base Case (**$$n=0$$**):**
   * LHS: $$\sum_{i=1}^0 i = 0$$ (empty sum).
   * RHS: $$\frac{0(0+1)}{2} = 0$$.
   * $$P(0)$$ is true.
3. **Inductive Step:**
   * Assume $$P(n)$$ is true for $$n \geq 0$$. (Inductive Hypothesis).
   * Show $$P(n+1)$$ is true: $$\sum_{i=1}^{n+1} i = \frac{(n+1)(n+2)}{2}$$.
   * _Derivation:_\
     $$\sum_{i=1}^{n+1} i = \left( \sum_{i=1}^n i \right) + (n+1)$$Using the inductive hypothesis:\
     $$= \frac{n(n+1)}{2} + (n+1)$$$$= \frac{n^2 + n + 2n + 2}{2} = \frac{n^2 + 3n + 2}{2}$$$$= \frac{(n+1)(n+2)}{2}$$
   * The expression matches the goal. Thus, $$P(n) \implies P(n+1)$$.
4. **Conclusion:** By induction, the theorem holds for all $$n$$.

**Critique of Induction:** While induction proves the theorem is correct, it does not help _derive_ the formula, nor does it provide intuition on _why_ it is true. It is a verification tool.

#### 3.4 Example 2: Divisibility

**Theorem:** For all $$n \geq 0$$, $$3 \mid (n^3 - n)$$.

**Proof:**

1. **Predicate:** $$P(n)$$ is the statement $$3 \mid (n^3 - n)$$.
2. **Base Case (**$$n=0$$**):** $$0^3 - 0 = 0$$. 3 divides 0. True.
3. **Inductive Step:**
   * Assume $$P(n)$$ is true ($$3$$ divides $$n^3 - n$$).
   * Consider $$(n+1)^3 - (n+1)$$.
   * Expand: $$n^3 + 3n^2 + 3n + 1 - n - 1 = n^3 + 3n^2 + 2n$$.
   * Rearrange to find $$n^3 - n$$: $$= (n^3 - n) + 3n^2 + 3n$$.
   * Since $$3$$ divides $$(n^3 - n)$$ (by hypothesis) and 3 divides $$3n^2 + 3n$$, the sum is divisible by 3.
   * Thus, $$P(n+1)$$ is true.

***

### 4. Pitfalls in Induction: The Horse Puzzle

Induction is powerful, but small errors in the logic can lead to absurd conclusions.

**False Theorem:** All horses are the same color.\
**Proof (Flawed):**

1. **Predicate:** $$P(n)$$: In any set of $$n$$ horses, all horses are the same color.
2. **Base Case (**$$n=1$$**):** In a set of 1 horse, all horses are the same color. True.
3. **Inductive Step:**
   * Assume $$P(n)$$ is true.
   * Consider a set of $$n+1$$ horses: $$\{h_1, h_2, \dots, h_{n+1}\}$$.
   * Subset $$\{h_1, \dots, h_n\}$$ has $$n$$ horses, so they are all the same color.
   * Subset $$\{h_2, \dots, h_{n+1}\}$$ has $$n$$ horses, so they are all the same color.
   * Since the two subsets overlap (at $$h_2, \dots, h_n$$), all $$n+1$$ horses must be the same color.
   * Therefore, $$P(n) \implies P(n+1)$$.

**The Flaw:**\
The proof implies that the "bridge" between the first horse and the last horse is formed by the overlapping horses ($$h_2 \dots h_n$$).\
Consider the case where $$n=1$$:

* Set is $$\{h_1, h_2\}$$.
* First subset is $$\{h_1\}$$. Second subset is $$\{h_2\}$$.
* There are **no** overlapping horses. The set $$\{h_2, \dots, h_n\}$$ is an **empty set** because $$n=1$$.
* Because the set is empty, there is no logical bridge to conclude $$h_1$$ and $$h_2$$ are the same color.
* The implication $$P(1) \implies P(2)$$ fails.

**Lesson:** Always check that the inductive step holds for **all** values starting from the base case, and be extremely careful with "dot dot dot" notation, especially when the set size implies an empty set.

***

### 5. Constructive Induction: The Courtyard Problem

Induction can be used to prove the existence of a construction or solution, not just verify a formula.

**Problem:** Tile a $$2^n \times 2^n$$ courtyard with L-shaped tiles (covering 3 squares each), leaving one center square empty for a statue ("Bill").

**Attempt 1:**

* **Predicate** $$P(n)$$**:** A $$2^n \times 2^n$$ grid can be tiled with the center square missing.
* **Inductive Step:** Divide a $$2^{n+1} \times 2^{n+1}$$ grid into four $$2^n \times 2^n$$ quadrants.
  * Bill is in one quadrant (say, top-left). We can tile that quadrant by hypothesis.
  * We are left with three $$2^n \times 2^n$$ quadrants that need a missing square in their centers to fit the hypothesis. The hypothesis does not support this directly.

**Solution Strategy: Strengthening the Hypothesis**\
When the original hypothesis is too weak to support the inductive step, choose a **stronger** hypothesis. This seems counterintuitive (proving "more" is harder), but a stronger hypothesis provides more assumptions to work with in the inductive step.

**Revised Predicate:** $$P(n)$$: A $$2^n \times 2^n$$ grid can be tiled with **one missing square anywhere** in the grid.

**Proof:**

1. **Base Case:** $$n=0$$. A $$1 \times 1$$ grid with one missing square is just an empty grid. Trivially true.
2. **Inductive Step:**
   * Assume any $$2^n \times 2^n$$ grid with one missing square can be tiled.
   * Consider a $$2^{n+1} \times 2^{n+1}$$ grid. Divide into four $$2^n \times 2^n$$ quadrants.
   * Bill is in one quadrant (say, top-left).
   * Place one L-shaped tile in the center, covering one corner square of each of the other three quadrants.
   * Now, all four quadrants have exactly one square missing (one has Bill, three have the L-tile corners).
   * By the inductive hypothesis, we can tile all four quadrants.
   * Thus, the whole grid is tiled. $$\square$$

***

gnrtd by Z AI.

## Lecture 3: Strong Induction and Invariants

**Course:** MIT 6.042J Mathematics for Computer Science\
**Topic:** Characteristics of Good Proofs, Invariants, and Strong Induction.

***

### 1. Characteristics of a Good Proof

A proof is not just a series of steps; it is an argument designed to convince. A good proof possesses seven key characteristics:

1. **Correct:** It must be logically valid.
2. **Complete:** All key steps and details must be present.
3. **Clear:** The reasoning must be understandable.
4. **Brief:** It should be crisp and to the point, avoiding unnecessary details.
5. **Elegant:** Ideally, the proof should be clever and "beautiful" in the mathematical sense.
6. **Organized:** Use lemmas like subroutines to structure the argument.
7. **In Order:** The steps should proceed logically from assumptions to conclusions (top-down).

**Warning on Order:** A common mistake is "backwards proof."

* _Example:_ Trying to prove $$A = B$$ by starting with $$A = B$$, doing steps, and ending with $$1 = 1$$.
* This is only valid if every step is reversible (iff). However, standard proofs should flow top-down: start with known truths and derive the result.

#### 1.1 Why Good Proofs Matter

Software errors can have catastrophic real-world consequences. Because proofs are the standard for verifying program correctness, rigorous proof skills are essential.

* **Airbus A300:** First commercial jet fully operated by software. A software bug caused a crash by opening a rear door during landing.
* **Therac-25:** A radiation therapy machine where a race condition caused massive radiation overdoses, killing patients.
* **2000 Election:** A bug in electronic voting booths resulted in a candidate receiving negative 16,000 votes.
* **Akamai:** A company delivering significant web content; a bug could take down a large portion of the internet.

#### 1.2 How Proofs Go Wrong

Even famous mathematicians make mistakes, often by omitting "obvious" details.

* **Gauss (1799):** In his PhD thesis on the Fundamental Theorem of Algebra, he stated a "fact" about algebraic curves entering bounded regions that took over 100 years to prove correctly.
* **Poincaré (1900):** Claimed a fact that he later demoted to a "conjecture," which took a century to solve.

#### 1.3 Top 10 Bad Proof Techniques

Avoid these "strategies" at all costs:

1. **Proof by Reference to Eminent Authority:** "I saw Fermat on the elevator and he said..."
2. **Proof by Appeal to Intuition:** "Any moron knows that..."
3. **Proof by Vehement Assertion:** Repeating it loudly doesn't make it true.
4. **Proof by Picture:** Visual proofs can be deceiving.
5. **Proof by Omission:** "The reader may easily supply the details."
6. **Proof by Exhaustion:** Wearing the reader down until they give up.
7. **Proof by Cumbersome Notation:** Confusing the reader with symbols.
8. **Proof by Vigorous Hand-waving:** Distracting the audience.
9. **Proof by Example:** "The case $$n=2$$ works, so it's generally true."
10. **Proof by Throwing in the Kitchen Sink:** Writing down every known theorem hoping one helps.

**Example: Fermat's Last Theorem**\
Fermat claimed a proof for $$x^n + y^n = z^n$$ having no integer solutions for $$n > 2$$ but didn't write it down. It took Andrew Wiles over 10 years and hundreds of pages to prove it in the 1990s. Fermat was right about one thing: the proof didn't fit in the margin.

***

### 2. Invariants: The 15-Puzzle

#### 2.1 The Problem

Consider an $$8$$-puzzle (a $$3 \times 3$$ grid with tiles labeled 'a' through 'h' and one blank square).

* **Start State:** Tiles a-f are in order, but g and h are swapped.
* **Goal State:** Tiles a-h are in alphabetical order.
* **Move:** Slide a tile into the adjacent blank square (row or column moves).
* **Question:** Can you solve the puzzle?

#### 2.2 The Method of Invariants

To prove a system can _never_ reach a certain state, we use an **invariant**.

> **Definition:** An invariant is a property that:
>
> 1. Holds for the initial state.
> 2. Is preserved by every legal move.
> 3. Does **not** hold for the target state.

If the target state lacks the invariant, it cannot be reached from the start state.

#### 2.3 Analyzing the Puzzle

We define the **natural order** of the grid cells as $$1, 2, \dots, 9$$. We check how moves affect the relative order of tiles.

**Definitions:**

* **Inversion:** A pair of letters $$(L_1, L_2)$$ is an inversion if $$L_1$$ precedes $$L_2$$ in the alphabet, but $$L_1$$ appears _after_ $$L_2$$ in the puzzle grid.

**Lemmas:**

1. **Row Move:** A row move shifts a tile between adjacent cells ($$i$$ to $$i \pm 1$$). This preserves the relative order of all pairs. **The number of inversions does not change.**
2. **Column Move:** A column move shifts a tile 3 positions ($$i$$ to $$i \pm 3$$). This changes the relative order of the tile with exactly two others.
   * If the tile jumps over 2 others, it swaps order with both.
   * Result: Inversions change by $$+2$$, $$-2$$, or $$0$$.
3. **Parity Lemma:** The parity (evenness or oddness) of the number of inversions does not change. (Adding/subtracting 2 does not change parity).

**Corollary:** The parity of the number of inversions is an **invariant**.

#### 2.4 Proof of Impossibility

* **Start State:** (g and h swapped).
  * Only one pair is out of order: (g, h).
  * Number of inversions = 1.
  * **Parity is Odd.**
* **Target State:** (a through h in order).
  * No pairs out of order.
  * Number of inversions = 0.
  * **Parity is Even.**

**Conclusion:** Since the start state has an odd parity invariant and the target state has an even parity, it is **impossible** to solve the puzzle using legal moves.

***

### 3. Strong Induction

Strong induction is a variant of induction that allows for a "stronger" inductive hypothesis.

#### 3.1 The Axiom

Let $$P(n)$$ be a predicate. If:

1. **Base Case:** $$P(0)$$ is true.
2. **Inductive Step:** For all $$n \ge 0$$, $$(P(0) \land P(1) \land \dots \land P(n)) \implies P(n+1)$$.

Then $$P(n)$$ is true for all $$n$$.

**Difference from Ordinary Induction:** In ordinary induction, you assume only $$P(n)$$ to prove $$P(n+1)$$. In strong induction, you assume $$P(0), P(1), \dots, P(n)$$.

_Note:_ Anything provable by strong induction is provable by ordinary induction, but strong induction often makes the proof easier by allowing more assumptions.

#### 3.2 Example: The Unstacking Game

**Rules:** You have a stack of $$n$$ blocks.

1. Split the stack into two smaller stacks of sizes $$k$$ and $$n-k$$.
2. Earn points equal to the product of the sizes of the two new stacks: $$k \times (n-k)$$.
3. Repeat until all stacks have size 1.
4. Total Score is the sum of points from all moves.

**Question:** What strategy maximizes the score?\
**Observation:** In class, different strategies for $$n=8$$ resulted in the same score (28).

#### 3.3 Theorem

**Theorem:** All strategies for the $$n$$-block Unstacking Game yield the same score.\
**Score Formula:** The score is always $$\frac{n(n-1)}{2}$$.

#### 3.4 Proof by Strong Induction

We prove the score is $$\frac{n(n-1)}{2}$$.

**Predicate:** $$P(n)$$: "Every strategy for a stack of $$n$$ blocks scores $$\frac{n(n-1)}{2}$$ points."

**Base Case (**$$n=1$$**):**

* No moves possible. Score = 0.
* Formula: $$\frac{1(0)}{2} = 0$$.
* $$P(1)$$ is true.

**Inductive Step:**

* **Inductive Hypothesis:** Assume $$P(1), P(2), \dots, P(n)$$ are all true.
* Consider a stack of $$n+1$$ blocks.
* The first move splits the stack into two sub-stacks of sizes $$k$$ and $$(n+1)-k$$, where $$1 \le k \le n$$.
* Points earned in first move: $$k(n+1-k)$$.
* **Recursive Scores:**
  * For the stack of size $$k$$, the score is fixed by hypothesis $$P(k)$$: $$\frac{k(k-1)}{2}$$.
  * For the stack of size $$(n+1)-k$$, the score is fixed by hypothesis $$P(n+1-k)$$: $$\frac{(n+1-k)(n-k)}{2}$$.
* **Total Score Calculation:**\
  $$\text{Score} = k(n+1-k) + \frac{k(k-1)}{2} + \frac{(n+1-k)(n-k)}{2}$$
* **Simplification:**
  * Expand the terms:\
    $$= nk + k - k^2 + \frac{k^2 - k}{2} + \frac{n^2 + n - 2nk - k + k^2}{2}$$
  * Multiply everything by 2 to clear the fraction:\
    $$= 2nk + 2k - 2k^2 + k^2 - k + n^2 + n - 2nk - k + k^2$$
  * Cancel terms:
    * $$-2k^2 + k^2 + k^2 = 0$$
    * $$2k - k - k = 0$$
    * $$2nk - 2nk = 0$$
  * Remaining: $$n^2 + n$$.
  * Divide by 2 (since we multiplied by 2 earlier in the logic of combining fractions):\
    $$\text{Score} = \frac{n^2 + n}{2} = \frac{n(n+1)}{2}$$
* This matches the formula for $$P(n+1)$$.

**Conclusion:** By strong induction, the score for $$n$$ blocks is always $$\frac{n(n-1)}{2}$$. This proves that strategy does not matter; the score is invariant.

