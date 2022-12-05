---
marp: true
theme: uncover-bb
paginate: true
---

<!-- _class: lead -->

<center>

![bg right](img/dalle_lady.png)

# Trustworthy AI: <br> Fairness, Interpretability <br> and Privacy 

## Lecture 4 - Preprocessing Methods in Fairness



<div class="footnote">

 Image generated by OpenAI dall-e
 *Prompt:* "a trustworthy robot helping an old lady cross a busy street, realistic"

</div> 

</center>


---

## Independence


The random variables $(A, \hat{Y})$ satisfy independence if $\hat{Y} \bot A$. 

If $\hat{Y}$ is binary, then independence simplifies to the following condition:

<br>

$$\mathbb{P}(\hat{Y} = 1 \mid A = a) = \mathbb{P}(\hat{Y} = 1 \mid A = b)$$

If $A$ may take onto more values than $\{a, b\}$, then the condition must be satisfied for all group pairs, e.g. $(a, b), (a, c), (b, c)$.





---


## Independence

We may also relax independence as follows:

<br>

$$\mathbb{P}(\hat{Y}=1 \mid A = a) \geq \mathbb{P}(\hat{Y}=1 \mid A = b) - \epsilon$$

Or rewrite it as a ratio:

$$\frac{\mathbb{P}(\hat{Y}=1 \mid A = a)}{\mathbb{P}(\hat{Y}=1 \mid A = b)} \geq 1 - \epsilon$$

Where in both cases $\epsilon \geq 0$. 

---


## Independence

* Independence may reflect the belief that **all groups have an equal claim to acceptance**. 
* It has been investigated rather deeply in the ML literature.
* Note that independence does not require taking $Y$ into account at all. This may be OK if you believe you have systematic measurement bias in your $Y$ - for example, in hiring applications. 




---


## Independence

There is a bigger issue with independence - imagine a ill-intentioned decision maker which hires people from group $a$ with a rigorous process and some probability $p > 0$. 

People from group $b$ are hired randomly at the same rate $p$. 

This methodology perfectly satisfies independence. However, consider the ML loop...


---


## Independence

<br>

<center>

![](img/ml-loop.png)

</center>

<br>

Actions have consequences on the state of the world and therefore on the data -- if the company starts a data-based hiring process after gathering this kind of data, it will see that employees of group $b$ are performing badly. 


---

## Independence

>Using a dataset of all CEO transitions in Fortune 500 companies over a 15-year period, we analyze mechanisms that shape the promotion probabilities and leadership tenure of women and racial/ethnic minority CEOs. Consistent with the theory of the glass cliff, we find that **occupational minorities**—defined as white women and men and women of color—**are more likely than white men to be promoted CEO of weakly performing firms**. Though we find no significant differences in tenure length between occupational minorities and white men, we find that when firm performance declines during the tenure of occupational minority CEOs, these leaders are likely to be replaced by white men. We term this phenomenon the “savior effect.”

Cook and Glass. "*Above the glass ceiling: When are women and racial/ethnic minorities promoted to CEO?*". Strategic Management Journal 35, 2014.


---

## Separation

As previously discussed, independence does not consider $Y$ at all.

Nonetheless, if we believe that our ground truth $Y$ was collected carefully and does not contain much bias, it may represent a **partitioning of the population** into groups of **equal claim to acceptance**.

Of course some demographic group $A = a$ may be less well-represented in the label group $Y = 1$. One could argue that in these cases it is justified to accept fewer individuals from group $a$.  

---

## Separation

Random variables $(\hat{Y}, A, Y)$ satisfy **separation** if $\hat{Y} \bot A \mid Y$.

In the binary classification and binary $A$ case:

$$
\mathbb{P}(\hat{Y} = 1 \mid Y = 1, A = a) = \mathbb{P}(\hat{Y} = 1 \mid Y = 1, A = b)
\mathbb{P}(\hat{Y} = 1 \mid Y = 0, A = a) = \mathbb{P}(\hat{Y} = 1 \mid Y = 0, A = b)
$$

**Intuitively**: once we pick a value for $Y$, the value of $A$ does not matter for the classifier.

---

## Separation

$$
\mathbb{P}(\hat{Y} = 1 \mid Y = 1, A = a) = \mathbb{P}(\hat{Y} = 1 \mid Y = 1, A = b) \\
\mathbb{P}(\hat{Y} = 1 \mid Y = 0, A = a) = \mathbb{P}(\hat{Y} = 1 \mid Y = 0, A = b)
$$

<br>

Notice that $\mathbb{P}(\hat{Y} = 1 \mid Y = 1)$ is the **true positive rate**.

Remember that $1-$ true positive rate $=$ **false negative rate**.

Notice that $\mathbb{P}(\hat{Y} = 1 \mid Y = 0)$ is the **false positive rate**. 

Thus, separation requires that all groups experience **the same false negative rate** and **the same false positive rate**.

Separation may be understood as **error rate parity** across groups.

---

## Sufficiency

Random variables $(\hat{Y}, A, Y)$ satisfy **sufficiency** if $Y \bot A \mid \hat{Y}$.

In the binary classification and binary $A$ case:

$$
\mathbb{P}(Y = 1 \mid \hat{Y} = 1, A = a) = \mathbb{P}(Y = 1 \mid \hat{Y} = 1, A = b) \\
\mathbb{P}(Y = 1 \mid \hat{Y} = 0, A = a) = \mathbb{P}(Y = 1 \mid \hat{Y} = 0, A = b)
$$

**Intuitively**: parity of predicted positive/negative values across groups. 

---


## Incompatibility Theorems

These criteria constrain the joint distribution of $(Y, A, \hat{Y})$ in non-trivial ways. There are several **incompatibility theorems** that show us how these criteria cannot be satisfied simultaneously by any classifier $f$.





---


## Independence vs. Sufficiency

Independence: $A \bot \hat{Y}$.

Sufficiency: $Y \bot A \mid \hat{Y}$.

**Theorem 1**. Assume $A$ and $Y$ are not independent. Then, sufficiency and independence cannot both hold. 

**Proof** by contradiction. $A \bot \hat{Y}  \; \wedge \; A \bot Y \mid \hat{Y} \to A \bot (Y, A) \to A \bot Y$



---


## Independence vs. Separation.

Independence: $A \bot \hat{Y}$.

Separation: $\hat{Y} \bot A \mid Y$.

**Theorem 2**. Assume that $Y$ is binary, $A$ is not independent of $Y$ and $\hat{Y}$ is not independent of Y. Then, independence and separation cannot both hold.




---


## Independence vs. Separation

**Theorem 2**. Assume that $Y$ is binary, $A$ is not independent of $Y$ and $\hat{Y}$ is not independent of Y. Then, independence and separation cannot both hold.

**Proof.** By the law of total probability, 
$\mathbb{P}(\hat{Y} = \hat{y} \mid A = a) = \sum_y \mathbb{P}(\hat{Y} = \hat{y} \mid A = a, Y = y) \mathbb{P}(Y = y \mid A = a)$

Since we assume that $A \bot \hat{Y}$ and that $A \bot \hat{Y} \mid Y$, we may simplify the above to 

$\mathbb{P}(\hat{Y} = \hat{y}) = \sum_y \mathbb{P}(\hat{Y} = \hat{y} \mid Y = y) \mathbb{P}(Y = y \mid A = a)$




---


## Independence vs. Separation


However, we can use total probability again to decompose $\mathbb{P}(\hat{Y} = \hat{y})$ as follows:

$\mathbb{P}(\hat{Y} = \hat{y}) = \sum_y \mathbb{P}(\hat{Y} = \hat{y} \mid Y = y) \mathbb{P}(Y = y)$

It follows that the last two derivations must equal one another:

$\sum_y \mathbb{P}(\hat{Y} = \hat{y} \mid Y = y) \mathbb{P}(Y = y \mid A = a) = \sum_y \mathbb{P}(\hat{Y} = \hat{y} \mid Y = y) \mathbb{P}(Y = y)$

Let us rewrite this equation in a more compact way.

$p \coloneqq \mathbb{P}(Y = 0); \; p_a \coloneqq \mathbb{P}(Y = 0 \mid A = a); \; r_y = \mathbb{P}(\hat{Y} = \hat{y} \mid Y = y)$



---


## Independence vs. Separation

Then,

$p r_0 + (1-p) r_1 = p_a r_0 + (1 - p_a) r_1$

which we may rewrite as

$p(r_0 - r_1) = p_a(r_0 - r_1)$

This equation is satisfied only if $r_0 = r_1$, which implies $\hat{Y} \bot Y$; or if $p = p_a$ for all $a$, which implies $Y \bot A$. 






---


## Separation vs. Sufficiency

Sufficiency: $Y \bot A \mid \hat{Y}$.

Separation: $\hat{Y} \bot A \mid Y$.

**Theorem 3**. Assume that all events in the joint distribution of $A, \hat{Y}, Y$ have non-zero probability. Also assume that $A \not \perp Y$. Then, separation and sufficiency cannot hold.

**Proof.** $A \bot \hat{Y} \mid Y \wedge A \bot Y \mid \hat{Y} \to A \bot (\hat{Y}, Y)$. Then, $A \bot \hat{Y} \wedge A \bot Y$.

---



## Fairness Interventions

If one is concerned about unbalanced/non-egalitarian decision making, one possible action is to employ **fairness interventions**

<br>

This is a catch-all term for various techniques which impact the level of independence, separation or sufficiency of a classifier - sometimes more than one at once.

<br>

As discussed previously, this is a **last resort approach** and no substitute to understand the data generation process.


---

## Fairness Interventions


<center>

![w:700px](img/ml-loop.png)

</center>

Broadly, three classes of interventions:

* **Pre-processing**: changing the **data**, $X$ or $Y$ or both.
* **In-processing**: changing the **learning** process $f_{\theta}$.
* **Post-processing**: changing the **actions** $\hat{Y}$.

---

## Fairness Interventions

There is no current consensus on which class of intervention one should apply depending on the situation. However, it is possible to mention some strengths and weaknesses of each family of methods. 

**Pre-processing methods** are overall quite **interpretable**. After changing the data in some way, it is still possible to investigate and try to make sense of it.

On the other hand, it is hard to say if it will be **impossible to discriminate** after employing these techniques.

Furthermore, pre-processing entails **not having access to the decisions $\hat{Y}$**, which means they can be limited to indepdendence definitions.


---

## Fairness Interventions

<br>

**In-processing methods** are often more **accurate**, as one explores the tradeoff of fairness vs. accuracy during the training process itself. 

<br>

On the other hand they are **only as interpretable as the model** they are based on. 


---

## Fairness Interventions

<br>

Some **post-processing methods** are **optimal** in the sense that they achieve the best fairness-accuracy tradeoff under some assumptions.

<br>

However, they often **require to draw different thresholds for different groups**, which means that the sensitive attribute needs to be available at test time.

---

## Preprocessing methods

Preprocessing methods may be further divided into two families:

* **Data-level preprocessing**: modify $x$, $y$ or both. 
<br>
* **Representation-level preprocessing**: a new representation $\hat{x}$ of $x$ is found so that sensitive information $A$ is removed.


---

## Interventions Taxonomy (1)

<center>

![](img/interventions.png)

</center>


---

## Interventions Taxonomy

Today: **suppression**, **massaging** and **reweighting**, as developed by Kamiran and Calders in *Data preprocessing techniques for classification without discrimination*, 2012. 

<br>




---

## Suppression

**Core idea**: finding those features in $X$ which correlate the most with the sensitive information $A$. 

Multiple measures of correlation between one feature $X_i$ and $S$ may be applicable here.

---

## Suppression

**Pearson correlation coefficient**: $\rho_{X_i, A} = \frac{cov(X_i, A)}{\sigma_{X_i} \sigma_{A}} = \frac{\mathbb{E}[(X_i - \mu_{X_i})(A - \mu_A)]}{\sigma_{X_i} \sigma_A}$

Also informally known as **linear correlation**. It is merely a normalization of the covariance between two random variables. 

* Easy to compute
* Possible to perform statistical testing for both zero and nonzero values
* Only considers **linear** correlation - more complex relationships may be missed. 

---

## Suppression

<br>

<center>

![w:800px](img/correlation.svg)

</center>

---

## Suppression

A more general alternative: the **mutual information** $I(X_i; A)$.

Originally developed by Claude Shannon and given its name by Roberto Fano, is a more general measure of correlation between two random variables. Its attractiveness is due to the fact that it is 0 **if and only if** two random variables are independent. 

It follows that one may also model independence as $I(\hat{Y}; A) = 0$ and separation as $I(\hat{Y}; A \mid Y) = 0$.


---

## Suppression

If $X_i$ and $A$ are discrete: $I(X_i; A) = \sum_{x_i \in X_i} \sum_{a \in A} \mathbb{P}(x_i, a) \; log( \frac{P(x_i, a)}{P(x_i)P(a))})$

<br>

In the continuous case, one can just substitute the sums with integrals. 

* General measure of correlation.
* **Hard to compute**. Estimating the joint probability $\mathbb{P}(x_i, a)$ can be extremely challenging. It is possible to simplify the computation somewhat via Bayes' theorem.


---

## Massaging

**Short summary**: changing the value of $y$ for some examples so to obtain independence or low values of the related metric.

**Assumptions**: a binary classification setting, binary sensitive attribute $A$, one privileged group $A=a$ and one underprivileged group $A=b$.

---

## Massaging

1. Train a ranker on the dataset $X, Y$ without taking into consideration $A$. A ranker is a function $r_{\theta}(x) \to [0, 1]$ which outputs the probability that $x$ is the most relevant (or most deserving) example. 
2. Create the **promotion list** by selecting all samples in which $A=b \wedge Y=0$ and rank them **descending** (from highest to lowest) with regard to the scores outputted by $r_{\theta}(x)$. 
3. Create the **demotion list** by selecting all samples in which $A=a \wedge Y=1$ and rank them **ascending** (from lowest to highest) with regard to the scores outputted by $r_{\theta}(x)$.
4. Select $m$ individuals from both lists. Change their label values.

---

## Massaging

* Quite an **intrusive** method, as it requires rewriting the data. 
<br>
* Different rankers $r_{\theta}$ will return different top-1 probabilities. This will in turn modify the **final result**.
<br>
* To be considered in the event that one is concerned about having a biased ground truth measurement process.

---

## Weighting