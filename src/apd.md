# A. Tensor Programs: llama2, r1
> *God does not play dice.*

<!-- *This appendix covers machine learning prerequisites by using language modeling as a running example.*
By the end of the appendix you should be comfortable with state of the art open
source models such as [llama](https://arxiv.org/abs/2407.21783) and [r1](https://arxiv.org/abs/2501.12948). -->

**Contents**
- [Prediction, Posteriors, Parameters](#overview)
    - [Prediction]()
        - [Non-parametric Inference with Posterior Updates]()
            - [Support Vector Machines (SVMs)]()
            - [Gaussian Processes (GPs)]()
            - [Bayesian Linear Models]()
            - [Bayesian Neural Networks (BNNs)]()
        - [Parametric Inference with Parameter Estimation](#12-parametric-inference-with-parameter-estimation)
            - [Generalized Linear Models (GLMs): Linear and Logistic Regression](#generalized-linear-models-glms-linear-and-logistic-regression)
            - [Deep Neural Networks (DNNs)](#deep-neural-networks-dnns)
    - [Posterior Updates](#probability-theory)
        - [Probability Theory]()
            - [Probability Space: Measurable Space + Measure](#probability-space)
            - [Discrete Random Variables: $Bin(n,p)$, $Poi(\lambda)$, $Geo(p)$]()
            - [Continuous Random Variables: $Uni(\alpha,\beta)$, $Exp(\lambda)$, $Nor(\mu, \sigma^2)$]()
            - [Joint Random Variables: Product Rule, Sum Rule, Bayes' Rule](#rulez-rule-product-rule-sum-rule-bayes-rule)
        - [Information Theory](#information-theory)
            - [Kullback–Leibler Divergence]()
            - [Entropy]()
            - [Compression]()
            - [Algorithmic Information Theory]()
        - [Linear Algebra]()
    - [Parameter Estimation](#statistics)
        - [Statistics]()
            - [Maximum Likelihood Estimation (MLE)](#maximum-likelihood-estimation-mle)
            - [Maximum a Posterior Estimation (MAP)](#maximum-a-posterior-estimation-map)
            - [Empirical Risk Minimization (ERM)](#empirical-risk-estimation-erm)
        - [Matrix Calculus](#matrix-calculus)
            - [Derivatives, Gradients, and Jacobians](#derivatives-gradients-and-jacobians)
            - [Automatic Diffrentiation: Sum Rule, Product Rule, Chain Rule](#automatic-differentiation)
        - [Optimization](#optimization)
            - [First Order Methods: GD, SGD, Adam, Shampoo, Muon]()
            - [Approximate Second Order Methods:]()
            - [Second Order Methods: Newtons Method ...]()
- [Pre-training: llama2, Post-training: r1]()

SAT.JUN.14
229 PREDICTION
- dnns
- theory
    - posteriors: **information theory** loss functions: (cross entropy. relative entropy. compression)
    - params: mle **stats**, param optimization is argmin(**optimization**) of gradient(**matrix calculus**)


# A.1 Prediction
The primary goal of supervised machine learning is to leverage patterns from stochastic
phenomena to **predict** quantities of interest without enumerating an entire
population. That is, to recover the underlying distribution from a random sample.
This is often the case with machine learning, where given dataset
$D=\{(\mathbf{x}^{(i)}, \mathbf{y}^{(i)}): (\mathbf{x}^{(i)}, \mathbf{y}^{(i)})\overset{\text{iid}}{\sim} \mathcal{X} \times \mathcal{Y} \}_{i=0}^{n}$
with $\mathcal{X} = \mathbb{R}^{d_{in}}$, $p(\mathbf{y}|\mathbf{x})$ needs to be
recovered. The assumption of **independence** is the statistical
questionably axiomatic counterpart to the probabilistic one  of **totality**. While
the assumption is philosophically and theoretically interesting to question with
alternative probabilistic frameworks and statistical modeling, the iid assumption
is a *useful* one to make progress in modeling. Returning to the problem setup,
when $\mathcal{Y} \subseteq \mathbb{N}$, the task is
**classification**, and when $\mathcal{Y} = \mathbb{R}^{d_{out}}$, the task is
**regression**.

There are a variety classes of functions that can be used to approximate
$p(\mathbf{y}|\mathbf{x})$, but the primary distinction is between **parametric**
and **non-parametric** models. The former have a *fixed* number of parameters
with respect to the size of the dataset where **differentiation** is
used to compute gradients in order to **optimize loss functions**. This stands in
contrast to the latter which has *variable* number of parameters where
**integration** is used to compute expectations in order to **update posteriors**.
Before the start of the deep learning "revolution" in 2012 with the ImageNet
"moment" the parametric/non-parametric distinction was more well known. There
were primarily three "schools of thought":

- the **"kernelists"**: proponents of **support vector machines (SVMs)**
- the **"bayesians"**: proponents of **gaussian processes (GPs)**
- the **"connectionists"**: proponents of **neural networks (NNs)**

where SVMs and GPs are non-parametric, and NNs are parametric. This chapter will
cover all three, highlighting their differences and similarities, and — as usual
in the artificial sciences — how this taxonification is in fact falsely trichotomous.
That is, by the end of this chapter the reader should understand how SVMs/GPs
are effectively **shallow networks** whereas NNs are **deep networks**,
and how the former is much better understood *theoretically* while the latter
is more successful *experimentally*.

todo: murphy's global/local latent distinction (with cho EBMs?)

---

## 1.1 Non-Parametric Inference with Posterior Updates

### Gaussian Processes
### Bayesian Logistic Regression
### Bayesian Neural Networks

---

## 1.2 Parametric Inference with Parameter Estimation

### Generalized Linear Models (GLMs): Linear and Logistic Regression

#### Linear Regression
Recall that the supervised learning problem is considered **regression** when
$\mathcal{Y} = \mathbb{R}$. spotify dataset

The **linear regression model** recovers $p(Y=y|\mathbf{X}=\mathbf{x};\mathbf{w})\sim \mathcal{N}(\mu, \sigma^2)$ that assumes $\mu$ is affine with respect to $x$. That is,

$$
\begin{aligned}
y^{(i)} :&= \mathbf w^{\mathsf T}\phi(x^{(i)}) + \epsilon^{(i)}
&&\text{[where $\epsilon^{(i)} \sim \mathcal{N}(\mu=0, \sigma^2)$]} \\
&\implies \epsilon^{(i)} = y^{(i)} - \mathbf{w}^{\mathsf T}\phi(x^{(i)}) \sim \mathcal{N}(\mu=0, \sigma^2) \\
&\implies y^{(i)} \sim \mathcal{N}(\mathbf{w}^{\mathsf T}\phi(x^{(i)}), \sigma^2) \\
&\implies p(y^{(i)}|x^{(i)}; \mathbf{w}) =  \frac{1}{\sqrt{2\pi\sigma^2}}\exp(\frac{[y^{(i)}-\mathbf{w}^{\mathsf T}\phi(x^{(i)})]^2}{2\sigma^2}) \\
\end{aligned}
$$

where error is unbiased. expectation(e) is 0.
error is model (unmodelled features im x)/data(measurement) uncertainty,
then the loss is the likelihood.

With the model now defined, the parameter $\mathbf{w}$ needs to be estimated from the data $D=\{(\mathbf{x}^{(i)}, \mathbf{y}^{(i)}): (\mathbf{x}^{(i)}, \mathbf{y}^{(i)})\overset{\text{iid}}{\sim} \mathcal{X} \times \mathcal{Y} \}_{i=0}^{n}$.
This is done by using the negatve log likelihood as the loss function $\mathcal{L}: \mathbb{R}^n \to \mathbb{R}$
to **minimize** so that $\mathcal{L(\mathbf{w})} := -\log \prod_{i=1}^{n} p(y^{(i)}|\mathbf{x}^{(i)})$
is fixed with respect to the data $(x^{(i)}, y^{(i)})$ and known variance $\sigma^2$:

$$
\begin{aligned}
\hat{\mathbf{w}}_{MLE} &\in \operatorname*{argmin}_{\mathbf{w} \in \mathbf{W}} \mathcal{L(\mathbf{w})} \\
                          &= \operatorname{argmin} - \sum_{i=1}^{n} \log [\frac{1}{\sqrt{2\pi\sigma^2}}\exp(\frac{[y^{(i)}-\mathbf{w}^{\mathsf T}\phi(x^{(i)})]^2}{2\sigma^2})] \\
                          &= \operatorname{argmin} - \sum_{i=1}^{n} \log \frac{1}{\sqrt{2\pi\sigma^2}} + \frac{[y^{(i)}-\mathbf{w}^{\mathsf T}\phi(x^{(i)})]^2}{2\sigma^2} \\
                          &= \operatorname{argmin} - \sum_{i=1}^{n} \frac{1}{2}[y^{(i)}-\mathbf{w}^{\mathsf T}\phi(x^{(i)})]^2
                          && \text{[by min wrt $\mathbf{w}$]}\\
\end{aligned}
$$

where $\operatorname{argmin}$ is implemented by first evaluating the **gradient**
$\nabla \mathcal{L(\mathbf{w})}$ and then iteratively applying **gradient descent**
for each time step $t$, $\mathbf{w}^{(t+1)} := \mathbf{w}^{t} -\alpha \nabla \mathcal{L(\mathbf{w})}$.

First, evaluating the gradient $\nabla \mathcal{L}(\mathbf{w})$ looks like:

$$
\begin{aligned}
\frac{\partial\mathcal{L}(\mathbf{w})}{\partial\mathbf{w}_j} &= - \frac{\partial}{\partial\mathbf{w}_j} \sum_{i=1}^{n} \frac{1}{2}[y^{(i)}-\mathbf{w}^{\mathsf T}\phi(x^{(i)})]^2 \\
                                                             &= - \sum_{i=1}^{n} \frac{\partial}{\partial\mathbf{w}_j} \frac{1}{2}[y^{(i)}-\mathbf{w}^{\mathsf T}\phi(x^{(i)})]^2 \\
                                                             &= - \sum_{i=1}^{n} [y^{(i)}-\mathbf{w}^{\mathsf T}\phi(x^{(i)})][-\phi(x^{(i)})_j]  \\
\end{aligned}
$$

And so the swapping indices $j$ for the entire gradient gives
$\nabla \mathcal{L}(\mathbf{w})=- \sum_{i=1}^{n} [y^{(i)}-\mathbf{w}^{\mathsf T}\phi(x^{(i)})][-\phi(x^{(i)})]$. Recall now that the second step in
implementing $\operatorname{argmin}$ after taking
$\nabla \mathcal{L}(\mathbf{w})$ is to then iteratively apply gradient descent
for each time step $t$, $\mathbf{w}^{(t+1)} := \mathbf{w}^{t} -\alpha \nabla \mathcal{L(\mathbf{w})}$.


#### Logistic Regression
Recall that the supervised learning problem is considered **classification**
when $\mathcal{Y} \subseteq \mathbb{N}$. The simple case when the output space is
$\mathcal{Y}=\{-1, 1\}$ is referred to as **binary classification**,
and in the case when the output space is $\mathcal{Y}=\{0,1, \ldots, k\}$ is
referred to as **multi-class classification**.

Many predictive values of interest that can be posed as answers to yes/no questions
are suitable to be modeled as binary classifiers. For instance, benign or malignant
in health care, innocent or guilty in law, real or spam in engineering. The running
example used for the next two models will be the domain of music — to classifying
a set of rap lyrics as a praise or diss. The set of lyrics $x^{(i)}$ that is of
particular interest is Kendrick's featured verse in Kanye's "No More Parties in LA":

```
Hey baby you forgot your Ray Bans
And my sheets still orange from your spray tan
It was more than soft porn for the K-man
She remember my Sprinter, said "I was in the grape van"
Uhm, well cutie, I like your bougie booty
Come Erykah Badu me, well, let's make a movie
Hell, you know my repertoire is like a wrestler
I show you the ropes, connect the dots
A country girl that love Hollywood
Mama used to cook red beans and rice
Now it's Denny's, 4 in the morning, spoil your appetite
Liquor pouring and niggas swarming your section with erection
Smoke in every direction, middle finger pedestrians
R&B singers and lesbians, rappers and managers
Music and iPhone cameras
This shit unanimous for you, it's damaging for you, I think
That pussy should only be holding exclusive rights to me, I mean
He flew you in this motherfucker on first class
Even went out his way so you could check in an extra bag
Now you wanna divide the yam like it equate the math?
That shit don't add up, you're making him mad as fuck
She said she came out here to find an A-list rapper
I said baby, spin that round and say the alphabet backwards
You're dealing with malpractice, don't kill a good nigga's confidence
Just cause he a nerd and you don't know what a condom is
The head still good though, the head still good though
Make me say "Nam Myoho Renge Kyo"
Make a nigga say big words and act lyrical
Make me get spiritual
Make me believe in miracles, Buddhist monks and Cap'n Crunch cereal
Lord have mercy, thou will not hurt me
Five buddies all herded up on a Thursday
Bottle service, head service, I came in first place
The opportunity, the proper top of breast and booty cheek
The pop community, I mean these bitches come with union fee
And I want two of these, moving units through consumer streets
Then my shoe released, she was kicking in gratuity
And yeah G, I was all for it
She said K Lamar, you kind of dumb to be a poet
I'mma put you on game for the lames that don't know they're a rookie
Instagram is the best way to promote some pussy
```

The sentiment of "No More Parties in LA" is extremely difficult for a machine to
classify correctly due to Kendrick's wordplay, double entendres, and insinuations.
Consider the positive/negative dictionaries where
positive={baby, cutie, love, good, spiritual, miracles, buddhist, buddies, opportunity, promote}
and negative={shit, spoil, damaging, motherfucker, mad, fuck, malpractice, kill, mercy, hurt, fee, dumb}.
Then a possible feature extractor $\phi: \mathcal{X} \to \mathbb{R}^3$ which maps
raw input to dimensioned feature representation is defined by:

| Dim             | Feature             | Value     |
|-----------------|-----------------------|------------------|
| $\phi(x)_1$ | $|\{ \forall w \in x : w \in positive\}|$                              | 10   |
| $\phi(x)_2$ | $|\{ \forall w \in x : w \in negative\}|$                          | 12    |
| $\phi(x)_3$ | $$\begin{cases} 1 & \text{if "lamar"} \in \text{x} \\ 0 & \text{otherwise} \end{cases}$$                          | 1 |

Now that the input space $\mathcal{X}$, feature extractor $\phi:\mathcal{X}\to \mathbb{R}^3$,
and output space $\mathcal{Y}=\{-1, 1\}$ is defined, the goal is to select some
function $f: \mathcal{X} \to \mathcal{Y}$.

todo: use regression. motivate the need for the range to be a p \in [0,1]

**Logistic Regression**

The logistic regression model is a **discriminative** $p(Y=y|\mathbf{X}=\mathbf{x}; \theta)\sim Ber(p)$
with a **decision boundary** $k \in [0,1]$ (in the case of $Ber(p)$, $k$ is usually set to $0.5$ so
as to divide the mass by 2) that assumes that the parameter $p$ is affine with respect to $x$. That is,
$$
\begin{aligned}
p(Y=1 \mid \mathbf X=\mathbf x) &= p := \sigma\bigl(\mathbf w^{\mathsf T}\phi(x)\bigr) 
                                 &&\text{[$\phi(x)_0 = 1$]} \\[4pt]
\implies\; p(Y=-1 \mid \mathbf X=\mathbf x) &= 1-p = 1-\sigma\bigl(\mathbf w^{\mathsf T}\phi(x)\bigr) \\[4pt]
\implies\; p(Y=y \mid \mathbf X=\mathbf x) &= p^{y}(1-p)^{1-y} \\[4pt]
                                           &= \sigma\bigl(\mathbf w^{\mathsf T}\phi(x)\bigr)^{y} \bigl[1-\sigma\bigl(\mathbf w^{\mathsf T}\phi(x)\bigr)\bigr]^{1-y}

\end{aligned}
$$

where $\sigma: \mathbb{R} \to [0,1]$ is a non-linear function
$\sigma := \frac{1}{1+\exp(-z)}$, and where $\mathbf{w^{\mathsf T} \phi(x)}$ is
referred to as the **logit** since the inverse $\sigma^{-1}:= \log \frac{p}{1-p}$
is defined as the log odds ratio.

With the model now defined, the parameter $\mathbf{w}$ needs to be estimated from the data $D=\{(\mathbf{x}^{(i)}, \mathbf{y}^{(i)}): (\mathbf{x}^{(i)}, \mathbf{y}^{(i)})\overset{\text{iid}}{\sim} \mathcal{X} \times \mathcal{Y} \}_{i=0}^{n}$.
This is done by using the negatve log likelihood as the loss function $\mathcal{L}: \mathbb{R}^n \to \mathbb{R}$
to **minimize** so that $\mathcal{L(\mathbf{w})} := -\log \prod_{i=1}^{n} p(y^{(i)}|\mathbf{x}^{(i)})$
is fixed with respect to the data $(x^{(i)}, y^{(i)})$

$$
\begin{aligned}
\hat{\mathbf{w}}_{MLE} &\in \operatorname{argmin} \mathcal{L(\mathbf{w})} \\
                          &= \operatorname{argmin} - \sum_{i=1}^{n} \log \sigma\bigl(\mathbf w^{\mathsf T}\phi(x^{(i)})\bigr)^{y^{(i)}} \bigl[1-\sigma\bigl(\mathbf w^{\mathsf T}\phi(x^{(i)})\bigr)\bigr]^{1-y^{(i)}} \\
                          &= \operatorname{argmin} - \sum_{i=1}^{n} y^{(i)}\log  \sigma\bigl(\mathbf w^{\mathsf T}\phi(x^{(i)})\bigr) + (1-y^{(i)})\log  \sigma\bigl(\mathbf w^{\mathsf T}\phi(x^{(i)})\bigr) \\
\end{aligned}
$$

where todo kl->ce.

where $\operatorname{argmin}$ is implemented by first evaluating the **gradient**
$\nabla \mathcal{L(\mathbf{w})}$ and then iteratively applying **gradient descent**
for each time step $t$, $\mathbf{w}^{(t+1)} := \mathbf{w}^{t} -\alpha \nabla \mathcal{L(\mathbf{w})}$.

First, to evaluate the gradient, $\nabla \mathcal{L}(\mathbf{w})$,
the negative log likelihood as loss function is simplified by defining
$\hat{y}^{(i)} := \sigma(\mathbf w^{\mathsf T}\phi(x^{(i)}))$ so that
$\nabla \mathcal{L}(\mathbf{w}) = - \sum_{i=1}^{n} y^{(i)}\log  \hat{y}^{(i)} + (1-y^{(i)})\log \hat{y}^{(i)}$.
Note that $\hat{y}$ is not the target label but the probability of the target label.
Then, since the derivative is linear with the derivative of the sum is the sum
of the derivatives where
$\frac{\partial}{\partial\mathbf{w}}\sum_{i=1}^{n}f(\mathbf{w}) = \sum_{i=1}^{n}\frac{\partial}{\partial\mathbf{w}}f(\mathbf{w})$, taking the derivative for a *single* example $i \in \{1, \ldots, n\}$
for a *single* parameter $\mathbf{w}_j$ where $\mathcal{L}(\mathbf{w}) = y\log  \hat{y} + (1-y)\log \hat{y}$ looks like

$$
\begin{aligned}
\frac{\partial \mathcal{L(\mathbf{w})}}{\partial\mathbf{w}_j} &= \frac{\partial\mathcal{L}(\mathbf{w})}{\partial\hat{y}} \frac{\partial \hat{y}}{\partial \mathbf{w}_j}
&& \text{[by chain rule]}\\
&= \frac{\partial\mathcal{L}(\mathbf{w})}{\partial\hat{y}} \hat{y}(1-\hat{y})\phi(x)_j
&& \text{[by $\frac{d\sigma}{dz}= \sigma(1-\sigma)$]}\\
&= \bigl[\frac{y}{\hat{y}} - \frac{1-y}{1-\hat{y}}\bigr] \hat{y}(1-\hat{y})\phi(x)_j
&& \text{[by $\frac{d\log(x)}{dx} = \frac{1}{x}$]}\\
&= \bigl[\frac{y(1-\hat{y}) - \hat{y}(1-y) }{\hat{y}(1-\hat{y})}\bigr] \hat{y}(1-\hat{y})\phi(x)_j \\
&= \bigl[y(1-\hat{y}) - \hat{y}(1-y)\bigr]\phi(x)_j\\
&= \bigl(y -\hat{y})\phi(x)_j\\
\end{aligned}
$$

and so the evaluating the derivative of all examples
$\nabla \mathcal{L}(\mathbf{w}) = - \sum_{i=1}^{n} y^{(i)}\log  \hat{y}^{(i)} + (1-y^{(i)})\log \hat{y}^{(i)}$
where $\hat{y}^{(i)} := \sigma(\mathbf w^{\mathsf T}\phi(x^{(i)}))$ looks like

$$
\begin{aligned}
\frac{\partial \mathcal{L(\mathbf{w})}}{\partial\mathbf{w}_j} &= - \sum_{i=1}^{n} \frac{\partial}{\partial \mathbf{w}_j}\bigl[y^{(i)}\log  \hat{y}^{(i)} + (1-y^{(i)})\log \hat{y}^{(i)}\bigr] \\
&= - \sum_{i=1}^{n} \bigl(y^{(i)} -\hat{y}^{(i)})\phi(x^{(i)})_j \\
&= - \sum_{i=1}^{n} \bigl(y^{(i)} -\sigma(\mathbf w^{\mathsf T}\phi(x^{(i)}))\bigr]\phi(x^{(i)})_j \\
\end{aligned}
$$

And so the swapping indices $j$ for the entire gradient gives
$\nabla \mathcal{L}(\mathbf{w})=- \sum_{i=1}^{n} \bigl(y^{(i)} -\sigma(\mathbf w^{\mathsf T}\phi(x^{(i)}))\bigr]\phi(x^{(i)})$. Recall now that the second step in
implementing $\operatorname{argmin}$ after taking
$\nabla \mathcal{L}(\mathbf{w})$ is to then iteratively apply gradient descent
for each time step $t$, $\mathbf{w}^{(t+1)} := \mathbf{w}^{t} -\alpha \nabla \mathcal{L(\mathbf{w})}$.

**Multinomial Logistic Regression**

#### GLM: $\mathcal{N}(\mu, \sigma^2)$ and $Ber(p)$ is $Exp()$
sigmoid justification
- nice derivative \frac{d\sigma}{d?} = \sigma(1-\sigma) (which helps with calculus -> optimization -> parameter estimation)
- glms justify the choice of logistic/sigmoid non-linear squashing function 1/1+exp(-z).


### Deep Neural Networks (DNNs)

| Feature                                                                                     | Dimension             | Value     |
|-----------------|-----------------------|------------------|
| $|\{ \forall w \in x : w \in positive\}|$                                                                    | $\phi(x)_1$                              | 12   |
| $|\{ \forall w \in x : w \in negative\}|$                                                                    | $\phi(x)_2$                          | 11    |
| $$\begin{cases} 1 & \text{if "lamar"} \in \text{x} \\ 0 & \text{otherwise} \end{cases}$$              | $\phi(x)_3$                          | 1 |

which apriori seems difficult to learn a useful mapping for, given that the
first and second dimension of the feature representation which respectively encode
positive/negative sentiment are roughly tied. For instance, what is the feature
extractor that maps the following set of lyrics (with subtle insinuations)
into a *useful* feature representation for a sentiment classifier?

```
A country girl that love Hollywood
Mama used to cook red beans and rice
Now it's Denny's, 4 in the morning, spoil your appetite
Liquor pouring and niggas swarming your section with erection
```

The short answer is that it's difficult because the concept and negative sentiment
becomes apparent after reading the entire paragraph rather than any specific word.
This is the motivation for representation learning.

<!-- ## Energy Perspective: Parameterization, Learning, Inference.
The energy perspective to machine learning not only provides a unifying
foundation, but also relates the discipline and it's toolbox to that of the one
used by physicists dating back to the 19th century, which is
describing systems in terms of **minimizing free energy**. The free energy
$e: \mathcal{O} \times \mathcal{Z} \times \Theta \to \mathbb{R}$
is the incomptability score between an observed/unobserved instance $(x,z)$ and is
parameterized by $\theta$. TODO: rubber band?

TODO: if we do not observe z, we cannot fully determine how preferable x is?
Partitioning the observation space into input/output space $\mathcal{X}$/$\mathcal{Y}$,
and given unseen input $x'$ with no unobserved $z=\varnothing$, the goal
of **supervised learning** is to search for the the $y$ that minimizes the free
energy with:
$$
\hat{y} := \argmin_{y\in \mathcal{Y}}e([x',y], \varnothing, \theta)
$$

where the countable discrete case $\mathcal{Y}\subseteq\mathbb{N}$ is **classification**
and the uncountable continuous case $\mathcal{Y}=\mathbb{R}$ is **regression**. With respect
to the unobserved space $\mathcal{Z}$, the goal is to search for the $z$ that
minimizes the free enrgy with:
$$
\hat{z} := \argmin_{z\in \mathcal{Z}}e([x,y], z, \theta)
$$

where the discrete is **clustering**, the continuous case is **representation learning**,
and noting that $x$ is not primed.

From this perspective, all machine learning paradigms are given some
**partial observation** and must **infer** the **unobserved**. This is done by
via **optimization** with respect to some subset of the energy function's domain.
Inference defined this way more general than forward pass evaluation of the
learned model — inference is now expanded to **search**. This is why the
discipline has overloaded the term *inference*, because the goal
is to search for a prediction about *the* future, not *the* model used to predict
the future.

TODO
- learning (backprop. sgd) of $\theta$
- parameterization (networks) of $e$ -->



# A.2 Posterior Updates
## Probability Theory

By default, mathematical reasoning is understood to be deterministic where
a **statement** $S$ is either true (holds) or false (does not hold). Any
**variable** $x$ can only take on one specific value at a time. However there are
other times where what's desirable is describing phenomena that is in fact
non-deterministic (while still remaining precise). Even if base reality is
fundamentally deterministic, in practice there are many scenarios where carrying
out calculation is intractable. i.e, predicting tomorrow's weather with the
position of every water molecule.

The most widely adopted mathematical language for formalizing our intuitions
around non-deterministic stochastic phenomena is probability theory (as opposed
to alternative frameworks such as [probabilistic logic](https://en.wikipedia.org/wiki/Probabilistic_logic) or
[uncertainty quantification](https://en.wikipedia.org/wiki/Uncertainty_quantification)).
In probability theory  **statements** $S$ are neither true nor false. Rather,
truth is distributed across a weighted set of **random events** $E$. Similarly,
**random variables** $X$ and their **probability distributions** $p$ do not take
on definite values but rather sets of values. Probability theory is the
generalization of aristotelian logic.


### Probability Space: Measurable Space + Measure
---

These three **concrete implementations** create the toolbox which serve as
workhorses for probability theory and require an **abstract interface**. The
Kolmogorov's 1933 formulation of probability theory in
[Grundbegriffe der Wahrscheinlichkeitsrechnung](https://archive.org/details/kolmogoroff-1933-grundbegriffe-der-wahrscheinlichkeitsrechnung/page/10/mode/2up)
is the universally accepted model for random events and random variables.
The guiding light in the axiomaticization are the first two out of three properties
that $p$ should satisfy: *non-negativity* and *normalization*, which formalize
*impossible (surely false)* and *guaranteed (surely true)* statements. They
ensure that probabilities are never less than 0 nor greater than 1.

The foundations start with an ambient sample space $\Omega$ which is the set of all
possible **outcomes** $\omega$ in an experiment. It's natural to expect the
probability function $p$ to be defined on $\Omega$, which is acceptable when
the sample space is **countable**. That is, when $|\Omega|<\aleph_1$,
the first two properties are possible to satisfy:
- **non-negativity:** $\forall \omega \in \Omega, P(\omega) > 0.$
- **normalization:** $\sum\limits_{i=0}^{\infty} P(\omega_i)=1.$

but when the sample space is **infinitely uncountable** then normalization is not possible.
That is, when $|\Omega| = \aleph_1$, TODO... The next approach then is to define
probabilities for the powerset $\mathcal{F}=2^\Omega$ with $p: \mathcal{F} \to [0,1]$.
The powerset $F$ is the set of all subsets, and is referred to as the
**event space**. The event space must be a **field**. That is, it must be
**closed** under union, intersection, and complement, satisfying:
- $\varnothing \in \mathcal{F} \land \Omega \in \mathcal{F}$.
- $A \in \mathcal{F} \implies \Omega \setminus A\in \mathcal{F}$.
- $A \in \mathcal{F} \land B \in \mathcal{F} \implies A \cup B \in \mathcal{F} \land A \cap B \in \mathcal{F} $.

This formulation of $p$ is now defined on $E \subset \Omega$ but still does not
support sample spaces where $|\Omega|=\aleph_1$ let alone $|\Omega|=\aleph_0$.
The property of *closure* of union and intersection need to be extended to support
sample spaces that are countable and uncountable infinite. Starting with the
former, a field $F$ that satisfies
- $E_1, E_2, \ldots \in \mathcal{F} \implies \bigcup\limits_{i=1}^{\infty}E_i \in \mathcal{F} \land \bigcup\limits_{i=1}^{\infty} \in \mathcal{F}$.

is then a **$\sigma$-field** and is defined for $|\Omega|=\aleph_0$. This 
**$\sigma$-field** with the sample space $\Omega$ form a **measurable space**.
Finally, the original goal that motivated the need to redefine $p$'s domain from
$\Omega$ to $\mathcal{F}=2^\Omega$ was to support uncountable infinities where
$|\Omega|=\aleph_1$. This motivates the need to add additional properties the
event space must satisfy which is...
this is a borel sigma field.

Returning to the probability function $p$, the original first two properties are
satisified, in addition to a third and final one:
- **non-negativity:** $\forall \omega \in \Omega, P(\omega) > 0.$
- **normalization:** $\sum\limits_{i=0}^{\infty} P(\omega_i)=1.$
- **additivity:**

A measurable sapce $(\Omega, \mathcal{F})$ with a measurable function $p: \mathcal{F} \to [0,1]$
is the abstract model for a **probability space**.

<!-- going back to $p$, this lets us normalize on sample spaces that are uncountably infinte?
we add a third property here?:
 whereas the third property — arguably
the most crucial — ensures we can add probabilities when two statements don't
"overlap".

Subsets of this sample space are **events** which are constructed by placing a boolean operator
(corresponding to some semantic invariant) on the sample space. Functions
$X: \Omega \to \mathbb{R}$ are **random variables**.. -->

##### Example 1: finite probability space
---

In the notebook's running example, consider describing a rappers vocabulary as
some sample space $\Omega=\{ acid, balls, chop, death, grind, mind, poppin, pills, razor, shallow, tequila, zoey \}$,
and the following probability assignment (take a moment to confierm that they
are all *non-negative* and *normalize* to 1).
- $P(\{acid\})=0.04$
- $P(\{balls\})=0.01$
- $P(\{chop\})=0.09$
- $P(\{death\})=0.08$
- $P(\{grind\})=0.08$
- $P(\{mind\})=0.05$
- $P(\{poppin\})=0.04$
- $P(\{pills\})=0.19$
- $P(\{razor\})=0.03$
- $P(\{shallow\})=0.22$
- $P(\{tequila\})=0.15$
- $P(\{zoey\})=0.02$

Let $X: \Omega \to \mathbb{R} $ be the r.v that maps words to their
lengths, and let $Y: \Omega \to \mathbb{R}, $ be the r.v that maps the word
$\omega$ to the ordinal-based unicode number of the first letter

- the rv taking on value $X=5$ constructs the event modeled by the subset $A=\{balls, death, grind, pills, razor\}$ and the p
- the rv taking on value $Y=112$ constructs the event modeled by the subset $B=\{poppin, pills\}$


The event where $X=5$ and
$Y=112$ constructs the subset $C=A\cup B=\{pills\}$. The event where $X=5$ or
$Y=112$ constructs the subset $C=A\cap B=\{balls, death, grind, poppin, pills, razor\}$


##### Example 2: continuous probability space
---

### Discrete Random Variables

### Continuous Random Variables

### Rulez Rule: Product Rule, Sum Rule, Bayes' Rule

With an axiomaticization in place where **random events** $E$ and **random variables**
$X$ are modeled as measurable subsets and measurable functions of a measurable space $\Omega$,
the focus is now on the core — invariant to sample space modifications — probabilistic
concepts that comprise the notion of updating beliefs.

It all starts with of **conditional** probability, which is the probability of some
event $A$ **given** that event $B$ was **observed**. If the Kolmogorov
axiomaticization admits modeling stochastic events $A$ and $B$ as a venn diagram
within an ambient sample space, then **conditional** probability changes the
fidelity of description so that $B$ is the new (and narrowed) "sample space"
being considered. That is, probability of $A$ given $B$ is denoted with
$P(A\mid B)$ and is defined as

$$P(A\mid B)\stackrel{\triangle}{=}\frac{P(A\cap B)}{P(B)}$$

and can itself be proved a valid probability space. In other scenarios
$P(A\mid B)$ is given, but $P(A\cap B)$ is not. This is
amenable by rearranging the definition and solving for the latter with
$P(A\cap B) = P(B)P(A\mid B)$. and is referred to as the **product rule**
(emphasis on the product) or **chain rule** (emphasis on the factors).

In other scenarios $P(A\mid B)$ is given (and now $P(A \cap B)$ by the product rule),
but $P(A)$ is not. This too is amenable by using event $B$ as a "background event",
where assuming events $B_1, B_2, \ldots B_n$ are mutually exclusive, then
$P(A) = P(A \cap B_1) +  P(A \cap B_2) + \ldots + P(A\cap B_N)$. This is referred
to as the **sum rule**, and can be composed with the product rule to further
decompose the terms of products into terms of conditionals.

Finally, in other scenarios where $P(A\mid B)$ is given but $P(B\mid A)$ is not, this
is amenable by rewriting the numerator and denominator of conditional probability
using the product rule and sum rule:

$$
\begin{aligned}
P(A\mid B) &= \frac{P(A\cap B)}{P(B)}                         
           && \text{[by definition of conditional]}\\
           &= \frac{P(A)\,P(B\mid A)}{P(B)}                    
           && \text{[by product rule on joint]}\\
        %    &= \frac{P(A)\,P(B\mid A)}
        %           {P(B\cap A)+P(B\cap\neg A)}
        %    && \text{[by sum rule on marginal]}\\
        %    &= \frac{P(A)\,P(B\mid A)}
        %           {P(A)P(B|A)+P(\neg A)P(B|\neg A)}
        %    && \text{[by product rule]}
\end{aligned}
$$

$$
\underbrace{p(x \mid y)}_{\text{posterior}}
\;=\;
\frac{
    \overbrace{\,p(y \mid x)\,}^{\text{likelihood}}
    \;
    \overbrace{p(x)}^{\text{prior}}
}{
    \underbrace{\displaystyle\int p(y \mid x)\,p(x)\,dx}_{\text{evidence}}
}\;.
$$

and is referred to as **bayes' rule**. The scenario where
$P(A\mid B)$ is accessible but $P(B\mid A)$ is not comes up many times practice,
where $A$ is some **latent** event and $B$ is an **observable** event. This
happens in science (why bayes rule is also referred to as the *logic of science*)
where $P(A\mid B)$ is used to model **hypothesis** and **evidence** where the
**posterior** $P(H\mid E)$ (latent given observed) is updated by
evaluating the product of the **prior** $P(H)$ and the **likelihood**
(observed given latent) $P(E\mid H)$ normalized by the **evidence** $P(E)$.


##### Example 3: conditional probability
---
Let $\Omega$ be the sample space used from the previous examples,
$Y: \Omega \to \mathbb{R}$ once again be the r.v that maps the word
$\omega$ to the ordinal-based unicode number of the first letter, and let
$Z: \Omega \to \mathbb{R}$ be a new identity r.v $Z$ that maps the word
to it's hash. What's $P(Z=hash("pills")|Y=112)$? The answer by definition
of conditional probability is $\frac{0.19}{0.04+0.19}=0.83$, where
$P(Y=112)=0.04+0.19$ because of *additivity*.


todo: examples 4/5/6: product rule/sum rule/bayes rule


### Inference: Joint Random Variables

```python
from jax import numpy as jnp
from jaxtyping import Array, Float

def bayes(joint: Float[Array, "dx dy"]) -> Float[Array, "dx dy"]:
    prior = 0
    likelihood = 0
    evidence = 0

    posterior = 0
    return prior, likelihood, evidence, posterior
```

## Information Theory

### Kullback-Leibler Divergence

### Entropy

### Compression

### Algorithmic Information Theory

# A.3 Parameter Estimation
## Statistics
In probabilistic models distributions of latent hypotheses are usually unknown
and their posteriors are inferred with bayes rule by taking the product of the
prior $p(h)$, likelihood $p(e,h)$, and normalizing by the evidence $p(e)$. When
these latter distributions (more specifically, the parameters $\theta$ that
characterize them), the parameters need to be recovered from data via
**parameter estimation**.

### Empirical Risk Minimization (ERM)
Risk is a quantity where the objective is to minimize.
Reward is a quantity where the objective is to maximize.

loss functions and risk.
- theta_likelihood from L_likelihood (likelihood from the probabilistic perspective)
- theta_0-1 from L_01
- theta_absolute from L_abs

### Unbiased Estimators

### Maximum Likelihood Estimation (MLE)

Maximizing the likelihood uses the probability model itself as the loss function.
Following the notion of minimizing risk, the maximization corresponds to
minimizing the negative likelihood $-p(x; \theta)$. One more transformation
is applied by mapping the likelihood to log space which simplifies the
$\operatorname{argmin}$ routine by decomposing the joint probability of the data
$p(x_1, x_2, \ldots, x_n; \theta)$ into a product of likelihoods with the
*chain rule* (assuming $X_1, X_2, \ldots, X_N\overset{\text{iid}}{\sim}p$). The
parameters which minimize negative log likelihood are the same that minimize
negative likelihood since logarithms perserve **monotonicity**. All together,
the optimization problem is:

$$
\begin{aligned}
\hat{\boldsymbol{\theta}} &\in \underset{\boldsymbol{\theta} \in \Theta}{\operatorname{argmax}}\ p(x;\theta) \\
                          &= \operatorname{argmax}\ \log p(x_1,x_2, \ldots, x_n;\theta)
                          && \text{[by monotonicity of log]}\\
                          &= \operatorname{argmin}\ -\log p(x_1,x_2, \ldots, x_n;\theta) \\
                          &= \operatorname{argmin}\ -\log \prod_{i=1}^{n}p(x_i;\theta)
                          && \text{[by $X_i \overset{\text{iid}}{\sim}p$]} \\
                          &= \operatorname{argmin}\ - \sum_{i=1}^{n}\log p(x_i;\theta)
                          && \text{[by log laws]} \\
\end{aligned}
$$

In the following examples $\operatorname{argmin}$ is implemented symbolically by
solving for $\frac{d}{d\theta}=0$.

##### Example 1: maximizing likelihood with $Poi(\lambda)$
---
Assume $X_1, X_2, \ldots, X_n \overset{\text{iid}}{\sim} Poi(\lambda).$
Then using negative log likelihood as the loss function corresponds to the
following optimization:

$$
\begin{aligned}
\hat{\lambda}_{MLE} &\in \operatorname{argmin} \mathcal{L(\lambda)} \\
                          &= \operatorname{argmin} - \sum_{i=1}^{n} \log \frac{e^{-\lambda}\lambda^{x_i}}{x_i!} \\
                          &= \operatorname{argmin} - \sum_{i=1}^{n} -\lambda + x_i\log\lambda - \log x_i!
\end{aligned}
$$

and solving for the arguments $\lambda$ which minimize the expression corresponds
to taking the derivative with respect to $\lambda$ and solve for those that equal 0:

$$
\begin{aligned}
\frac{\partial \mathcal{L}}{\partial \lambda} &= 0 \\
    - \sum_{i=1}^{n} -1 + \frac{x_i}{\lambda} &= 0 \\
     n - \frac{1}{\lambda} \sum_{i=1}^{n} x_i &= 0 \\
               \frac{1}{n} \sum_{i=1}^{n} x_i &= \lambda_{MLE}

\end{aligned}
$$

##### Example 2: maximizing likelihood with $Ber(p)$
---
Assume $X_1, X_2, \ldots, X_n \overset{\text{iid}}{\sim} Ber(p).$
Then using negative log likelihood as the loss function corresponds to the
following optimization:

$$
\begin{aligned}
\hat{p}_{MLE} &\in \operatorname{argmin} \mathcal{L(p)} \\
                          &= \operatorname{argmin} - \sum_{i=1}^{n} \log[p^{x_i}(1-p)^{1-x_i}] \\
                          &= \operatorname{argmin} - \sum_{i=1}^{n} x_i\log p + (1-x_i)\log(1-p) \\
\end{aligned}
$$

and solving for the arguments $p$ which minimize the expression corresponds
to taking the derivative with respect to $p$ and solve for those that equal 0:

$$
\begin{aligned}
                                                  \frac{\partial \mathcal{L}}{\partial p} &= 0 \\
    \frac{\partial \mathcal{L}}{\partial p} - \sum_{i=1}^{n} x_i\log p + (1-x_i)\log(1-p) &= 0 \\
                    \frac{\partial \mathcal{L}}{\partial p} -[X\log p + (n - X)\log(1-p)] &= 0 && \text{[where $X = \sum_{i=1}^{n}x_i$]} \\
                                                           \frac{-X}{p} + \frac{n-X}{1-p} &= 0 \\
                                                                         -X + Xp + np -Xp &= 0 \\
                                                                         np &= X \\
                                                                          p_{MLE} &= \frac{1}{n}\sum_{i=1}^{n}x_i \\


\end{aligned}
$$



##### Example 3: maximizing likelihood with $Nor(n, \sigma^2)$
---
Assume $X_1, X_2, \ldots, X_n \overset{\text{iid}}{\sim} Nor(\mu, \sigma^2).$
Then using negative log likelihood as the loss function corresponds to the
following optimization:

$$
\begin{aligned}
\hat{\theta}_{MLE} &\in \operatorname{argmin} \mathcal{L(\theta)} && \text{[where $\theta=(\mu, \sigma^2)$]}\\
                          &= \operatorname{argmin} - \sum_{i=1}^{n} \log[p^{x_i}(1-p)^{1-x_i}] \\
\end{aligned}
$$

and solving for the arguments $p$ which minimize the expression corresponds
to taking the derivative with respect to $p$ and solve for those that equal 0:

$$
\begin{aligned}
                                                  \frac{\partial \mathcal{L}}{\partial p} &= 0 \\
    \frac{\partial \mathcal{L}}{\partial p} - \sum_{i=1}^{n} x_i\log p + (1-x_i)\log(1-p) &= 0 \\
                    \frac{\partial \mathcal{L}}{\partial p} -[X\log p + (n - X)\log(1-p)] &= 0 && \text{[where $X = \sum_{i=1}^{n}x_i$]} \\
                                                           \frac{-X}{p} + \frac{n-X}{1-p} &= 0 \\
                                                                         -X + Xp + np -Xp &= 0 \\
                                                                         np &= X \\
                                                                          p_{MLE} &= \frac{1}{n}\sum_{i=1}^{n}x_i \\


\end{aligned}
$$


### Maximum a Posterior Estimation (MAP)

## Matrix Calculus
Recall that for some scalar function $f: \mathbb{R} \to \mathbb{R}$ that the
derivative is usually defined as $\frac{df}{dx} = f'(x)$ where the **derivative**
$f'(x)$ is the **ratio** between the **differentials** (**inifinitesmal differences**)
of input $dx$ and output $df$. While $/: \mathbb{R} \to \mathbb{R}$ is defined
on $\mathbb{R}$, this is not always the case for higher dimensional euclidean
spaces.

Moreover, the problem with the formulation of derivatives as $\frac{df}{dx}$ is
not limited to denotational semantic lawyering — there's also a problem with the
derivative **rules** themselves. Consider the function
$f: \mathbb{R}^{n \times n} \to \mathbb{R}^{n \times n}$, $f(\mathbf{X}) := \mathbf{X}^2$
which squares a matrix. The power rule of scalar functions does not generalize here,
because $f'(x) \neq 2\mathbf{X}$.

Defining $f'(x)$ as a ratio $f'(x) = \frac{df}{dx}$ does not generalize. Instead, the
correct approach is to define the derivative as **linearization** which defines
the derivative as the linear operator you apply to a change in output to receive
a change in output. That is, $df = f'(x)dx$. This formulation of the derivative
generalizes **scalar** and **vector** differential calculus over
$\mathbb{R}$ and $\mathbb{R}^n$ into higher dimensional vector spaces for
**matrix** and **tensor** calculus over $\mathbb{R}^{n\times m}$ and
$\mathbb{R}^{d_0 \times d_1 \cdots \times d_n}$. In essence, linearization
is locally approximating complex surfaces in vector spaces $\mathcal{V}$
(more specifically, **Banach spaces** equipped with norm $||x||$) with
**linear operators** $L: \mathcal{V} \to \mathcal{V}$ ($L[v_1+v_2] = L[v_1] + L[v_2]$) so that $\Delta \text{out} = L[\Delta\text{in}]$.

### Derivatives, Gradients, and Jacobians
- for some function $f: \mathbb{R} \to \mathbb{R}$, $f'(x)$ is the term such that
$df = f'(x)[dx]$. For this expression to be defined,
$dx \in \mathbb{R} \land df \in \mathbb{R} \implies f'(x) \in \mathbb{R} \land [\bullet]:= *$ where $*: \mathbb{R} \to \mathbb{R}$.
- for some function $f: \mathbb{R}^n \to \mathbb{R}$, $f'(\mathbf{x})$ is the term such that
$df = f'(\mathbf{x})[d\mathbf{x}]$. For this expression to be defined,
$d\mathbf{x} \in \mathbb{R}^n \land df \in \mathbb{R} \implies f'(\mathbf{x}) \in \mathbb{R}^n \land [\bullet]:= \cdot$ where $\cdot: \mathbb{R}^n, \mathbb{R}^n \to \mathbb{R}$. In this case, $f'(\mathbf{x})$ is often referred to as the **gradient** of $f$ and denoted by $\nabla f$ 
- for some function $f: \mathbb{R}^n \to \mathbb{R}^m$, $f'(\mathbf{x})$ is the term such that
$d\mathbf{f} = f'(\mathbf{x})[d\mathbf{x}]$. For this expression to be defined,
$d\mathbf{x} \in \mathbb{R}^n \land d\mathbf{f} \in \mathbb{R}^m \implies f'(\mathbf{x}) \in \mathbb{R}^{m\times n} \land [\bullet]:= @$ where $@: \mathbb{R}^n \to \mathbb{R}^m$. In this case, $f'(\mathbf{x})$ is often referred to as the **jacobian** of $f$ and denoted by $Jf$ 

numerical example.

### Automatic Differentiation
as opposed to symbolic or numerical.

## Optimization

### Gradient Descent

### Stochastic Gradient Descent

### Adam

### Shampoo

### Muon



and more generally we define the derivative *as* the linear operator L on vector
space V which you apply to a change in input in order to obtain the change in
output. That is, Δout = (lin.op)[Δin]. which generalizes to higher dimensions.
While defining f'x ≜ df/dx is legal when f: ℝ -> ℝ, it's usually not clear what
df/dx means in higher dimensions, since /: ℝ^n, R^m -> ? is not defined.

f: ℝ^d -> ℝ
-----------
Consider f'(x) when f: ℝ^d -> ℝ. Then, given that dx ∈ ℝ and df ∈ ℝ^d, the linear
operator f'(x) *must* (formalized by the Riecz-Frchét Representation Theorem) be
the dot product with some column vector, (multiplication with the column vector
transposed). Let's call this column vector the "gradient" of f, where the
component-wise definition with indices is:
```
    ∇f ∈ ℝ^d, f'(x) = ∇fᵀx
    ∇f_i ≜ ∂f/∂x_i ∀ i ∈ [0..d]
and all together, we use f'(x) to linearize df:
    f(x+dx)-f(x) = df
                 = f'(x)dx
                 = ∇fᵀdx
```

f: ℝ^m -> ℝ^n
-------------
Moving on, consider f'(x) where f: ℝ^m -> ℝ^n. Then given that dx ∈ ℝ^m and
df ∈ ℝ^n, the linear operator f'(x) *must* (formalized by the Riecz-Frchét
Representation Theorem once again) expressable as a matrix multiplication with
some matrix A ∈ ℝ^(mxn). Let's call this matrix the "jacobian", where the
component-wise definition with indices is the following:
```
    Jf ∈ ℝ^(nxm), f'(x) = Jdx
    Jf_ij ≜ ∂fi/∂xj ∀ i ∈ [0..n], j ∈ [0..m]
and all together, we use f'(x) to linearize df:
    f(x+dx)-f(x) = df
                 = f'(x)dx
                 = Jdx
```

f: ℝ^m, ℝ^n -> ℝ^p [f: ℝ^(nxm), ℝ^(mxp) -> ℝ^(nxp)]
---------------------------------------------------
For example, given f: ℝ^m -> ℝ^n, f(x) := Ax
```
==> df = f(x+dx)-f(x)
       = (Ax+dx)-Ax
       = Adx
==> f'(x) = A = J
```

While it's very elegant that the derivative of Ax turns out to be A, training
neural networks requires that is A is not just constant, but variable with
respect to f. To make the deep learning context clearer, we will use W instead
of A:
```
  fi(W,x) ≜ ΣWij*xj
    f(x) := [f1(x), f2(x), ..., fn(x)]
but we can just promote x to be a matrix too. Finally, consider f'(x) when
f: ℝ^(nxm), ℝ^(mxp) -> ℝ^(nxp)

ℒ: ℝ^d0 -> ℝ ∋ ℒ(Z) where Z := X @ Y
==> dℒ = dℒ/dZ : dZ                             [by Riecz. Representation Theorem]
       = dℒ/dZ : (dX@Y + X@dY)                  [by product rule on dZ]
       = dℒ/dZ : (dX@Y) + dℒ/dZ : (X@dY)        [: linear]
       = (dℒ/dZ@Yᵀ) : dX + (Xᵀ@dℒ/dZ) : dY      [by TODO...circularity of trace]                              
==> dℒ/dX = dℒ/dZ@Yᵀ ∧ dℒ/dY = Xᵀ@dℒ/dZ
```






Stepping back, defining the derivative this way generalizes the rise over run with
scalars, the gradient with vectors, and the jacobian with matrices into the same
notion: they are the linear operators defined on vector spaces that you apply to
a change in input in order to obtain the change in output, and the types/shapes
are defined the way they are out of necessity, rather than by fiat. Moreover,
this also sharpens the semantics of the denotation in higher dimensions given
that df/dx is often ill-defined. i.e. while df/dx makes sense when df, dx ∈ ℝ^n,
dividing tensors with mismatched shapes becomes nonsensical.

This also means that this definition extends to any vector space where a linear
operator is defined, for instance, the space of matrices (vectorization of
matrices), and the space of functions (calculus of variations).

While training deep neural networks do not involve functions that map
matrices to matrices or functions to functions, defined this way it's clear
that many of the "grads" we take in pytorch are actually jacobians that all
compose into one single gradient for the entire expression graph which represents
some function f: ℝ^n -> ℝ.

2b — Derivative rules: backward methods
---------------------------------------
defining rules for any f: V->V

```
Sum rule
--------
f: V -> V
f(x) := g(x) + h(x)
==>      df = dg + dh
    f'(x)dx = g'(x)dx + h'(x)dx   [by def]
      f'(x) = g'(x) + h'(x)       [/ dx]

Product rule
------------
f: V -> V
f(x) := g(x)h(x)
==> df = f(x+dx)-f(x)
       = g(x+dx)h(x+dx)-g(x)h(x)                        [by def of f]
       = [g(x)+g'(x)dx][h(x)+h'(x)dx]-g(x)h(x)          [by def of i(x+dx)=i(x)+i'(x)]
       = g(x)h(x)                                       [expand product]
         + g(x)h'(x)dx
         + h(x)g'(x)dx
         + g'(x)h'(x)(dx)^2
       - g(x)h(x)
       = g(x)h'(x)dx + h(x)g'(x)dx + g'(x)h'(x)(dx)^2   [g(x)h(x) cancels]
       = g(x)h'(x)dx + h(x)g'(x)dx                      [g'(x)h'(x)(dx)^2 -> 0]
       = [g(x)h'(x) + h(x)g'(x)]dx
       = f'(x)dx

Chain rule
----------
f: V -> V
f(x) := g[h(x)]
==> df = f(x+dx)-f(x)
       = g[h(x+dx)]-g[h(x)]
       = g[h(x)+h'(x)dx]-g[h(x)]                  [by def of h(x+dx)=h(x)+h'(x)]
       = g'[h(x)]h'(x)dx                          [g(x+dx)-g(x) = dg = g'(x)dx
                                                   where x=h'(x)dx]
```

2c — Automatic differentiation: calculus on computational graph
---------------------------------------------------------------
Recall that we need to evaluate the gradient of a neural network's loss
function in order to train it with parameter estimation. The method used to
compute derivatives is automatic differentiation — an algorithmic technique as
opposed to symbolic techniques that are inefficient or numeric ones that are
inaccurate. Given some expression graph that represents a neural network
f: ℝ^d_0 -> ℝ, autodiff will recursively apply the chain rule for each
subexpression h: ℝ^d_i -> ℝ^d_j.

```
c = a*b (1)
d = e+f (2)
g = c*d (3)

 .-.
( a <---+
 `-'    |   * .-. (1)
 .-.    +----( c <---+
( b )<--+     `-'    |
 `-'                 |    * .-. (3)
 .-.                 +-----( g )
( e )<--+   / .-. (2)|      `-'
 `-'    +----( d )<--+
 .-.    |     `-'
( f )<--+
 `-'
Fig 4. expression graph
```

Notice in figure 4 that even though evaluation of the expression is denoted from
left to right, the edges of the graph form a directed acyclic graph (tree) rooted
at the output of the expression which simplifies graph modification.

Interpreting each step of computation (1, 2, 3) is referred to as the
"forward pass" and constructs the graph in memory which either creates a new tree
amongst a forest (2) or roots the forest into a single tree (3).

Evaluating the derivative of the output with respect to all inputs is referred
to as the "backward pass" — as opposed to "second forward pass" because deep
learning frameworks implement reverse-mode differentiation as opposed to
forward-mode.

The difference between forward-mode and reverse-mode differentiation is the
direction you step through the graph which can end up influencing speed depending
on the shape of the function. Stepping from inputs to outputs is considered a
forward traversal, and the opposite is considered to be a backward traversal.

For instance, consider the applicaiton of the chain rule when evaluating the
f'(x) for f(x) := cos(x^2). Which subexpression do you derive first? Evaluating
d/dx[cos(x)] = sinx first is reverse mode, and evaluating d/dx[x^2] = 2x first
is forward mode. With neural networks, most expression graphs represent functions
of the form f: ℝ^n -> ℝ, and so the default traversal for all deep learning
frameworks is reverse mode.

However, regardless of the order of traversal, the effective computation is the
same: a summation over all possible products of paths. The base case starts
with ∂f/∂f = 1, propagating the local dout/din, and recursively applies the chain
rule for each subexpression. This is why reverse-mode differentiation is also
referred to as "backpropagation". Stepping through the backward for the same
function f as above where d denotes the global derivative and ∂ denotes the local
derivative (overloading ∂'s classic usage for partial derivative)

```
---forward
c = a*b (1)
d = e+f (2)
g = c*d (3)

---backward
∂g/∂g = 1 [base case] (4)

dg/dc = ∂g/∂g*∂g/∂c           [chain rule]
      = ∂g/∂g*d               [product rule]
      = d                     [∂g/∂g cached] (5)

dg/dd = ∂g/∂g*∂g/∂d           [chain rule]
      = ∂g/∂g*c               [product rule]
      = c                     [∂g/∂g cached] (6)

dg/da = ∂g/∂g*∂g/∂c*∂c/∂a     [chain rule]
      = ∂g/∂g*∂g/∂c*b         [product rule]
      = d*b                   [∂g/∂g*∂g/∂c cached] (7)

dg/db = ∂g/∂g*∂g/∂c*∂c/∂b     [chain rule]
      = ∂g/∂g*∂g/∂c*a         [product rule]
      = d*a                   [∂g/∂g*∂g/∂c cached] (8)

dg/de = ∂g/∂g*∂g/∂d*∂d/∂e     [chain rule]
      = ∂g/∂g*∂g/∂d*1         [sum rule]
      = c                     [∂g/∂g*∂g/∂d cached] (9)

dg/df = ∂g/∂g*∂g/∂d*∂d/∂f     [chain rule]
      = ∂g/∂g*∂g/∂d*1         [sum rule]
      = c                     [∂g/∂g*∂g/∂d cached] (10)

 .-. (7)
( a <---+
 `-'    |   * .-. (1) (5)
 .-.    +----( c <---+
( b )<--+     `-'    |
 `-' (8)             |    * .-. (4)
 .-. (9)             +-----( g )
( e )<--+   + .-. (2)|(6)   `-'
 `-'    +----( d )<--+
 .-.    |     `-'
( f )<--+
 `-'(10)
 ```

What's delightful from a user experience point of view is that autodifferentiation
allows the user to implicitly construct the expression graph by specifying the
forward pass, and the backward pass is abstracted away with an output.backward().

TODO: show expression graph for a neural network.
TODO: let's now revisit the definition of the derivative (2b) and rederive a few derivative rules (2c)
- +(tensor, tensor)
- @(tensor, tensor)
- tanh(tensor)


2d — Advanced AD: jacobian-vector and vector-jacobian products
--------------------------------------------------------------

2e — Gradient descent: non-convex optimization
----------------------------------------------
Finally, recall evaluating the derivative of the computational graph's output
with respect to all inputs is used to implement argmin ℒ(Θ) by iteratively
adjusting θ̂ via gradient descent:
    θ^(t+1) := θ^t - α∇ℒ(Θ)
             = θ^t - α∇-ΣlogP(y^i|x^i;Θ)

# 1. Generation

**Contents**
- [1.1 Autoregressive Models]()
    - [Recurrent Neural Networks (RNNs)]()
    - [Long Short-Term Memory Networks (LSTMs)]()
    - [Generative Pretrained Transformers (GPTs) with dense qwen/llama]()
    - [Mixture of Experts (MoEs) with moe deepseek/olmoe]()
<!-- - [3.2 Diffusion Models]()
https://x.com/DrYangSong/status/1891314153580556468
https://x.com/dpkingma/status/1863662926101434594
https://x.com/sirbayes/status/1773771737127428236
https://diffusion.csail.mit.edu/
- [3.3 Variational Autocenters]()
- [3.4 Normalizing Flow Models]()
- [3.5 Generative Adversarial Networks]()
- [3.3 Energy Based Models]() -->

# 1.1 Autoregressive Models

## Feedforward Neural Networks (FNNs)

Autoregressive language models.
logistic regression -> FFN -> RNN -> LSTM -> GPT

```python
"""
model: Neural Language Models (Bengio et al. 2003)

Dimension key:
# windows
B: batch size
T: sequence length

# input/output
V: vocabulary size
E: embedding dimension (E != D in paper)
D: model dimension
"""
import picograd
# import torch
import matplotlib.pyplot as plt
# picograd = torch
# %matplotlib inline
# from jaxtyping import

# *********************MODEL*********************
B, T = 32, 3
V, E, D = 27, 10, 200

class Linear:
  def __init__(self, D_in, D_out, bias=True):
    self.W_DiDo = picograd.randn((D_in, D_out)) * 0.01
    self.b_Do = picograd.zeros(D_out) if bias else None

  def __call__(self, X_Di):
    self.X_Do = X_Di @ self.W_DiDo
    if self.b_Do is not None: self.X_Do += self.b_Do
    self.out = self.X_Do
    return self.X_Do

  def parameters(self):
    return [self.W_DiDo] + ([] if self.b_Do is None else [self.b_Do])

class Tanh:
  def __call__(self, X_BD):
    self.X_BD = picograd.tanh(X_BD)
    self.out = self.X_BD
    return self.X_BD
  
  def parameters(self):
    return []

model = [
  Linear(T * E, D, bias=False), Tanh(),
  Linear(D, D, bias=False), Tanh(),
  Linear(D, V, bias=False)
]

C_VE = picograd.randn((V,E)) #, generator=g)
params = [C_VE] + [p for l in model for p in l.parameters()]
for p in params:
    p.requires_grad = True

print("model loaded to cpu")

# *********************DATA*********************
import picograd.nn.functional as F
import random

words = open('./tests/names.txt', 'r').read().splitlines()
v = sorted(list(set(''.join(words))))
encode = { c:i+1 for i,c in enumerate(v) }
encode['.'] = 0
decode = { i:c for c,i in encode.items() }

def gen_dataset(words):
  X, Y = [], []
  for w in words[:10]:
    context = [0] * T;
    for c in w + '.':
      X.append(context)
      Y.append(encode[c])
      # print(''.join(decode[i] for i in context), '-->', decode[encode[c]])
      context = context[1:] + [encode[c]]

  X, Y = picograd.tensor(X), picograd.tensor(Y) # X:(N,C) Y:(N)
  return X, Y

random.seed(42)
random.shuffle(words)
n1, n2 = int(0.8*len(words)), int(0.9*len(words))
X_NT, Y_N = gen_dataset(words)#[:n1])
print(X_NT.shape, Y_N.shape)
Xdev, Ydev = gen_dataset(words[n1:n2])
Xte, Yte = gen_dataset(words[n2:])

# *********************TRAINING LOOP*********************
# 1. preprocess
# 2. forward
# 3. backward
# 4. update

losses, steps = [], []
for step in range(100): #200000):
  # 1. preprocess
  i_B = picograd.randint(0, X_NT.shape[0], (B,)) # a. minibatch: X_NT -> X_BT
  X_BT, Y_B = X_NT[i_B], Y_N[i_B]
  X_BTE = C_VE[X_BT] # b. embed: X_BT -> X_BTE where t ∈ [0,V]
  X = X_BTE.reshape((-1, T * E)) # concat so X=X_B(TE) # TODO: can take .view instead

  # 2. forward
  for h in model:
    X = h(X) # X_B(TE) -> X_BD -> X_BV
  yhat_BV = X
  loss = F.cross_entropy(yhat_BV, Y_B)

  # 3. backward
  for layer in model:
    layer.out.retain_grad() # 6 .retain_grad()
  for p in params:
    p.grad = None
  loss.backward()

  # 4. update
  for p in params:
    p.data += -0.01 * p.grad

  (_, _) = (steps.append(step), losses.append(loss.log10().item()))
  if step % 10000 == 0: print(f"step: {step}/{200000}, loss {loss.item()}")

plt.plot(steps, losses)

# *********************INFERENCE LOOP*********************
for _ in range(20): # n samples
  output, context = [], [0] * T
  while True:
    # 1. preprocessing: C_VE[X_1T]
    # print("tokens", context)
    X_1T = picograd.tensor([context]) # B=1 for inference (1 response)
    X_1TE = C_VE[X_1T] # X_1T ∈ [0..=26] must hold
    X_1cTE = X_1TE.reshape((-1, T*E)) # reshape from 3dims -> 2dims B=1 TE
    X = X_1cTE

    # 2. process: f: ℝ^d -> [0,1]^k
    for h in model:
      X = h(X)
    y_hat = F.softmax(X, dim=1)

    # 3. postprocess: sample + update
    token = picograd.multinomial(y_hat, num_samples=1, replacement=True).item()#, generator=g).item()
    output.append(decode[token])
    context = context[1:] + [token]
    if token == 0:
        break
  print(''.join(output))
```

## Recurrent Neural Networks (RNNs)

## Long Short-Term Memory Networks (LSTMs)

## Generative Pretrained Transformers (GPTs)

- gpt2
- gpt3

## Generative Pretrained Transformers (GPTs++)

- 1. manning ebook-2.torchtitan/torchtune-3.flexattention: llama1, llama2, llama3, llama4
- modded nanogpt by keller. based on llm.c by karpathy.
- beyondnanogpt
