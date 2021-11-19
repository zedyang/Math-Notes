---
author:
- Zed
title: '**Linear Methods for Regression**'
---

Ordinary Least Squares
======================

We write the linear regression model
$$f(X) = \beta_0 + \sum_{j=1}^p X_j \beta_j = X^{\top} \beta$$ where
$\beta=(\beta_0, \beta_1..., \beta_p)^{\top}$.
$X=(1, X_1, ..., X_p)^{\top}$ is a $p+1$ column vector, with the inputs
$X_j$ being quantitative, factor variables
($X_j = \mathbbm{1}_{\{G=\mathcal{G}_j\}}$), transformation of
quantitative (say $\sin X_j$, $\log X_j$), basis expansions
($X_2 = X_1^2, X_3 = X_1^3$, ...) or cross terms ($X_3 = X_2 X_1$). We
have a quick review of the familiar OLS estimator before proceeding to
new concepts and models.

Algebraic Properties
--------------------

-   **Least Squares Estimator**: We choose sqaured error as loss
    function, and solve
    $$\hat{\beta} = \operatorname*{argmin}\limits_{\beta} \sum_{i=1}^N (y_i - \bm{x}_i^{\top}\beta)^2 = \operatorname*{argmin}\limits_{\beta} (\bm{y}-\bm{X}\beta)^{\top}(\bm{y}-\bm{X}\beta)$$
    by the familiar method of moments, and get
    $\hat{\beta} = (\bm{X}^{\top} \bm{X})^{-1}\bm{X}^{\top} \bm{y}$;  \

the prediction for *training set* is
$\hat{\bm{y}}=\bm{X}(\bm{X}^{\top} \bm{X})^{-1}\bm{X}^{\top} \bm{y}$,
which is, geometrically, an orthogonal projection of $\bm{y}$ onto the
column space of $\bm{X}$, i.e.
$\mathcal{C}(\bm{X})=\text{span}\{\text{Cols}(\bm{X})\}$. A few recap
and highlights:

-   (*Orthogonal Projection*) $\hat{\bm{y}}$ is within
    $\mathcal{C}(\bm{X})$, since $\hat{y}=\bm{X}\hat{\beta}$, a linear
    combination of the columns of $\bm{X}$. The residual
    $\bm{y}-\hat{\bm{y}}$ is orthogonal to the subspace
    $\mathcal{C}(\bm{X})$, since
    $\bm{X}^{\top}(\bm{y}-\hat{\bm{y}}) = \bm{X}^{\top}(\bm{y}-\bm{X}(\bm{X}^{\top} \bm{X})^{-1}\bm{X}^{\top} \bm{y}) = 0$.

-   (*Orthogonal Complement*) Our sample $\bm{y} \in \mathbb{R}^N$,
    which can always be decomposed as
    $\mathbb{R}^N = V \oplus V^{\perp}$, where $V$ is a subspace,
    $V^{\perp}$ is the orthogonal complement of $V$. We already have the
    column space $\mathcal{C}(\bm{X})$, and we can show that
    $\mathcal{C}(\bm{X})^{\perp} = \mathcal{N}(\bm{X}^{\top})$, the null
    space of $\bm{X}^{\top}$, which has dimension $N-p-1$.\
    *Proof.  * Suppose $\bm{z} \in \mathcal{C}(\bm{X})^{\perp}$, then
    $\bm{z}^{\top} \bm{X}\beta =0$ for all linear combination parameter
    $\beta \ne 0$. Hence the only way is
    $\bm{z}^{\top} \bm{X} = \bm{0}$, i.e.
    $\bm{X}^{\top} \bm{z} = \bm{0}$. $\square$\

-   (*Hat Matrix*) The matrix
    $\bm{H}_{\bm{X}} := \bm{X}(\bm{X}^{\top} \bm{X})^{-1}\bm{X}^{\top}$
    is called the “hat” matrix, which maps a vector to its orthogonal
    projection on $\mathcal{C}(\bm{X})$. (symmetric, idempotent, and
    maps columns of $\bm{X}$ to itself.) A curious object is the trace
    of this matrix:
    $$\text{tr}(\bm{H}_{\bm{X}}) = \text{tr}(\bm{X}(\bm{X}^{\top} \bm{X})^{-1}\bm{X}^{\top}) = \text{tr}(\bm{I}_{p+1}) = p+1$$

-   (*Residual*) We are also interested in the error of the estimator
    *within the training set*, i.e. define
    $\hat{\bm{u}}=\bm{y}-\hat{\bm{y}}$ as the residual term. It follows
    immediately that the residual sum of square
    $RSS= \hat{\bm{u}}^{\top} \hat{\bm{u}}$. And apply the hat matrix we
    see $\hat{\bm{u}}=(\bm{I}_N-\bm{H}_{\bm{X}})\bm{y}$. The object in
    between is also symmetric, idempotent, due to these property of
    $\bm{H}_{\bm{X}}$; consider
    $$(\bm{I}-\bm{H}_{\bm{X}})(\bm{I}-\bm{H}_{\bm{X}}) = \bm{I}-2 \bm{H}_{\bm{X}} + \bm{H}_{\bm{X}}$$

-   (*When $\bm{X}^{\top} \bm{X}$ is Singular*) When columns of $\bm{X}$
    are linearly dependent, $\bm{X}^{\top} \bm{X}$ becomes singular, and
    $\hat{\beta}$ is not uniquely defined. But $\hat{\bm{y}}$ is still
    the orthogonal projection onto $\mathcal{C}(\bm{X})$, just with more
    than one way to do the projection.

Statistical Properties
----------------------

(**Linear Assumptions**) To discuss statistical properties of
$\hat{\beta}$, we assume that the linear model is the true model for the
mean, i.e. the conditional expectation of $Y$ is $X\beta$, and that the
devation of $Y$ from the mean is additive, distributed as
$\epsilon \sim \mathcal{N}(0, \sigma^2)$. That is
$$Y=\mathbb{E}\left[Y\middle|X\right] + \epsilon = X\beta + \epsilon$$
We further assume that the inputs $\bm{X}$ in the training set are fixed
(non-random).

Under these assumptions, a few other highlights on statistical
properties of OLS estimator:

-   (*Expectation of $\hat{\beta}$*)
    $\mathbb{E}(\hat{\beta}) = \mathbb{E}\left[(\bm{X}^{\top}\bm{X})^{-1} \bm{X}^{\top}(\bm{X}\beta + \epsilon)\right] = \beta$, i.e.
    it is an unbiased estimator.

-   (*Variance of $\hat{\beta}$*)
    $\mathrm{\mathbb{V}ar}(\hat{\beta}) = \mathbb{E}\left[(\bm{X}^{\top}\bm{X})^{-1} \bm{X}^{\top} \bm{\epsilon} \bm{\epsilon}^{\top}(\bm{X}^{\top} \bm{X})^{-1} \bm{X}\right] = \sigma^2 (\bm{X}^{\top} \bm{X})^{-1}$.
    That is, the estimator
    $\hat{\beta}\sim \mathcal{N}(\beta, \sigma^2(\bm{X}^{\top} \bm{X})^{-1})$

-   (*Residual Revisited*) With the assumption of the real model of
    $\bm{y}$, we can further write
    $\hat{\bm{u}}=(\bm{I}-\bm{H}_{\bm{X}})\bm{y} = (\bm{I}-\bm{H}_{\bm{X}})(\bm{X}\beta+\bm{\epsilon}) =(\bm{I}-\bm{H}_{\bm{X}}) \bm{\epsilon} $.
    It is easy to see that
    $\mathbb{E}\left[\hat{\bm{u}}\right] = \mathbb{E}\left[\bm{X}(\beta-\hat{\beta})+\bm{\epsilon}\right] = 0$.
    And therefore
    $$\mathrm{\mathbb{V}ar}\left[\hat{\bm{u}}\right] = \mathbb{E}[\hat{\bm{u}}\hat{\bm{u}}^{\top}] = \mathbb{E}\left[(\bm{I}-\bm{H}_{\bm{X}}) \bm{\epsilon} \bm{\epsilon}^{\top}(\bm{I}-\bm{H}_{\bm{X}})\right] = \sigma^2 (\bm{I}-\bm{H}_{\bm{X}})$$
    So, although the errors $\bm{\epsilon}$ are i.i.d., residuals
    $\hat{\bm{u}}$ are correlated.

-   (*Individual Residual Term*) Pick any individual residual
    $\hat{u}_i$,
    $\mathrm{\mathbb{V}ar}\left[\hat{u}_i\right] = \sigma^2(1-h_i)$,
    where $h_i$ is the i-th diagonal entry of $\bm{H}_{\bm{X}}$.
    Furthermore
    $\mathrm{\mathbb{C}ov}\left[\hat{u}_i, \hat{u}_j\right] = \sigma^2 h_{ij}$,
    $i\ne j$, $h_{ij}$ is the row $i$, column $j$ entry in
    $\bm{H}_{\bm{X}}$.

 \
An unbiased estimator of residual variance (square of residual standard
error: $RSE^2$) is
$$\hat{\sigma}^2 = \frac{RSS}{N-p-1} = \frac{\hat{\bm{u}}^{\top} \hat{\bm{u}}}{N-p-1}$$

-   $\mathbb{E}(\hat{\sigma}^2) = \sigma^2$. We present two proofs.\
    *Proof (1).  * $$\begin{split}
                \mathbb{E}\left[\hat{\bm{u}}^{\top} \hat{\bm{u}}\right] = \mathbb{E}\left[\sum_{i=1}^N \hat{u}_i^2\right] = \sum_{i=1}^N \mathrm{\mathbb{V}ar}\left[\hat{u}_i\right] = \sum_{i=1}^N \sigma^2(1-h_i)
            \end{split}$$ By the trace formula we have discussed,
    $\sum{h_i} = \text{tr}(\bm{H}_{\bm{X}}) = p+1$. Hence
    $(2)=\sigma^2(N-p-1)$. We conclude that
    $$(N-p-1)\mathbb{E}(\hat{\sigma}^2) = \mathbb{E}\left[\bm{\epsilon}^{\top}(\bm{I}-\bm{H}_{\bm{X}})\bm{\epsilon}\right] = (N-p-1)\sigma^2~~~~~\square.$$
    Before the second proof, we present a lemma.

-   (**Distribution of Quadratic Form**)

    -   If an $n$-vector $\bm{x}$ is distributed as
        $\mathcal{N}(\bm{0}, \bm{\Sigma})$, then the quadratic form
        ${\bm{x}}^{\top} \bm{\Sigma}^{-1} {\bm{x}} \sim \chi^2(n)$.

    -   If an $n$-vector $\bm{x}$ is standard multivariate normal:
        $\mathcal{N}(\bm{0}, \bm{I})$, and $\bm{H}_{\bm{Z}}$ is a
        projection matrix onto the column space of $\bm{Z}$, which has
        dimension $r$ (i.e. consider $\bm{Z}$ is a $n\times r$ matrix,
        and $\bm{Z}$ and $\bm{H}_{\bm{Z}}$ both have rank $r$); then the
        quadratic form
        $\bm{x}^{\top} \bm{H}_{\bm{Z}} \bm{x}\sim \chi^2(r)$.

    *Proof of lemma.  * (*First Part*) Since $\bm{\Sigma}$ is symmetric
    positive definite, we have *Cholesky decomposition*
    $\bm{\Sigma} = \bm{Q}\bm{Q}^{\top}$, where $\bm{Q}$ is $n\times n$
    lower triangular.
    $$\bm{x}^{\top} \bm{\Sigma}^{-1}\bm{x} = \bm{x}^{\top} \bm{Q}^{-\top}\bm{Q}^{-1}\bm{x} = (\bm{Q}^{-1}\bm{x})^{\top}(\bm{Q}^{-1}\bm{x}) = \bm{z}^{\top} \bm{z}$$
    in which we let $\bm{z}:=\bm{Q}^{-1}\bm{x}$. It is clear that
    $\mathbb{E}\left[\bm{z}\right] = \bm{Q}^{-1} \mathbb{E}\left[\bm{x}\right] = 0$.
    And
    $$\mathrm{\mathbb{V}ar}\left[\bm{z}\right] = \mathbb{E}\left[\bm{\bm{Q}^{-1}\bm{x}}(\bm{Q}^{-1}\bm{x})^{\top}\right] = \bm{Q}^{-1} \mathbb{E}[\bm{x}\bm{x}^{\top}] \bm{Q}^{-\top} = \bm{Q}^{-1} \mathrm{\mathbb{V}ar}\left[\bm{x}\right] \bm{Q}^{-\top} = \bm{Q}^{-1} \bm{\Sigma} \bm{Q}^{-\top} = \bm{I}$$
    which indicates that $\bm{z}\sim \mathcal{N}(\bm{0}, \bm{I}_n)$ is
    an $n$-variate standard normal. It follows that
    $\bm{z}^{\top} \bm{z}\sim \chi^2(n)$.    $\square$\
    (*Second Part*)
    $$\bm{x}^{\top} \bm{H}_{\bm{Z}}\bm{x} = \bm{x}^{\top} \bm{Z}(\bm{Z}^{\top} \bm{Z})^{-1}\bm{Z}^{\top}\bm{x} = \bm{y}^{\top} \bm{\Omega}^{-1}\bm{y}$$
    in which we let $\bm{y}:=\bm{Z}^{\top} \bm{x}$ (an $r\times 1$
    vector), and $\bm{\Omega}:=\bm{Z}^{\top} \bm{Z}$ (an
    $r\times r$ matrix). This is exactly the form in part 1. And the
    linear transform of $n$-variate normal: $\bm{Z}^{\top} \bm{x}$ is
    distributed as $r$-variate normal
    $\mathcal{N}(\bm{0}, \bm{Z}^{\top} \bm{Z})$. By the result of part 1
    $\Rightarrow \bm{x}^{\top} \bm{H}_{\bm{Z}}\bm{x} \sim \chi^2(r)$.
    $\square$

    *Proof (2).  * $$\begin{split}
                (N-p-1)\hat{\sigma}^2 &= \hat{\bm{u}}^{\top} \hat{\bm{u}} = \bm{y}^{\top} (\bm{I}-\bm{H}_{\bm{X}})^{\top}(\bm{I}-\bm{H}_{\bm{X}})\bm{y} \\
                &= \bm{\epsilon}^{\top}(\bm{I}-\bm{H}_{\bm{X}})\bm{\epsilon} = \bm{\epsilon}^{\top} \bm{H}_{\bm{Z}} \bm{\epsilon}
            \end{split}$$ in which we let
    $\bm{H}_{\bm{Z}}:= \bm{I}-\bm{H}_{\bm{X}}$. By previous result, this
    is also symmetric, idempotent, and projects any vector to the null
    space of $\bm{X}^{\top}$, the orthogonal complement of
    $\mathcal{C}(\bm{X})$. We can always compose a matrix $\bm{Z}$ whose
    columns are the general solutions of $\bm{X}^{\top} \bm{z} = 0$.
    Clearly it has $N-p-1$ columns, since the orthogonal complement has
    dimension $N-p-1$. Hence $\bm{H}_{\bm{Z}}$ has $(N-p-1)$ rank.
    Morever,
    $\bm{\epsilon}^{\top} \bm{H}_{\bm{Z}} \bm{\epsilon} = \bm{\epsilon}^{\top}\bm{Z}(\bm{Z}^{\top} \bm{Z})^{-1}\bm{Z}^{\top} \bm{\epsilon}$,
    and $\bm{Z}^{\top} \bm{Z}$ is of $(N-p-1)\times(N-p-1)$. By lemma,
    and multiply a normalization factor $\Rightarrow$
    $\bm{Z}^{\top} \bm{\epsilon}/\sigma \sim \mathcal{N}(\bm{0}, (\bm{Z}^{\top} \bm{Z}))$,
    $\frac{1}{\sigma^2}\bm{\epsilon}^{\top} \bm{H}_{\bm{Z}} \bm{\epsilon} \sim \chi^2(N-p-1)$.
    So:
    $$\mathbb{E}\left[\bm{\epsilon}^{\top} \bm{H}_{\bm{Z}} \bm{\epsilon}\right] = \sigma^2(N-p-1)~~~~\square$$
    Proof (2) gives us a stronger result:

-   (*Distribution of Sample Estimator of Variance*) The residual sum of
    square is Chi squared distributed with degree of freedom $(N-p-1)$.
    $$(N-p-1)\hat{\sigma}^2 = RSS \sim \sigma^2 \chi^2(N-p-1)$$ In
    addition, $\hat{\beta}$ and $\hat{\sigma}$ are independent.

Hypothesis Tests
----------------

(**t Statistic**) The $t(n)$ distribution is defined as
$t(n)\sim \frac{\mathcal{N}(0,1)}{\sqrt{\chi^2(n)/n}}$. To test
hypothesis that a particular coefficient $\beta_j=0$, we formulate the
statistic
$$t_j = \frac{\hat{\beta}_j/\mathrm{se}(\hat{\beta_j})}{\sqrt{(N-p-1)\hat{\sigma}^2/(N-p-1)\sigma^2}} = \frac{\hat{\beta}_j}{\hat{\sigma} \cdot \mathrm{se}(\hat{\beta}_j)/\sigma} = \frac{\hat{\beta}_j}{\hat{\sigma} \sqrt{v_j}}$$
where $\hat{\sigma}=\sqrt{RSS/(N-p-1)}$, $\sqrt{v_j}$ is the $j$-th
diagonal element of $(\bm{X}^{\top} \bm{X})^{-1}$. And we know that
$\hat{\beta}_j/\mathrm{se}(\hat{\beta_j})\sim \mathcal{N}(\beta_j/\mathrm{se}(\hat{\beta_j}),1)$
and that
$\sqrt{(N-p-1)\hat{\sigma}^2/(N-p-1)\sigma^2}\sim \sqrt{\chi^2_{N-p-1}/(N-p-1)}$.
Under the null hypothesis $\beta_j=0$,
$\hat{\beta}_j/\mathrm{se}(\hat{\beta_j})\sim \mathcal{N}(0,1)$. We have
$t_j \sim t(N-p-1)$.  \
If we know $\sigma$ before hand, we just use it instead of
$\hat{\sigma}$. And $t_j$ reduces to
$\hat{\beta}_j/\mathrm{se}(\hat{\beta_j})\sim \mathcal{N}(0,1)$. Where
$\mathrm{se}(\hat{\beta_j}) = \sigma \sqrt{v_j}$.

 \
(**F Statistic**) The $\mathcal{F}(n_1, n_2)$ distribution is defined as
$\mathcal{F}(n_1, n_2)\sim \frac{\chi^2(n_1)/n_1}{\chi^2(n_2)/n_2}$. To
test hypothesis that $k$ coefficients $\beta_{[1]}=...=\beta_{[k]}=0$
simultaneously, we formulate the statistic
$$F = \frac{(RSS_0 - RSS_1)/p_1-p_0}{RSS_1/(N-p_1-1)}$$ Where the bigger
model 1 has $p_1+1$ parameters, the smaller model 0 (corresponds to null
hypothesis $H_0$) has $p_0+1$ parameters, $p_1-p_0=k$. We have
$F\sim \mathcal{F}(p_1-p_0, N-p_1-1)$ under the null hypothesis.

 \
(**Confidence Interval**) We can isolate $\beta_j$ to form a $1-2\alpha$
confidence interval
$$\beta_j \in (\hat{\beta}_j-z_{(1-\alpha)} \sqrt{v_j}\hat{\sigma}, \hat{\beta}_j+z_{(1-\alpha)} \sqrt{v_j}\hat{\sigma})$$
*Proof.  * We know that
$\hat{\beta}\sim \mathcal{N}(\beta, \sigma^2 (\bm{X}^{\top} \bm{X})^{-1})$,
a multivariate normal. So isolating $\hat{\beta}_j$, we have
$\hat{\beta}_j\sim \mathcal{N}(\beta_j, \sigma^2 v_j)$, where, as
before, $v_j$ is the j-th diagonal element of the covariance matrix of
$\hat{\beta}$. $\text{se}(\hat{\beta}_j)=\sigma \sqrt{v_j}$. And hence
$\frac{\hat{\beta}_j-\beta_j}{\sigma \sqrt{v_j}}\sim \mathcal{N}(0,1)$.
$$1- 2 \alpha = \mathbb{P}\left(\left|\frac{\hat{\beta}_j-\beta_j}{\sigma \sqrt{v_j}}\right|> z_{(1-\alpha)}\right) = \mathbb{P}\left(\hat{\beta}_j-z_{(1-\alpha)} \sqrt{v_j}\sigma<\beta_j<\hat{\beta}_j+z_{(1-\alpha)} \sqrt{v_j}\sigma\right)$$
And substitute $\sigma$ with the estimate $\hat{\sigma}$, yields the
result. $\square$

 \
(**Confidence Region**) We also obtain a confidence set for the entire
parameter vector $\beta$,
$$\beta \in C_{\beta} = \{(\hat{\beta}-\beta)^{\top}\bm{X}^{\top} \bm{X}(\hat{\beta}-\beta)\leq \hat{\sigma}^2 \chi^2_{p+1, (1-\alpha)}\}$$
*Proof.  * We know
$\hat{\beta}-\beta \sim \mathcal{N}(\bm{0}, \sigma^2 (\bm{X}^{\top} \bm{X})^{-1})$,
by *lemma* (Dist of quadratic form) part 1,
$(\hat{\beta}-\beta)^{\top} \frac{1}{\sigma^2}(\bm{X}^{\top} \bm{X}) (\hat{\beta}-\beta) \sim \chi^2(p+1)$.
Hence
$$1- \alpha = \mathbb{P}\left((\hat{\beta}-\beta)^{\top} \frac{1}{\sigma^2}(\bm{X}^{\top} \bm{X}) (\hat{\beta}-\beta) \leq \chi^2_{p+1, (1-\alpha)} \right) = \mathbb{P}\left((\hat{\beta}-\beta)^{\top} (\bm{X}^{\top} \bm{X}) (\hat{\beta}-\beta) \leq \sigma^2\chi^2_{p+1, (1-\alpha)} \right)$$
And substitute $\sigma$ with the estimate $\hat{\sigma}$, yields the
result. $\square$

Gauss Markov Theorem
--------------------

-   (**Gauss-Markov**) the least squares estimator has smallest variance
    among all *linear unbiased* estimates.

    *Proof.  * Let $\tilde{\beta}$ be an unbiased linear estimator other
    than $\hat{\beta}$, which is the ols estimator. By linearity:
    $\tilde{\beta} = \bm{A} \bm{y}$, where $\bm{A}$ is some (non-random)
    matrix. Hence we may decompose
    $\tilde{\beta} = ((\bm{X}^{\top} \bm{X})^{-1}\bm{X}^{\top} + \bm{C})\bm{y} = \hat{\beta} + \bm{Cy}$,
    where we let
    $\bm{C}:=\bm{A} - (\bm{X}^{\top} \bm{X})^{-1}\bm{X}^{\top}$.

     \
    By unbiasedness:
    $\beta=\mathbb{E}[\tilde{\beta}] = \mathbb{E}\left[\bm{Ay}\right] = \mathbb{E}\left[\bm{A}(\bm{X}\beta + \bm{\epsilon})\right] = \bm{AX} \beta + \bm{A} \mathbb{E}\left[\bm{\epsilon}\right]$.
    Since the last term has mean $\bm{0}$, this requires
    $\bm{AX}=\bm{I}$ $\Rightarrow \bm{CX}=\bm{O}$. Hence
    $\bm{Cy} = \bm{C}(\bm{X}\beta + \bm{\epsilon}) = \bm{C \epsilon}$.
    Therefore $$\begin{split}
                \mathrm{\mathbb{C}ov}[\hat{\beta}, \bm{Cy}] &= \mathrm{\mathbb{C}ov}[\hat{\beta}, \bm{C \epsilon}] = \mathbb{E}[(\hat{\beta} - \mathbb{E}\hat{\beta})(\bm{C \epsilon} - \bm{C}\mathbb{E}\bm{\epsilon})^{\top}] = \mathbb{E}[(\hat{\beta} - \beta)\bm{\epsilon}^{\top} \bm{C}^{\top}]  \\
                &= \mathbb{E}[(\bm{X}^{\top}\bm{X})^{-1} \bm{X}^{\top} \bm{\epsilon}\bm{\epsilon}^{\top} \bm{C}^{\top}] = \sigma^2 (\bm{X}^{\top} \bm{X})^{-1} (\bm{CX})^{\top} = \bm{O}
            \end{split}$$

    So:
    $$\mathrm{\mathbb{V}ar}[\tilde{\beta}]  = \mathrm{\mathbb{V}ar}[\hat{\beta} + \bm{Cy}] = \mathrm{\mathbb{V}ar}[\hat{\beta} + \bm{C \bm{\epsilon}}] = \mathrm{\mathbb{V}ar}[\hat{\beta}] + \sigma^2 \bm{C}\bm{C}^{\top}~~~~~\square$$

    Algorithm for Multiple Regression
    ---------------------------------

    For the univariate regression (with no intercept), we calculate ols
    estimator as:
    $$\hat{\beta}_1 = (\bm{x}^{\top}\bm{x})^{-1} \bm{x}^{\top} \bm{y} = \frac{\langle \bm{x}, \bm{x} \rangle}{\langle \bm{x}, \bm{y} \rangle}$$
    And the residual $\bm{r} = \bm{y} - \bm{x}\hat{\beta}$. Suppose
    $\langle \bm{x}_i, \bm{x}_j \rangle = 0$, i.e. $\bm{X}$ is an
    orthogonal matrix, then
    $\hat{\beta}_j = \langle \bm{x}_j, \bm{y} \rangle/ \langle \bm{x}_j, \bm{x}_j \rangle$,
    just write down $(\bm{X}^{\top}\bm{X})^{-1} \bm{X}^{\top} \bm{y}$
    and use the fact that $\bm{X}$ is orthogonal we can easily get
    the result. This implies that when the inputs are orthogonal, they
    have no effect on each other’s parameter estimates in the model.

     \
    For non-orthogonal $\bm{X}$, we perform the *Gram-Schmidt*
    orthogonalization procedure:

    -   (*Gram-Schmidt*) Suppose
        $\bm{X}=(\bm{1}, \bm{x}_1, ..., \bm{x}_p)$.

        -   Let $\bm{z}_0 \leftarrow  \bm{x}_0 \leftarrow  \bm{1}$.

        -   `For j = 1:p`: Regress $\bm{x}_j$ on
            $\bm{z}_0, ..., \bm{z}_{j-1}$ respectively to produce
            coefficients
            $\hat{\gamma}_{ij} \leftarrow \langle \bm{z}_i, \bm{x}_j \rangle / \langle \bm{z}_i, \bm{z}_i \rangle$,
            $i=0,1,...,j-1$; $\hat{\gamma}_{jj} \leftarrow 1$.

        -   Calculate residual
            $\bm{z}_j \leftarrow \bm{x}_j - \sum_{i=0}^{j-1} \hat{\gamma}_{ij} \bm{z}_i$

        -   Regress $\bm{y}$ on the residual $\bm{z}_j$ to produce
            $\hat{\beta}_j \leftarrow \langle \bm{z}_j, \bm{y} \rangle / \langle \bm{z}_j, \bm{z}_j \rangle$

    -   $\bm{Z} = (\bm{z}_0, \bm{z}_1..., \bm{z}_p)$ is orthogonal.\
        *Proof.  * We show by induction proof. Firstly, it is easy to
        see that
        $$\langle \bm{z}_0, \bm{z}_1 \rangle = \langle \bm{z}_0, \bm{x}_1 - \frac{\langle \bm{z}_0, \bm{x}_1 \rangle}{\langle \bm{z}_0, \bm{z}_0 \rangle} \bm{z}_0 \rangle = \langle \bm{z}_0, \bm{x}_1 \rangle - \langle \bm{z}_0, \bm{x}_1 \rangle = 0$$
        We assume $\langle \bm{z}_0, \bm{z}_k \rangle = 0$ for all
        $1 < k\leq j < p$. Then for $k=j+1$:
        $$\langle \bm{z}_0, \bm{z}_{j+1} \rangle = \langle \bm{z}_0, \bm{x}_{j+1}-\sum_{l=0}^j\frac{\langle \bm{z}_l, \bm{x}_{j+1} \rangle}{\langle \bm{z}_l, \bm{z}_l \rangle} \bm{z}_l\rangle = \langle \bm{z}_0, \bm{x}_{j+1} \rangle - \langle \bm{z}_0, \frac{\langle \bm{z}_0, \bm{x}_{j+1} \rangle}{\langle \bm{z}_0, \bm{z}_0\rangle} \bm{z}_0 \rangle = 0$$
        So we conclude that $\langle \bm{z}_0, \bm{z}_j \rangle = 0$ for
        $j=1,2,...,p$. Do the same induction for $\bm{z}_1$ as follows:

        -   Base case, using the fact (what we already known):
            $\langle \bm{z}_0, \bm{z}_1 \rangle = 0$
            $$\langle \bm{z}_1, \bm{z}_{2} \rangle = \langle \bm{z}_1, \bm{x}_{2}-\frac{\langle \bm{z}_0, \bm{x}_{2} \rangle}{\langle \bm{z}_0, \bm{z}_0\rangle} \bm{z}_0 - \frac{\langle \bm{z}_1, \bm{x}_{2} \rangle}{\langle \bm{z}_1, \bm{z}_1\rangle} \bm{z}_1\rangle = \langle \bm{z}_1, \bm{x}_2 \rangle - \langle \bm{z}_1, \bm{x}_2 \rangle = 0$$

        -   The induction, assume
            $\langle \bm{z}_1, \bm{z}_k \rangle = 0$ for all
            $2 < k\leq j < p$. Then for $k=j+1$:
            $$\langle \bm{z}_1, \bm{z}_{j+1} \rangle = \langle \bm{z}_1, \bm{x}_{j+1}-\sum_{l=0}^j\frac{\langle \bm{z}_l, \bm{x}_{j+1} \rangle}{\langle \bm{z}_l, \bm{z}_l \rangle} \bm{z}_l\rangle = \langle \bm{z}_1, \bm{x}_{j+1} \rangle - \langle \bm{z}_1, \frac{\langle \bm{z}_1, \bm{x}_{j+1} \rangle}{\langle \bm{z}_1, \bm{z}_1\rangle} \bm{z}_1 \rangle = 0$$

        So we conclude that $\langle \bm{z}_1, \bm{z}_j \rangle = 0$ for
        $j=2,...,p$. And the induction for $\bm{z}_i$, $i=2,3,...,p-1$
        in the same fashion, we have $\bm{Z}$ is orthogonal. $\square$

    Another observation is that $\bm{x}_j$ is a linear combination of
    $\bm{z}_k$, for $k\leq j$. Hence $\bm{Z}$ is a orthogonal basis for
    the column space of $\bm{X}$. Let
    $\bm{D}=\text{diag}(\left\|\bm{z}_j\right\|)$, then
    $\bm{Z}\bm{D}^{-1}$ gives the *orthonormal basis* of column sapce of
    $\bm{X}$. We denote $\bm{Q}:=\bm{Z}\bm{D}^{-1}$, which is also an
    orthogonal matrix.

     \
    By writing the algo in a matrix form, we denote
    $\bm{\Gamma}=\{\hat{\gamma}_{ij}\}$, which is an upper triangular
    matrix with main diagonal entries being $1$s. And hence we have
    $$\bm{X} = \bm{Z\Gamma} = \bm{ZD}^{-1} \bm{D\Gamma} =: \bm{QR}$$ And
    the ols estimator given by
    $$\hat{\beta} = (\bm{X}^{\top}\bm{X})^{-1} \bm{X}^{\top} \bm{y} = (\bm{R}^{\top}\bm{Q}^{\top}\bm{QR})^{-1} \bm{R}^{\top} \bm{Q}^{\top} \bm{y} = \bm{R}^{-1} \bm{R}^{-\top} \bm{R}^{\top} \bm{Q}^{\top} \bm{y} = \bm{R}^{-1} \bm{Q}^{\top} \bm{y}$$
    $$\hat{\bm{y}} = \bm{X}\hat{\beta} = \bm{QR}\bm{R}^{-1} \bm{Q}^{\top} \bm{y} = \bm{Q}\bm{Q}^{\top} \bm{y}$$

Subset Selection
================

-   (*Best-Subset Selection*) Look at all possible models at every given
    number ($k$) of variables chosen. (computationally expensive,
    becomes infeasible for $p$ much larger than 30-40 or so)

-   (*Forward-Stepwise Selection*) Rather than search through all
    possible subsets, we want to seek a path through them. FSS proceeds
    by sequentially adds into the model the predictor that most improves
    the fit. This is charactered as a *greedy algorithm*, which must
    produce a nested sequence of models, i.e. it may not find the best
    model, when, for example, the best subset of size 2 does not include
    that of size 1 (which may happen). However, it has lower variance
    compared with best-subset.

-   (*Backward-Stepwise Selection*) Starts with the full model, and
    sequentially deletes the predictors that has the least impact on
    the fit. Can only be used for $N>p$.

-   (*Forward-Stagewise (FS) Selection*) Start as the forward-stepwise,
    with intercept $\bar{y}$, and centered predictors with coefficients
    initially set as $0$. Then at each step, choose the variable that
    are most *correlated* with the current residual, then compute simple
    regression param $\gamma$ of residual on this varible, add this to
    the current $\beta_j$, i.e. $\beta_j \leftarrow \beta_j + \gamma$.
    Continues until none are correlated with the residual. The
    convergence of this algorithm can be slow, but it has good
    performance for problems with high dimensionality.

Subset selection is a *discrete* process, we either include a variable
or exclude it. As a result it often exhibits high variance. Shrinkage
methods are more continuous, and do not suffer from high variability.

Shrinkage Methods
=================

The motivation of various shrinkage methods is to overcome the
*combinatorial explosion* of the number of possible subsets (when $p$
large) by converting the discrete problem to a continuous one, which
turn out to be simpler to solve.

Ridge Regression
----------------

The ridge regression shrinks the regression coefs by imposing a penalty
on the magnitudes of these coefficients. Denote the ridge estimator
$\hat{\beta}^{ridge}$, it minimizes a penalized sum of squares:

$$\hat{\beta}^{ridge} = \operatorname*{argmin}\limits_{\beta} \left\{\sum_{i=1}^N (y_i - \beta_0 - \sum_{j=1}^p x_{ij}\beta_j)^2 + \lambda \sum_{j=1}^p \beta_j^2\right\}~~~~(*)$$
where $\lambda$ is a hyperparameter (complexity parameter) that controls
the amount of shrink. The solution of $(*)$ are not equivalent under the
scaling of inputs. Hence we usually standardize the input before solving
$(*)$. In addition, we don’t penalize the magnitude $\beta_0$

 \
The standardization procedure is done as: calculate the centered inputs
as $x_{ij}-\bar{x}_j$, (in the following text we assume
$\{\bm{X}\}_{ij}$ is this, has $p$ columns without $\bm{1}$), and
estimate $\hat{\beta}_0$ by $\sum_{1}^N y_i/N$.  \

-   **Ridge Regression Estimator**: We minimize loss function with
    penalization:
    $$\hat{\beta}^{ridge} = \operatorname*{argmin}\limits_{\beta} \{(\bm{y} - \bm{X}\beta)^{\top}(\bm{y} - \bm{X}\beta) + \lambda \beta^{\top} \beta\}$$
    We have
    $\partial RSS(\lambda)/\partial \beta = 2\bm{X}^{\top}(\bm{y} - \bm{X}\beta) + 2 \lambda \beta = 0$
    $\Rightarrow$
    $\hat{\beta}^{ridge} = (\bm{X}^{\top}\bm{X}+\lambda \bm{I}_p)^{-1} \bm{X}^{\top} \bm{y}$

Note that $\hat{\beta}^{ridge}$ is still a linear function of $\bm{y}$.
And $\bm{X}^{\top}\bm{X}+\lambda \bm{I}_p$ is nonsigular, even if
$\bm{X}^{\top} \bm{X}$ is singular.
