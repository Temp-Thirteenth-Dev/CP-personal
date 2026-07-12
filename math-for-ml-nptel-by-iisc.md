# Math for ML NPTEL by IISc



## Probability

Here are the detailed lecture notes based on the provided transcript.

## Lecture 38: Expected Value of Random Variable

**Course:** Mathematical Foundations for Machine Learning\
**Topic:** Expected Value, Properties, Common Distributions, and Mean Squared Error

***

### 1. Introduction and Motivation

In previous lectures, we established that the Probability Mass Function (PMF) or Probability Density Function (PDF) provides a complete description of a random variable. However, in many real-world scenarios, we may not have access to the full distribution.

* **The Problem:** Can we characterize a random variable with a single representative number if we don't know the full distribution?
* **The Analogy:** A student's Cumulative Grade Point Average (CGPA). While it doesn't show individual semester performance, it summarizes overall academic performance in a single value.
* **The Solution:** We use the **Expected Value** (or Mean) of a random variable to describe its central tendency or "long-run average."

***

### 2. Definition of Expected Value

The expected value of a random variable $$X$$, denoted as $$E[X]$$, is calculated differently for discrete and continuous variables.

#### A. Discrete Random Variable

For a discrete random variable $$X$$ taking values $$x_i$$ with probability mass function $$P_X(x_i)$$:\
$$E[X] = \sum_{i} x_i P_X(x_i)$$

* **Interpretation:** This is a weighted sum of all possible values, where the weights are the probabilities of occurrence.
* **Simple Average Connection:** If all outcomes are equally likely (e.g., a fair die), $$P_X(x_i) = 1/n$$. The formula reduces to the arithmetic mean: $$\frac{\sum x_i}{n}$$.

#### B. Continuous Random Variable

For a continuous random variable $$X$$ with probability density function $$f_X(x)$$:\
$$E[X] = \int_{-\infty}^{\infty} x f_X(x) \, dx$$

***

### 3. Interpretations of Expected Value

1. **Average Value:** It is the average value of the outcomes over a large number of experimental trials.
2. **Center of Mass:** Physically, $$E[X]$$ corresponds to the center of mass of the probability distribution.
3. **DC Value:** In signal processing, $$E[X]$$ corresponds to the DC (Direct Current) component present in a signal.
4. **Best Predictor:** It is the best prediction of the outcome of an experiment (minimizes mean squared error, discussed later).

**Important Note:** $$E[X]$$ does not have to be a value that $$X$$ can actually take.

* _Example:_ Rolling a fair die results in values $$\{1, 2, 3, 4, 5, 6\}$$. The expected value is $$3.5$$, which is not in the set.

***

### 4. Expected Values of Common Distributions

The lecture derives or states the expected values for standard distributions covered in previous classes.

#### Discrete Distributions

1. **Bernoulli Distribution (**$$X \sim \text{Bernoulli}(p)$$**):**
   * $$E[X] = 0 \cdot (1-p) + 1 \cdot p = p$$.
2. **Binomial Distribution (**$$X \sim \text{Binomial}(n, p)$$**):**
   * Represents $$n$$ independent Bernoulli trials.
   * $$E[X] = n \cdot p$$.
3. **Geometric Distribution (**$$X \sim \text{Geometric}(p)$$**):**
   * Models the number of trials needed to get the first success.
   * Intuition: If probability of success $$p = 1/10$$, you expect to wait 10 trials.
   * $$E[X] = \frac{1}{p}$$.
4. **Poisson Distribution (**$$X \sim \text{Poisson}(\lambda)$$**):**
   * The parameter $$\lambda$$ represents the average arrival rate.
   * $$E[X] = \lambda$$.

#### Continuous Distributions

1. **Uniform Distribution (**$$X \sim \text{Uniform}(a, b)$$**):**
   * PDF is constant: $$f_X(x) = \frac{1}{b-a}$$.
   * Calculation: $$\int_{a}^{b} x \frac{1}{b-a} \, dx = \frac{b^2 - a^2}{2(b-a)} = \frac{a+b}{2}$$.
   * $$E[X] = \frac{a+b}{2}$$ (the midpoint of the interval).
2. **Exponential Distribution (**$$X \sim \text{Exponential}(\lambda)$$**):**
   * $$E[X] = \frac{1}{\lambda}$$.
3. **Gaussian (Normal) Distribution (**$$X \sim N(\mu, \sigma^2)$$**):**
   * The parameter $$\mu$$ is the mean.
   * $$E[X] = \mu$$.

***

### 5. Properties of Expected Value

#### A. Linearity

For any constants $$a$$ and $$b$$:\
$$E[aX + b] = aE[X] + b$$

#### B. Vector Interpretation

In Linear Algebra terms, if vector $$\mathbf{x}$$ represents values and vector $$\mathbf{p}$$ represents probabilities, the expected value is the dot product:\
$$E[X] = \mathbf{x}^T \mathbf{p}$$This can be viewed as the projection of the data onto a line where all components are equidistant (the vector of ones).

***

### 6. Expected Value as the Best Predictor

We prove that $$E[X]$$ is the best constant prediction for $$X$$ by minimizing the **Mean Squared Error (MSE)**.

* **Goal:** Find a constant $$B$$ (prediction) that minimizes the squared error $$(X - B)^2$$.
* **Objective Function:** Minimize $$MSE(B) = E[(X - B)^2]$$.

**Derivation:**

1. Expand the squared term:\
   $$MSE(B) = E[X^2 - 2BX + B^2]$$
2. Use linearity of expectation:\
   $$MSE(B) = E[X^2] - 2B E[X] + B^2$$
3. Differentiate with respect to $$B$$ and set to zero to find the minimum:\
   $$\frac{d}{dB} (MSE(B)) = -2E[X] + 2B = 0$$
4. Solve for $$B$$:\
   $$B = E[X]$$

**Conclusion:** The expected value is the constant that minimizes the mean squared error of the prediction.

***

### 7. Limitations of Expected Value

While $$E[X]$$ provides a central value, it does not describe the **spread** or dispersion of the data.

* **The "Startup Salary" Example:**
  * A startup founder tells an intern the average salary is 10 Lakhs/year.
  * **Scenario 1:** Founder takes 20L, Intern gets 0L. Average = 10L.
  * **Scenario 2:** Founder takes 10L, Intern gets 10L. Average = 10L.
  * **Scenario 3:** Founder takes 18L, Intern gets 2L. Average = 10L.
* **Conclusion:** The average alone is insufficient to understand the situation. We need a measure of **spread** (how much the salaries deviate from the mean).

***

### 8. Preview: Variance

To address the limitation of expected value, we introduce the concept of **Variance**.

* **Definition:** Variance measures the spread or dispersion of a random variable around its mean.
* **Importance:** Knowing both the expected value (mean) and the variance gives a much clearer picture of the distribution (e.g., in Gaussian distribution, knowing $$\mu$$ and $$\sigma^2$$ defines the entire curve).

The next lecture will cover Variance and Moments in detail.



Here are the detailed lecture notes based on the provided transcript.

## Lecture 39: Moments and Variance

**Course:** Mathematical Foundations for Machine Learning\
**Topic:** Moments, Central Moments, Variance, and Properties of Variance

***

### 1. Introduction and Motivation

In previous lectures, we established that the Probability Mass Function (PMF) and Probability Density Function (PDF) provide complete descriptions of random variables. We also introduced the **Expected Value (**$$E[X]$$ **or** $$\mu$$**)** as a single representative number (like a CGPA) that characterizes the center of the distribution.

* **The Limitation of Mean:** The expected value does not capture the **spread** or **variability** of the data.
  * _Example:_ A student could perform exceptionally well in some semesters and poorly in others, yet end up with the same CGPA as a student who performed consistently average. The mean hides this variation.
* **The Goal:** We need a measure to quantify the spread of a random variable to better understand its behavior. This leads us to the concepts of **Moments** and **Variance**.

***

### 2. Moments of a Random Variable

Moments are a set of parameters that help describe the shape and characteristics of a distribution.

#### A. Nth Moment about a Non-Random Value ($$C$$)

The general definition of the $$n$$-th moment about a value $$C$$ is:

* **Discrete:** $$E[(X - C)^n] = \sum (x_i - C)^n P_X(x_i)$$
* **Continuous:** $$E[(X - C)^n] = \int_{-\infty}^{\infty} (x - C)^n f_X(x) \, dx$$

#### B. Moments about the Origin (Raw Moments)

If we set $$C = 0$$, we get the moments about the origin.

* **Definition:** The $$n$$-th moment about the origin is $$E[X^n]$$.
* **First Moment (**$$n=1$$**):** $$E[X] = \mu$$. This is the **Mean**.
* **Linearity:** Recall that the expectation operator is linear:\
  $$E[aX + bY] = aE[X] + bE[Y]$$

***

### 3. Central Moments

To compare the variability of two different distributions (e.g., heights of college students vs. middle school students), we face a problem: their means are different. To make a fair comparison of spread, we need to center the distributions at the same point.

* **Solution:** We shift the random variable by subtracting its mean.
* **Definition:** The $$n$$-th **Central Moment** is the moment about the mean ($$\mu$$).\
  $$E[(X - \mu)^n]$$

#### Properties of Central Moments

1. **First Central Moment (**$$n=1$$**):**\
   $$E[X - \mu] = E[X] - \mu = 0$$The first central moment is always zero.
2. **Second Central Moment (**$$n=2$$**):**\
   $$E[(X - \mu)^2]$$This is defined as the **Variance**.

***

### 4. Variance and Standard Deviation

#### Variance ($$\sigma_X^2$$)

Variance measures the spread or dispersion of the random variable around its mean.

* **Definition:** $$\text{Var}(X) = \sigma_X^2 = E[(X - \mu)^2]$$
* **Interpretation:** It quantifies how much the values deviate from the average. A high variance indicates high spread; a variance of zero indicates all values are identical.

#### Standard Deviation ($$\sigma_X$$)

* **Definition:** $$\sigma_X = \sqrt{\text{Var}(X)}$$
* It is the positive square root of the variance. It is useful because it has the same units as the random variable $$X$$.

#### Computational Formula for Variance

Instead of calculating $$E[(X - \mu)^2]$$ directly, we can expand the term:\
$$\text{Var}(X) = E[X^2 - 2\mu X + \mu^2]$$Using linearity of expectation:\
$$\text{Var}(X) = E[X^2] - 2\mu E[X] + E[\mu^2]$$Since $$E[X] = \mu$$ and $$\mu$$ is a constant ($$E[\mu^2] = \mu^2$$):\
$$\text{Var}(X) = E[X^2] - \mu^2$$Or generally:\
$$\sigma_X^2 = E[X^2] - (E[X])^2$$

***

### 5. Properties of Variance

#### Property 1: Variance is Non-Negative

$$\text{Var}(X) \ge 0$$Since it is the expectation of a squared term $$(X-\mu)^2$$, it cannot be negative.

#### Property 2: Variance of a Constant

If $$X$$ is a constant $$B$$, there is no spread.\
$$\text{Var}(B) = 0$$

#### Property 3: Effect of Linear Transformation

Let $$Y = aX + b$$. What is $$\text{Var}(Y)$$?\
$$\text{Var}(aX + b) = E[(aX + b)^2] - (E[aX + b])^2$$Expanding and simplifying using linearity yields:\
$$\text{Var}(aX + b) = a^2 \text{Var}(X)$$

* **Key Insight:** Adding a constant ($$b$$) shifts the data but **does not change the spread** (variance). Scaling by $$a$$ scales the variance by $$a^2$$.

#### Property 4: Variance Operator is NOT Linear

In general, variance is not additive.\
$$\text{Var}(g_1(X) + g_2(X)) \neq \text{Var}(g_1(X)) + \text{Var}(g_2(X))$$

* **Example:** Let $$g_1(X) = X$$ and $$g_2(X) = X$$. Then we are looking at $$\text{Var}(X + X) = \text{Var}(2X)$$.
  * Using Property 3: $$\text{Var}(2X) = 2^2 \text{Var}(X) = 4\text{Var}(X)$$.
  * If variance were linear, it would be $$\text{Var}(X) + \text{Var}(X) = 2\text{Var}(X)$$.
  * Since $$4\text{Var}(X) \neq 2\text{Var}(X)$$, variance is **not** linear.

***

### 6. Variance of Common Distributions

#### A. Bernoulli Distribution

* $$X \sim \text{Bernoulli}(p)$$
* $$E[X] = p$$
* $$E[X^2] = \sum x^2 P_X(x) = 0^2(1-p) + 1^2(p) = p$$
* **Variance:**\
  $$\text{Var}(X) = E[X^2] - (E[X])^2 = p - p^2 = p(1-p)$$

#### B. Binomial Distribution

* $$X \sim \text{Binomial}(n, p)$$
* A Binomial variable is the sum of $$n$$ **independent** Bernoulli trials.
* Since the trials are independent, the variances add up (though the operator isn't linear generally, for independent variables $$X_1, X_2$$, $$\text{Var}(X_1+X_2) = \text{Var}(X_1) + \text{Var}(X_2)$$).
* **Variance:**\
  $$\text{Var}(X) = n \cdot p(1-p)$$

***

### 7. Interpretation in Signal Processing

For those familiar with signals:

* **Mean (**$$\mu$$**):** Corresponds to the **DC value** (Direct Current) or the constant offset in a signal.
* **Variance (**$$\sigma^2$$**):** Corresponds to the **AC power** (Alternating Current) or the power of the fluctuating component.
* This concept is fundamental in communication systems (e.g., calculating Signal-to-Noise Ratio).

***

### 8. Conclusion and Summary

* **Moments:** Provide a way to characterize a random variable. The first moment is the mean; the second central moment is the variance.
* **Characterization:** While PDF/PMF are exact descriptions, Mean and Variance provide a summary that is often sufficient for analysis.
* **Next Steps:** We have analyzed single random variables. Real-world scenarios often involve multiple interacting random variables (e.g., petrol prices affecting commodity prices). The next lectures will cover **Jointly Distributed Random Variables**.



## Lecture 40: Joint Distributions and Marginals

**Course:** Mathematical Foundations for Machine Learning\
**Topic:** Jointly Distributed Random Variables, PMF, PDF, CDF, and Marginals

***

### 1. Introduction and Motivation

**Context:**\
In previous weeks, we covered Linear Algebra and the basics of Probability (up to Bayes' Theorem). We also characterized single random variables using PMF, PDF, CDF, and moments.

**The Problem:**\
Random variables rarely exist in isolation. Real-world phenomena often involve multiple interacting random variables.

* _Example 1 (Economics):_ The price of petroleum products (Random Variable $$X$$) and the price of essential commodities (Random Variable $$Y$$). An increase in one likely influences the other.
* _Example 2 (Signal Processing):_ A transmitted signal combined with atmospheric noise. The received signal is a combination of two random variables.
* _Example 3 (Behavioral):_ Two children with distinct individual behaviors may exhibit a completely different "joint behavior" when they interact.

**Key Questions:**\
When studying multiple random variables (e.g., $$X$$ and $$Y$$):

1. Does a change in one variable influence the other?
2. Are they independent?
3. How do we quantify this relationship?

***

### 2. Jointly Distributed Random Variables

To study the joint behavior of two random variables, we extend the concepts of probability from single events to pairs of variables.

#### A. Discrete Random Variables: Joint PMF

Let $$X$$ and $$Y$$ be two **discrete** random variables. We define the **Joint Probability Mass Function (Joint PMF)**.

**Definition:**\
The Joint PMF gives the probability that $$X$$ takes a specific value $$x_i$$ AND $$Y$$ takes a specific value $$y_j$$.\
$$P_{XY}(x_i, y_j) = P(X = x_i \cap Y = y_j)$$

**Representation:**\
This can be visualized as a table where rows represent values of $$X$$ and columns represent values of $$Y$$. The entries are the probabilities $$P_{XY}(x_i, y_j)$$.

**Properties:**

1. **Non-negativity:** $$0 \le P_{XY}(x_i, y_j) \le 1$$.
2. **Normalization:** The sum of all probabilities in the table must equal 1.\
   $$\sum_i \sum_j P_{XY}(x_i, y_j) = 1$$

#### B. Marginal Probability Mass Functions

From the joint distribution, we can recover the individual distribution of a single variable. This is called the **Marginal PMF**.

**Analogy:** Imagine a table where rows represent groups of people. If you sum the weights of all people in a specific row, you get the total weight for that row.

* **Marginal PMF of X:** Sum the joint probabilities over all values of $$Y$$.\
  $$P_X(x_i) = \sum_j P_{XY}(x_i, y_j)$$
* **Marginal PMF of Y:** Sum the joint probabilities over all values of $$X$$.\
  $$P_Y(y_j) = \sum_i P_{XY}(x_i, y_j)$$

#### C. Independence (Discrete Case)

The concept of independent events extends to random variables.

**Definition:**\
Two random variables $$X$$ and $$Y$$ are **independent** if the joint PMF is the product of the marginal PMFs for **all** pairs $$(x_i, y_j)$$.\
$$P_{XY}(x_i, y_j) = P_X(x_i) \cdot P_Y(y_j) \quad \forall i, j$$

If this condition fails for even a single pair, the variables are not independent.

***

### 3. Continuous Random Variables: Joint PDF

Let $$X$$ and $$Y$$ be two **continuous** random variables. We cannot define probability at a single point. Instead, we use the **Joint Probability Density Function (Joint PDF)**, denoted $$f_{XY}(x, y)$$.

**Visualization:**\
While a single PDF is a curve (2D), a joint PDF is a **surface** (3D) defined over the $$xy$$-plane. The probability corresponds to the **volume** under this surface.

**Properties:**

1. **Non-negativity:** $$f_{XY}(x, y) \ge 0$$.
2. **Normalization:** The total volume under the surface is 1.\
   $$\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f_{XY}(x, y) \, dy \, dx = 1$$

**Calculating Probability:**\
The probability that $$(X, Y)$$ falls within a specific region $$R$$ is the volume integral over that region.

#### A. Marginal Probability Density Functions

To find the individual density of one variable, we "integrate out" the other variable.

* **Marginal PDF of X:**\
  $$f_X(x) = \int_{-\infty}^{\infty} f_{XY}(x, y) \, dy$$
* **Marginal PDF of Y:**\
  $$f_Y(y) = \int_{-\infty}^{\infty} f_{XY}(x, y) \, dx$$

#### B. Independence (Continuous Case)

**Definition:**\
$$X$$ and $$Y$$ are independent if the joint PDF factors into the product of the marginal PDFs for all $$x, y$$:\
$$f_{XY}(x, y) = f_X(x) \cdot f_Y(y)$$

***

### 4. Joint Cumulative Distribution Function (Joint CDF)

The Joint CDF applies to both discrete and continuous variables.

**Definition:**\
$$F_{XY}(x, y) = P(X \le x, Y \le y)$$

**Geometric Interpretation:**\
This represents the probability that the random variable $$X$$ falls to the left of $$x$$ AND $$Y$$ falls below $$y$$. It essentially covers a rectangular region extending to $$-\infty$$ in both directions.

#### Properties of Joint CDF

1. **Bounds:** $$0 \le F_{XY}(x, y) \le 1$$.
2. **Limits:**
   * $$F_{XY}(\infty, \infty) = 1$$ (The entire sample space).
   * $$F_{XY}(-\infty, y) = 0$$ and $$F_{XY}(x, -\infty) = 0$$ (Impossible events).
3. **Monotonicity:** The function is non-decreasing. If $$x_1 \le x_2$$ and $$y_1 \le y_2$$, then $$F_{XY}(x_1, y_1) \le F_{XY}(x_2, y_2)$$.

#### Marginal CDF

We can obtain the marginal CDF from the joint CDF by taking the limit to infinity for the other variable.

* **Marginal CDF of X:**\
  $$F_X(x) = F_{XY}(x, \infty)$$
* **Marginal CDF of Y:**\
  $$F_Y(y) = F_{XY}(\infty, y)$$

#### Calculating Probability over a Rectangle

To find the probability that $$(X, Y)$$ lies within a rectangle defined by $$x_1 < X \le x_2$$ and $$y_1 < Y \le y_2$$:

$$P(x_1 < X \le x_2, y_1 < Y \le y_2) = F_{XY}(x_2, y_2) - F_{XY}(x_2, y_1) - F_{XY}(x_1, y_2) + F_{XY}(x_1, y_1)$$

This formula adds the region up to $$(x_2, y_2)$$ and subtracts the areas outside the desired rectangle, adding back the corner $$(x_1, y_1)$$ which gets subtracted twice.

***

### 5. Summary and Next Steps

**Summary:**

* **Joint Distribution:** Describes the behavior of multiple variables together.
* **Marginal Distribution:** Describes the behavior of a single variable extracted from the joint distribution.
* **Independence:** Occurs when the joint distribution is simply the product of the marginals.

**Upcoming Topics:**

* **Joint Moments:** How to calculate expected values for joint distributions (e.g., Covariance).
* **Conditional Probability for Random Variables:** Updating the distribution of one variable given the value of another.
* **Bayes' Theorem for Random Variables:** Connecting priors and posteriors in the context of probability distributions.

***

## Lecture 41: Joint Moments of Random Variables

**Course:** Mathematical Foundations for Machine Learning\
**Topic:** Joint Moments, Covariance, Correlation, and Variance of Sums

***

### 1. Introduction and Motivation

In previous lectures, we established the framework for jointly distributed random variables (Joint PMF, Joint PDF, Joint CDF). We now aim to quantify the relationship between two random variables, $$X$$ and $$Y$$, using numerical parameters.

* **Goal:** Define joint expected values (moments) to describe the relationship between variables.
* **Key Questions:** Can we calculate the expectation of sums or products? How do we measure if variables influence each other?

***

### 2. Expected Value of Joint Distributions

For a function $$g(X, Y)$$ of two random variables, the expected value is calculated using the joint distribution.

#### General Definition

For discrete random variables $$X$$ and $$Y$$:\
$$E[g(X, Y)] = \sum_i \sum_j g(x_i, y_j) P_{XY}(x_i, y_j)$$For continuous random variables, the summation is replaced by an integral over the joint PDF.

#### A. Expected Value of a Sum ($$X + Y$$)

Let $$Z = g(X, Y) = X + Y$$.

$$E[X + Y] = \sum_i \sum_j (x_i + y_j) P_{XY}(x_i, y_j)$$Separating the terms:\
$$E[X + Y] = \sum_i x_i \underbrace{\sum_j P_{XY}(x_i, y_j)}_{P_X(x_i)} + \sum_j y_j \underbrace{\sum_i P_{XY}(x_i, y_j)}_{P_Y(y_j)}$$$$E[X + Y] = E[X] + E[Y]$$

**Linearity of Expectation:**\
This property holds regardless of whether the variables are independent.\
$$E[aX + bY] = aE[X] + bE[Y]$$

#### B. Expected Value of a Product ($$XY$$)

Let $$Z = g(X, Y) = XY$$. The expected value of the product is called the **Correlation**.

$$E[XY] = \sum_i \sum_j x_i y_j P_{XY}(x_i, y_j)$$

**Case of Independence:**\
If $$X$$ and $$Y$$ are **independent**, then $$P_{XY}(x_i, y_j) = P_X(x_i) P_Y(y_j)$$. Substituting this into the equation:\
$$E[XY] = \left( \sum_i x_i P_X(x_i) \right) \left( \sum_j y_j P_Y(y_j) \right) = E[X]E[Y]$$**Note:** The converse is not necessarily true; $$E[XY] = E[X]E[Y]$$ does not guarantee independence.

**Orthogonality:**\
If $$E[XY] = 0$$, the random variables $$X$$ and $$Y$$ are said to be **Orthogonal**.

***

### 3. Joint Moments

We formalize the concept of moments for joint distributions.

#### A. Moments About the Origin (Raw Moments)

The $$(m+n)$$-th moment about the origin is defined as:\
$$m_{mn} = E[X^m Y^n]$$

* $$m_{10} = E[X]$$ (Mean of X)
* $$m_{01} = E[Y]$$ (Mean of Y)
* $$m_{11} = E[XY]$$ (Correlation)

#### B. Moments About the Mean (Central Moments)

The $$(m+n)$$-th central moment is defined as:\
$$\mu_{mn} = E[(X - \mu_X)^m (Y - \mu_Y)^n]$$

* $$\mu_{10} = E[X - \mu_X] = 0$$
* $$\mu_{01} = E[Y - \mu_Y] = 0$$
* $$\mu_{11} = E[(X - \mu_X)(Y - \mu_Y)]$$ (Covariance)

***

### 4. Covariance

Covariance is a measure of the joint variability of two random variables. It is the 1-1 joint central moment.

#### Definition

$$\text{Cov}(X, Y) = E[(X - \mu_X)(Y - \mu_Y)]$$Expanding this expression:\
$$\text{Cov}(X, Y) = E[XY - X\mu_Y - Y\mu_X + \mu_X\mu_Y]$$Using linearity of expectation ($$\mu_X, \mu_Y$$ are constants):\
$$\text{Cov}(X, Y) = E[XY] - \mu_Y E[X] - \mu_X E[Y] + \mu_X\mu_Y$$$$\text{Cov}(X, Y) = E[XY] - E[X]E[Y]$$

#### Uncorrelated Random Variables

If $$\text{Cov}(X, Y) = 0$$, the random variables $$X$$ and $$Y$$ are called **Uncorrelated**.

* This happens when $$E[XY] = E[X]E[Y]$$.

***

### 5. Variance of the Sum of Random Variables

We seek to find $$\text{Var}(Z)$$ where $$Z = X + Y$$.

**Derivation:**\
$$\text{Var}(Z) = E[Z^2] - (E[Z])^2$$$$\text{Var}(X+Y) = E[(X+Y)^2] - (E[X+Y])^2$$

1. Expand $$E[(X+Y)^2] = E[X^2 + 2XY + Y^2] = E[X^2] + 2E[XY] + E[Y^2]$$.
2. Expand $$(E[X+Y])^2 = (E[X]+E[Y])^2 = (E[X])^2 + 2E[X]E[Y] + (E[Y])^2$$.

Subtracting term 2 from term 1:\
$$\text{Var}(X+Y) = \underbrace{E[X^2] - (E[X])^2}_{\text{Var}(X)} + \underbrace{E[Y^2] - (E[Y])^2}_{\text{Var}(Y)} + 2 \underbrace{(E[XY] - E[X]E[Y])}_{\text{Cov}(X,Y)}$$

**Result:**\
$$\text{Var}(X+Y) = \text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X, Y)$$

#### Special Case: Independent Variables

If $$X$$ and $$Y$$ are independent, they are uncorrelated, meaning $$\text{Cov}(X, Y) = 0$$.\
$$\text{Var}(X+Y) = \text{Var}(X) + \text{Var}(Y)$$

* **Analogy:** This resembles the **Pythagorean theorem** ($$c^2 = a^2 + b^2$$). In statistics, the variances of independent variables add up like perpendicular sides of a right-angled triangle. This is the probabilistic version of the Pythagorean theorem.

***

### 6. Independence vs. Uncorrelatedness

A crucial distinction in probability theory:

1. **Independence implies Uncorrelatedness:**\
   If $$X$$ and $$Y$$ are independent $$\implies \text{Cov}(X, Y) = 0$$ (Uncorrelated).
2. **Uncorrelated does NOT imply Independence:**\
   If $$\text{Cov}(X, Y) = 0$$, $$X$$ and $$Y$$ are not necessarily independent.
   * _Reasoning:_ Covariance measures only the _linear_ relationship between variables. Variables might have a non-linear relationship (e.g., $$Y = X^2$$ over a symmetric interval), resulting in zero covariance but clear dependence.

**Summary Hierarchy:**

* **Independent:** Strongest condition. $$P_{XY} = P_X P_Y$$.
* **Uncorrelated:** Weaker condition. $$\text{Cov}(X,Y) = 0$$.
* **Orthogonal:** Weakest condition (specific to zero means). $$E[XY] = 0$$.

***

## Lecture 42: Independence and Correlation

**Course:** Mathematical Foundations for Machine Learning\
**Topic:** Independence vs. Zero Covariance, Counter-Examples, Units of Measurement, and Correlation Coefficient

***

### 1. Recap: Independence vs. Zero Covariance

In the previous lecture, we introduced the concept of Covariance ($$\text{Cov}(X, Y)$$). We concluded with a critical statement regarding the relationship between independence and correlation:

1. **Independence implies Zero Covariance:** If $$X$$ and $$Y$$ are independent, then $$\text{Cov}(X, Y) = 0$$.
2. **Zero Covariance does NOT imply Independence:** If $$\text{Cov}(X, Y) = 0$$, $$X$$ and $$Y$$ are not necessarily independent.

**The Trap:**\
It is tempting to assume that because independence leads to $$E[XY] = E[X]E[Y]$$ (which results in zero covariance), the converse must be true. However, this is false. We demonstrate this with a counter-example.

***

### 2. Counter-Example: Dependent but Uncorrelated

To prove that zero covariance does not imply independence, we construct a specific discrete joint probability distribution.

#### Setup

Consider two discrete random variables $$X$$ and $$Y$$ taking values in $$\{-1, 0, 1\}$$.\
We define the Joint Probability Mass Function (PMF) such that there are only four possible outcomes, each with equal probability $$P = 1/4$$:

1. $$(0, 1)$$
2. $$(0, -1)$$
3. $$(1, 0)$$
4. $$(-1, 0)$$

All other combinations $$(x, y)$$ have probability $$0$$.

#### Step 1: Calculate Marginal PMFs

By summing the rows and columns of the joint distribution table:

* **Marginal PMF of X (**$$P_X(x)$$**):**
  * $$P(X = -1) = 1/4$$
  * $$P(X = 0) = 1/4 + 1/4 = 1/2$$
  * $$P(X = 1) = 1/4$$
* **Marginal PMF of Y (**$$P_Y(y)$$**):**
  * $$P(Y = -1) = 1/4$$
  * $$P(Y = 0) = 1/4 + 1/4 = 1/2$$
  * $$P(Y = 1) = 1/4$$

#### Step 2: Calculate Expected Values

* $$E[X] = \sum x P_X(x) = (-1)(1/4) + (0)(1/2) + (1)(1/4) = 0$$
* $$E[Y] = \sum y P_Y(y) = (-1)(1/4) + (0)(1/2) + (1)(1/4) = 0$$

#### Step 3: Calculate Covariance

$$\text{Cov}(X, Y) = E[XY] - E[X]E[Y]$$Since $$E[X] = 0$$ and $$E[Y] = 0$$, we only need $$E[XY]$$.\
$$E[XY] = \sum \sum x y P_{XY}(x, y)$$Looking at the four possible points:

* $$(0, 1) \to 0 \cdot 1 = 0$$
* $$(0, -1) \to 0 \cdot (-1) = 0$$
* $$(1, 0) \to 1 \cdot 0 = 0$$
* $$(-1, 0) \to (-1) \cdot 0 = 0$$

In all valid cases, either $$x$$ or $$y$$ is zero. Therefore, $$E[XY] = 0$$.\
**Result:** $$\text{Cov}(X, Y) = 0 - 0 = 0$$.

#### Step 4: Check for Independence

Are $$X$$ and $$Y$$ independent?

* We check if $$P_{XY}(x, y) = P_X(x) P_Y(y)$$ for all points.
* Take the point $$(0, 1)$$.
  * $$P_{XY}(0, 1) = 1/4$$ (from definition).
  * $$P_X(0) \cdot P_Y(1) = (1/2) \cdot (1/4) = 1/8$$.
* Since $$1/4 \neq 1/8$$, the condition for independence fails.

**Intuitive Reasoning:**\
Look at the relationship between the variables. If you know $$Y = 1$$, you know with certainty that $$X$$ must be $$0$$ (the point must be $$(0, 1)$$). Similarly, if $$X = 1$$, $$Y$$ must be $$0$$.\
Knowing one variable gives **complete information** about the other. This is the opposite of independence; they are heavily dependent.

#### Conclusion

$$X$$ and $$Y$$ are dependent variables, yet their covariance is zero.\
**Why?** Covariance measures **linear** relationships. The relationship in this example ($$x^2 + y^2 = 1$$) is non-linear. Covariance failed to capture this non-linear dependence.

***

### 3. The Problem of Units

Covariance suffers from a practical issue: it is not dimensionless. It depends on the units of measurement of $$X$$ and $$Y$$.

**Example:**\
Let $$X$$ be weight and $$Y$$ be height.

* In India: Weight in kg, Height in cm. $$\text{Cov}(X, Y)$$ is in units of $$\text{kg} \cdot \text{cm}$$.
* In the US: Weight in lbs, Height in inches. $$\text{Cov}(X, Y)$$ is in units of $$\text{lbs} \cdot \text{in}$$.

If the exchange/convertion rate is $$1 \text{ kg} \approx 2.2 \text{ lbs}$$ and $$1 \text{ cm} \approx 0.4 \text{ in}$$, the magnitude of the covariance value changes drastically (multiplied by approx $$0.88$$).

**Another Example:**\
Comparing laptop prices ($$X$$) and mobile prices ($$Y$$) in Rupees ($$\text{Rupees}^2$$) vs Dollars ($$\$^2$$). A currency conversion factor (e.g., 80) would be squared ($$6400$$), making the covariance appear 6400 times larger in Rupees than in Dollars.

**The Need:** We need a measure of relationship that is independent of the scale and units used.

***

### 4. Correlation Coefficient ($$\rho_{XY}$$)

To solve the unit problem, we normalize the covariance. We divide by a measure that has the same units as the variables: the **Standard Deviation**.

**Analogy:** In vector algebra, to get a unit vector (dimensionless direction), we divide a vector by its magnitude (length). The standard deviation acts as the "length" or spread metric for a random variable.

#### Definition

The Correlation Coefficient, denoted by $$\rho_{XY}$$ (rho), is defined as:

$$\rho_{XY} = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y}$$

Alternatively, expressed in terms of expectation:

$$\rho_{XY} = E\left[ \left(\frac{X - \mu_X}{\sigma_X}\right) \left(\frac{Y - \mu_Y}{\sigma_Y}\right) \right]$$

Here, $$\frac{X - \mu_X}{\sigma_X}$$ and $$\frac{Y - \mu_Y}{\sigma_Y}$$ are the "standardized" versions of the random variables (mean 0, standard deviation 1).

#### Properties

1. **Dimensionless:** It has no units. It is a pure number.
2. **Range:** It is bounded between $$-1$$ and $$+1$$ (to be proven in the next lecture).
3. **Interpretation:** It measures the strength and direction of the **linear relationship** between $$X$$ and $$Y$$.

**Key Takeaway:** If $$\rho_{XY} = 0$$, we say $$X$$ and $$Y$$ are **uncorrelated**. This means there is no linear relationship between them. However, as seen in the counter-example, there might still be a strong non-linear relationship.

***

### 5. Summary

* **Independence vs. Zero Covariance:**
  * Independence $$\implies$$ Zero Covariance.
  * Zero Covariance $$\not\Rightarrow$$ Independence (Counter-example proved this).
* **Covariance:** Measures linear dependence but is sensitive to units/scale.
* **Correlation Coefficient (**$$\rho$$**):** A normalized, dimensionless measure of linear dependence.
* **Next Lecture:** We will explore the specific properties of the correlation coefficient and prove its bounds ($$-1 \le \rho \le 1$$).

***

## Lecture 43: Correlation and Covariance

**Course:** Mathematical Foundations for Machine Learning\
**Topic:** Correlation Coefficient, Interpretation of $$\rho$$, and Jointly Gaussian Random Variables

***

### 1. Recap: Covariance and Independence

**Context:**\
In previous lectures, we established the relationship between two random variables $$X$$ and $$Y$$.

* **Covariance:** Defined as $$\text{Cov}(X, Y) = E[XY] - E[X]E[Y]$$.
* **Uncorrelated:** If $$\text{Cov}(X, Y) = 0$$, the variables are uncorrelated.
* **Independence vs. Zero Covariance:**
  * Independence $$\implies$$ Zero Covariance.
  * Zero Covariance $$\does \not \implies$$ Independence (generally).
  * _Example:_ We saw a counter-example where variables were dependent (non-linear relationship) but had zero covariance.
* **The Problem with Covariance:** It is dependent on units of measurement (e.g., kg$$\cdot$$cm vs. lbs$$\cdot$$in). A change in units changes the magnitude of the covariance, making it difficult to compare relationships directly.

***

### 2. The Correlation Coefficient ($$\rho_{XY}$$)

To solve the unit dependency problem, we normalize the covariance to create a dimensionless measure.

#### Definition

The Correlation Coefficient, denoted by $$\rho_{XY}$$, is the standardized covariance:\
$$\rho_{XY} = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y}$$

Alternatively, defined as the expected value of the product of standardized variables:\
$$\rho_{XY} = E\left[ \left(\frac{X - \mu_X}{\sigma_X}\right) \left(\frac{Y - \mu_Y}{\sigma_Y}\right) \right]$$Here, $$\frac{X - \mu_X}{\sigma_X}$$ is the standardized variable (similar to the Z-score), which has zero mean and unit variance.

#### Properties

1. **Dimensionless:** It has no units, allowing for comparison across different scales.
2. **Range:** Using the Cauchy-Schwarz inequality, it can be proven that the value is bounded:\
   $$-1 \le \rho_{XY} \le 1$$
3. **Analogy to Cosine Similarity:**\
   Recall the dot product formula for vectors $$\mathbf{u}$$ and $$\mathbf{v}$$:\
   $$\cos \theta = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\| \|\mathbf{v}\|}$$The structure is identical to the correlation coefficient formula, where covariance is the "dot product" and standard deviations are the "magnitudes."

***

### 3. Interpretation of $$\rho_{XY}$$

The value of the correlation coefficient indicates the nature of the linear relationship between $$X$$ and $$Y$$.

#### A. Positive Correlation ($$\rho_{XY} = +1$$)

* **Interpretation:** Perfect positive linear relationship. If $$X$$ increases, $$Y$$ tends to increase. If $$X$$ decreases, $$Y$$ tends to decrease.
* **Example:** Number of classes attended vs. Attendance percentage. More attendance leads to a higher percentage.

#### B. Negative Correlation ($$\rho_{XY} = -1$$)

* **Interpretation:** Perfect negative linear relationship. If $$X$$ increases, $$Y$$ tends to decrease.
* **Example:** Number of classes bunked vs. Attendance percentage. More bunking leads to a lower percentage.

#### C. Zero Correlation ($$\rho_{XY} = 0$$)

* **Interpretation:** No linear relationship exists between $$X$$ and $$Y$$. The variables are **uncorrelated**.
* **Important Caveat:** This does not imply independence (unless the variables are Gaussian, see below). There may still be a strong non-linear relationship.

***

### 4. The Gaussian Exception: Uncorrelated implies Independent

We posed the question: _Does there exist a pair of random variables such that if they are uncorrelated, they are necessarily independent?_

**Answer:** Yes, if the random variables are **Jointly Gaussian Distributed**.

#### Jointly Gaussian Random Variables

Two random variables $$X$$ and $$Y$$ are jointly Gaussian if their joint PDF $$f_{XY}(x, y)$$ follows a specific bell-shaped distribution. The PDF is complex but includes parameters $$\mu_X, \mu_Y, \sigma_X, \sigma_Y$$, and $$\rho$$.

**The Derivation:**\
If $$X$$ and $$Y$$ are uncorrelated, then $$\rho = 0$$.

Substituting $$\rho = 0$$ into the Bivariate Gaussian PDF, the exponential term splits:\
$$\exp\left( -\frac{1}{2} \left[ \left(\frac{x-\mu_X}{\sigma_X}\right)^2 + \left(\frac{y-\mu_Y}{\sigma_Y}\right)^2 \right] \right)$$

This allows the joint PDF to be factored into the product of two individual Gaussian PDFs:\
$$f_{XY}(x, y) = \left[ \frac{1}{\sqrt{2\pi}\sigma_X} e^{-\frac{(x-\mu_X)^2}{2\sigma_X^2}} \right] \times \left[ \frac{1}{\sqrt{2\pi}\sigma_Y} e^{-\frac{(y-\mu_Y)^2}{2\sigma_Y^2}} \right]$$$$f_{XY}(x, y) = f_X(x) \cdot f_Y(y)$$

**Conclusion:**\
For jointly Gaussian random variables, uncorrelatedness implies independence.\
$$\text{Uncorrelated Gaussian RVs} \implies \text{Independent}$$

***

### 5. Visualizing Joint Gaussian Distribution

The Joint Gaussian PDF can be visualized as a 3D surface plot:

* **Axes:** $$X$$ and $$Y$$ form the horizontal plane; $$Z$$ (probability density) is the vertical axis.
* **Shape:** Resembles a "cowboy hat" or a bell shape extending in 3D.
* **Contours:** The level curves (contours) of equal density are ellipses. If $$\rho = 0$$, these ellipses become circles.

***

### 6. Correlation does not Imply Causation

A crucial warning in statistical analysis:

**Observation:** Finding a high correlation (positive or negative) between two variables does not mean one variable _causes_ the other.

* **Example:** Ice cream sales and shark attacks may be correlated (both increase in summer), but ice cream does not cause shark attacks. The hidden variable (temperature/season) drives both.
* **Rule:** Correlation measures association, not cause-and-effect.

***

### 7. Preview: Conditioning of Random Variables

We have studied joint distributions ($$P_{XY}$$) and independence. The next step is **Conditioning**.

Just as we defined conditional probability for events $$P(A|B) = \frac{P(A \cap B)}{P(B)}$$, we can define conditional probability for random variables.

* **Question:** If we know the value of one random variable (e.g., a feature in a dataset), how does that update our knowledge of another random variable (e.g., the class label)?
* **Next Topic:** Conditional Distributions for Discrete and Continuous Random Variables.

***

## Lecture 44: Joint Moments of Continuous Random Variables and Conditioning of Random Variables

**Course:** Mathematical Foundations for Machine Learning\
**Topic:** Joint Moments (Continuous), Conditional PMF/PDF, and Bayes' Theorem for Random Variables

***

### 1. Introduction and Recap

The lecture bridges the gap between discrete and continuous random variables regarding joint moments and introduces the concept of **Conditioning of Random Variables**.

* **Context:** Previously, we covered joint moments, covariance, and correlation for discrete variables.
* **Motivation:** In Machine Learning, we often need to calculate the probability of a specific outcome (class) given observed data (features). This requires understanding conditional distributions for random variables.

***

### 2. Joint Moments of Continuous Random Variables

The definitions for continuous random variables are direct extensions of the discrete case, replacing summations with integrations.

#### Definitions

Let $$X$$ and $$Y$$ be continuous random variables with a Joint Probability Density Function (PDF) $$f_{XY}(x, y)$$.

* **General Joint Moment:** The $$(m+n)$$-th moment about the origin is defined as:\
  $$E[X^m Y^n] = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} x^m y^n f_{XY}(x, y) \, dy \, dx$$

#### Specific Cases

1. **First Moment (Mean):**
   * Setting $$m=1, n=0$$: $$E[X] = \iint x f_{XY}(x, y) \, dy \, dx$$
   * Setting $$m=0, n=1$$: $$E[Y] = \iint y f_{XY}(x, y) \, dy \, dx$$
2. **Correlation:**
   * Setting $$m=1, n=1$$: $$E[XY] = \iint xy f_{XY}(x, y) \, dy \, dx$$
3. **Covariance:**
   * Defined similarly as the first central moment:\
     $$\text{Cov}(X, Y) = E[(X - \mu_X)(Y - \mu_Y)] = \iint (x - \mu_X)(y - \mu_Y) f_{XY}(x, y) \, dy \, dx$$

All properties derived for discrete variables (correlation coefficient, independence implications) hold for continuous variables as well.

***

### 3. Motivation for Conditioning

Recall conditional probability for events: $$P(A|B) = \frac{P(A \cap B)}{P(B)}$$. This represents updating our knowledge of event $$A$$ given that event $$B$$ has occurred.

We want to extend this to random variables. This is essential for ML problems where we ask: _"Given these features, what is the probability it belongs to this class?"_

#### Example: The Coin Experiment

Consider an experiment with two coins:

* **Coin 1:** Fair ($$P(H)=0.5, P(T)=0.5$$).
* **Coin 2:** Biased (e.g., double-headed, $$P(H)=1$$).

**Experiment:** Choose a coin at random and toss it five times.\
**Question:** What is the probability of observing more than two heads?

**Analysis:**

* The probability depends on which coin was chosen.
* We can use the **Law of Total Probability**:\
  $$P(K) = P(K|\text{Fair})P(\text{Fair}) + P(K|\text{Biased})P(\text{Biased})$$
* Here, the distribution of the number of heads ($$K$$) is conditioned on the choice of the coin. This motivates the definition of a **Conditional Probability Mass Function (PMF)**.

***

### 4. Conditional Probability Mass Function (Discrete)

#### Definition

Let $$X$$ and $$Y$$ be discrete random variables. The conditional PMF of $$Y$$ given $$X$$ is defined as:\
$$P_{Y|X}(y_j | x_i) = \frac{P_{XY}(x_i, y_j)}{P_X(x_i)}$$Where:

* $$P_{XY}(x_i, y_j)$$ is the joint PMF.
* $$P_X(x_i)$$ is the marginal PMF of $$X$$.

#### Properties

1. **Valid Probability:** For a fixed $$x_i$$, the conditional PMF satisfies the axioms of probability:
   * $$0 \le P_{Y|X}(y_j | x_i) \le 1$$
   * **Normalization:** $$\sum_j P_{Y|X}(y_j | x_i) = 1$$ (Summing over all possible $$y_j$$ for a specific $$x_i$$ yields 1).
2. **Family of PMFs:** The conditional PMF is actually a family of PMFs, one for each value of $$X$$.
3. **Important Note:** Summing over the conditioned variable (summing over $$i$$ for a fixed $$j$$) does **not** generally equal 1.

***

### 5. Conditional Probability Density Function (Continuous)

For continuous variables, we replace PMFs with PDFs. Note that point probabilities are zero, so we work with densities.

#### Definition

Let $$X$$ and $$Y$$ be continuous random variables with joint PDF $$f_{XY}(x, y)$$. The conditional PDF of $$Y$$ given $$X$$ is:\
$$f_{Y|X}(y|x) = \frac{f_{XY}(x, y)}{f_X(x)}$$

Where the marginal PDF $$f_X(x)$$ is:\
$$f_X(x) = \int_{-\infty}^{\infty} f_{XY}(x, y) \, dy$$

Similarly, the conditional PDF of $$X$$ given $$Y$$ is:\
$$f_{X|Y}(x|y) = \frac{f_{XY}(x, y)}{f_Y(y)}$$

***

### 6. Bayes' Theorem for Random Variables

We can apply Bayes' Theorem to relate conditional distributions.

#### Discrete Case

$$P_{Y|X}(y_j | x_i) = \frac{P_{X|Y}(x_i | y_j) P_Y(y_j)}{\sum_j P_{X|Y}(x_i | y_j) P_Y(y_j)}$$

* **Posterior:** $$P_{Y|X}$$ (Probability of class $$Y$$ given feature $$X$$).
* **Likelihood:** $$P_{X|Y}$$.
* **Prior:** $$P_Y$$.
* **Evidence (Denominator):** $$P_X(x_i)$$.

#### Continuous Case

$$f_{Y|X}(y|x) = \frac{f_{X|Y}(x|y) f_Y(y)}{\int_{-\infty}^{\infty} f_{X|Y}(x|y) f_Y(y) \, dy}$$

This forms the mathematical foundation for **Bayesian Classification** in Machine Learning.

***

### 7. Conclusion and Summary

This lecture completed the discussion on joint behavior by covering:

1. **Joint Moments** for continuous variables (integrals replacing sums).
2. **Conditional Distributions** (PMF and PDF), which update our knowledge of one variable based on the value of another.
3. **Bayes' Theorem** for random variables, connecting priors and posteriors.

**Preview of Next Module:**

* **Inequalities:** Markov and Chebyshev inequalities (tail probabilities).
* **Limit Theorems:** Central Limit Theorem (CLT).
* **Sample Geometry:** Vector perspective of samples.
* **Covariance Matrix:** Properties and applications.

***

***

***

## END CARD

***

***

***
