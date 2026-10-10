# CS740\_Midsem\_Q14\_Q15\_Solutions

## Q14. Principal Component Analysis (PCA)

This question focuses on the covariance matrix, variance along a direction, and how to compute PCA efficiently when the data dimension is much larger than the number of data points.

### Setup and notation

Suppose we have **N data points**, each in (\mathbb R^d). Put the points into the columns of a matrix:

\[ X=\[x\_1;x\_2;\cdots;x\_N]\in\mathbb R^{d\times N}. ]

Assume the data have been centered, meaning their mean is zero:

\[ \frac1N\sum\_{i=1}^N x\_i=0. ]

Using the population-covariance convention, the covariance matrix is

\[ C=\frac1NXX^T\in\mathbb R^{d\times d}. ]

(Some courses use (1/(N-1)) instead of (1/N). That changes the eigenvalues by a common factor, but not the principal directions/eigenvectors.)

### (a) Show that the covariance matrix is positive semidefinite

A matrix (C) is positive semidefinite (PSD) if it is symmetric and

\[ z^TCz\geq 0\quad\text{for every }z\in\mathbb R^d. ]

First, since (C=\frac1NXX^T),

\[ C^T=\frac1N(XX^T)^T=\frac1NXX^T=C. ]

So (C) is symmetric. Now take any vector (z):

\[ \begin{aligned} z^TCz &=z^T\left(\frac1NXX^T\right)z\ &=\frac1N z^TXX^Tz\ &=\frac1N(X^Tz)^T(X^Tz)\ &=\frac1N|X^Tz|\_2^2\ &\geq 0. \end{aligned} ]

The final quantity is a squared norm, so it cannot be negative. Therefore,

\[ \boxed{C\text{ is symmetric positive semidefinite.\}} ]

**Exam tip:** The fastest PSD proof is to rewrite (z^TCz) as a squared norm.

### (b) Show that the variance along a unit direction (w) is (w^TCw)

Take a unit vector (w\in\mathbb R^d), with (w^Tw=1). Project each data point onto this direction. Its scalar projection is

\[ y\_i=w^Tx\_i. ]

Because the original data are centered, the projected values also have mean zero. Their variance is therefore

\[ \begin{aligned} \operatorname{Var}(y) &=\frac1N\sum\_{i=1}^N y\_i^2\ &=\frac1N\sum\_{i=1}^N (w^Tx\_i)^2\ &=\frac1N\sum\_{i=1}^N w^Tx\_ix\_i^Tw\ &=w^T\left(\frac1N\sum\_{i=1}^N x\_ix\_i^T\right)w\ &=w^TCw. \end{aligned} ]

Thus,

\[ \boxed{\operatorname{Var}(w^Tx)=w^TCw.} ]

PCA chooses a unit direction that maximizes this variance:

\[ \max\_{w^Tw=1}w^TCw. ]

Since (C) is symmetric, it has an orthonormal eigenvector basis. The Rayleigh quotient result tells us that the maximum is the largest eigenvalue (\lambda\_1), attained when (w) is a corresponding eigenvector. Hence the **first principal component direction** is

\[ \boxed{w\_1=\text{eigenvector of }C\text{ for its largest eigenvalue}.} ]

The second principal direction is the eigenvector for the second-largest eigenvalue, and so on. The directions are orthogonal because (C) is symmetric.

**Interpretation:** A larger eigenvalue means the data have more variance in that principal direction. Keeping the first (k) directions retains the largest-variance (k)-dimensional subspace.

### (c) Efficient PCA when (d\gg N)

The direct covariance matrix (C=XX^T/N) is (d\times d). If the dimension (d) is enormous but there are only (N) samples, forming and eigendecomposing this matrix may be expensive.

Instead, form the smaller **Gram matrix**

\[ G=X^TX\in\mathbb R^{N\times N}. ]

Suppose its eigenpairs satisfy

\[ Gv\_i=\lambda\_i v\_i, \qquad |v\_i|\_2=1, \qquad \lambda\_i>0. ]

Define

\[ \boxed{u\_i=\frac{Xv\_i}{\sqrt{\lambda\_i\}}.} ]

We show that (u\_i) is a unit eigenvector of (XX^T):

\[ \begin{aligned} XX^Tu\_i &=XX^T\frac{Xv\_i}{\sqrt{\lambda\_i\}}\ &=\frac{X(X^TX)v\_i}{\sqrt{\lambda\_i\}}\ &=\frac{X(\lambda\_i v\_i)}{\sqrt{\lambda\_i\}}\ &=\lambda\_i\frac{Xv\_i}{\sqrt{\lambda\_i\}}\ &=\lambda\_i u\_i. \end{aligned} ]

Also,

\[ \begin{aligned} |u\_i|\_2^2 &=\frac{v\_i^TX^TXv\_i}{\lambda\_i}\ &=\frac{v\_i^T(\lambda\_i v\_i)}{\lambda\_i}\ &=1. \end{aligned} ]

Thus, the nonzero eigenvalues of (XX^T) and (X^TX) are the same, and their eigenvectors are related by (u\_i=Xv\_i/\sqrt{\lambda\_i}). For the covariance matrix (C=XX^T/N), the eigenvalues are (\lambda\_i/N), with the same eigenvectors (u\_i).

**Why this helps:**

* Direct method: eigendecompose a (d\times d) matrix.
* Gram-matrix method: eigendecompose an (N\times N) matrix.
* When (d\gg N), the second matrix is much smaller.

This is also a consequence of the SVD. If (X=U\Sigma V^T), the columns of (U) are the principal directions, and the squared singular values (divided by (N), under this covariance convention) are the covariance eigenvalues.

### Q14: final things to remember

\[ \boxed{C=\frac1NXX^T,\quad z^TCz=\frac1N|X^Tz|^2\geq0} ]

\[ \boxed{\text{Variance along unit }w=w^TCw} ]

\[ \boxed{\text{First PC} = \text{top eigenvector of }C} ]

\[ \boxed{X^TXv\_i=\lambda\_i v\_i\implies u\_i=\frac{Xv\_i}{\sqrt{\lambda\_i\}}} ]

***

## Q15. QR factorization, Gram–Schmidt, and Cholesky factorization

The key ideas are: Gram–Schmidt creates orthonormal columns for a QR factorization, while Cholesky factors a symmetric positive-definite matrix into a triangular matrix times its transpose.

### (a) QR factorization using Gram–Schmidt

For a matrix (A=\[a\_1;a\_2]) with linearly independent columns, Gram–Schmidt constructs orthonormal vectors (q\_1,q\_2):

\[ r\_{11}=|a\_1|,\qquad q\_1=\frac{a\_1}{r\_{11\}}, ]

\[ r\_{12}=q\_1^Ta\_2, \qquad u\_2=a\_2-r\_{12}q\_1, ]

\[ r\_{22}=|u\_2|,\qquad q\_2=\frac{u\_2}{r\_{22\}}. ]

Then

\[ Q=\[q\_1;q\_2],\qquad R=\begin{bmatrix}r\_{11}\&r\_{12}\0\&r\_{22}\end{bmatrix}, \qquad \boxed{A=QR}. ]

The columns of (Q) are orthonormal, so (Q^TQ=I), and (R) is upper triangular.

#### Worked example using (B=\begin{bmatrix}4&2\2&3\end{bmatrix})

Use the columns

\[ b\_1=\begin{bmatrix}4\2\end{bmatrix},\qquad b\_2=\begin{bmatrix}2\3\end{bmatrix}. ]

**Step 1: First orthonormal column**

\[ r\_{11}=|b\_1|=\sqrt{4^2+2^2}=\sqrt{20}=2\sqrt5. ]

\[ q\_1=\frac{b\_1}{r\_{11\}} =\begin{bmatrix}2/\sqrt5\1/\sqrt5\end{bmatrix}. ]

**Step 2: Project the second column onto (q\_1)**

\[ r\_{12}=q\_1^Tb\_2 =\frac{2}{\sqrt5}(2)+\frac{1}{\sqrt5}(3) =\frac7{\sqrt5}. ]

Remove this projection:

\[ \begin{aligned} u\_2&=b\_2-r\_{12}q\_1\ &=\begin{bmatrix}2\3\end{bmatrix} -\frac7{\sqrt5}\begin{bmatrix}2/\sqrt5\1/\sqrt5\end{bmatrix}\ &=\begin{bmatrix}-4/5\8/5\end{bmatrix}. \end{aligned} ]

**Step 3: Normalize the remaining component**

\[ r\_{22}=|u\_2|=\sqrt{\frac{16}{25}+\frac{64}{25\}} =\frac{4\sqrt5}{5}. ]

\[ q\_2=\frac{u\_2}{r\_{22\}} =\begin{bmatrix}-1/\sqrt5\2/\sqrt5\end{bmatrix}. ]

Thus,

\[ \boxed{Q=\frac1{\sqrt5} \begin{bmatrix}2&-1\1&2\end{bmatrix\}} ]

and

\[ \boxed{R= \begin{bmatrix} 2\sqrt5&7/\sqrt5\ 0&4\sqrt5/5 \end{bmatrix\}}. ]

Check: (Q^TQ=I), and multiplying (QR) gives (B).

### (b) Cholesky factorization of (B=\begin{bmatrix}4&2\2&3\end{bmatrix})

For a symmetric positive-definite matrix, Cholesky factorization writes

\[ \boxed{B=LL^T} ]

where (L) is lower triangular with positive diagonal entries. Let

\[ L=\begin{bmatrix}a&0\b\&c\end{bmatrix}. ]

Then

\[ LL^T= \begin{bmatrix} a^2\&ab\ ab\&b^2+c^2 \end{bmatrix}. ]

Match this with

\[ B=\begin{bmatrix}4&2\2&3\end{bmatrix}. ]

Equating corresponding entries gives

\[ a^2=4\implies a=2, ]

\[ ab=2\implies 2b=2\implies b=1, ]

\[ b^2+c^2=3\implies 1+c^2=3\implies c=\sqrt2. ]

Therefore,

\[ \boxed{L=\begin{bmatrix}2&0\1&\sqrt2\end{bmatrix\}}. ]

Check by multiplication:

\[ LL^T= \begin{bmatrix}2&0\1&\sqrt2\end{bmatrix} \begin{bmatrix}2&1\0&\sqrt2\end{bmatrix} =\begin{bmatrix}4&2\2&3\end{bmatrix}=B. ]

So the factorization is correct.

#### Why is Cholesky allowed here?

The matrix is symmetric. It is also positive definite because its leading principal minors are positive:

\[ 4>0,\qquad \det(B)=4(3)-2(2)=8>0. ]

For a symmetric (2\times2) matrix, these conditions establish positive definiteness.

### Q15: final things to remember

* **QR:** (A=QR), with (Q^TQ=I) and (R) upper triangular.
* **Gram–Schmidt:** subtract projections onto earlier orthonormal columns, then normalize.
* **Cholesky:** for symmetric positive-definite (B), write (B=LL^T), where (L) is lower triangular.
* Check your work by multiplying the factors back together.

For the matrix in this worked example:

\[ \boxed{B=QR,\quad Q=\frac1{\sqrt5}\begin{bmatrix}2&-1\1&2\end{bmatrix},\quad R=\begin{bmatrix}2\sqrt5&7/\sqrt5\0&4\sqrt5/5\end{bmatrix\}} ]

\[ \boxed{B=LL^T,\quad L=\begin{bmatrix}2&0\1&\sqrt2\end{bmatrix\}} ]

***

## Last-minute formula sheet

| Topic                       | Formula / fact                                                                |
| --------------------------- | ----------------------------------------------------------------------------- |
| Covariance of centered data | (C=XX^T/N)                                                                    |
| PSD proof                   | (z^TCz=\|X^Tz\|^2/N\geq0)                                                     |
| Projected variance          | (\operatorname{Var}(w^Tx)=w^TCw), for unit (w)                                |
| First principal component   | Eigenvector of (C) corresponding to the largest eigenvalue                    |
| Efficient PCA for (d\gg N)  | Diagonalize (X^TX); recover (u\_i=Xv\_i/\sqrt{\lambda\_i}) for (\lambda\_i>0) |
| QR factorization            | (A=QR), (Q^TQ=I), (R) upper triangular                                        |
| Cholesky factorization      | (B=LL^T), (L) lower triangular                                                |
