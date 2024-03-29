---
marp: true
theme: uncover-bb
paginate: true
---



<!-- _class: lead -->

<center>

![bg right](img/dalle_lady.png)

# Trustworthy AI: <br> Fairness, Interpretability <br> and Privacy 

## Lecture 3 - Fairness in Classification



<div class="footnote">

 Image generated by OpenAI dall-e
 *Prompt:* "a trustworthy robot helping an old lady cross a busy street, realistic"

</div> 

</center>

<!--

Open points:
* broad and narrow view of equal opportunity, chapter 4 of fair ml book
* rawls and veil of ignorance: unclear how to discuss it. see https://www.jpe.ox.ac.uk/papers/indirect-discrimination-is-not-necessarily-unjust/





---


## Outline

* Recap: success stories in AI, what is classification, the measurement problem
* Can unawareness be the solution?
* Grounding our intuition of fairness/discrimination/bias in philosophy
* Grounding our intuition of fairness/discrimination/bias in computer science: Friedman and Nisselbaum
* Back to the ML loop: chances for amplification of discrimination
* Fairness definitions - starting from unawareness and why it does not work
* A few words about causality
* Fairness interventions: data level, model level, decision level






---


## Outline

* equality of opportunity and equality of outcome
* fairness by unawareness: procedural justice



---


## Outline

* compounding unfairness: the poverty loop and ML (see O'Neil in stoyanovich slides)
* parity-based fairness metrics
* why I did not just give the metrics?
* fairness interventions - the data is what it is. the world is what it is. 
* however, interventions are a "all is lost!" approach 
* data level, model level, decision level

-->

---

## A few ML success stories

<br>

<br>

<center>

![bg left:64% w:800](img/beauty.png)

</center>

A "beauty pageant algorithm"

---

## A few ML success stories

<br>

<br>

<center>

![bg left:64% w:800](img/guardian_small.png)

</center>

Guardian, mostly white winners in the AI Beauty Pageant

---

## A few ML success stories 

Reuters: Amazon scraps secret AI recruiting tool that showed bias against women

![bg left w:550px](img/amazon_tool.png)


---

## A few ML success stories 

>“Everyone wanted this holy grail,” one of the people said. “They literally wanted it to be an engine where I’m going to give you 100 resumes, it will spit out the top five, and we’ll hire those.”

![bg left w:550px](img/amazon_tool.png)


---

## A few ML success stories 

>[...] Amazon’s computer models were trained to vet applicants by observing patterns in resumes submitted to the company over a 10-year period. Most came from men, a reflection of male dominance across the tech industry.

![bg left w:550px](img/amazon_tool.png)


---


## A few ML success stories 

>[...] It penalized resumes that included the word “women’s,” as in “women’s chess club captain.” And it downgraded graduates of two all-women’s colleges, according to people familiar with the matter. 

![bg left w:550px](img/amazon_tool.png)

---



## COMPAS


<center>

![w:600px](img/bias1.png)

</center>

---

## COMPAS


<center>

![w:600px](img/bias2.png)

</center>

---

## COMPAS


<center>

![w:600px](img/compas.png)

</center>

---


## How does this happen

Models/AI systems will always make **mistakes**, but the **distribution of those mistakes** may be affecting different groups of people in a disparate way 

<br>

As a perfect model is usually not available, the study of **fairness** in machine learning seeks to avoid situations in which certain groups are discriminated against

---

## Classification - A Primer


$X$: **data**, covariates
$Y$: ground truth, labels, **target variable**

Classification is the process of determining a plausible value for $Y$ given $X$.

More technically, one seeks to learn the parameters $\theta$ of some function $f_{\theta}$ which maps the **random variable** $X$ to an estimate $\hat{Y}$ of $Y$:

<br>

$$\hat{Y} = f_{\theta}(X)$$


---

## Classification - A Primer



<center>

![](img/hiring-1.png)

</center>

$X$: qualification score
$Y$: the person received a positive evaluation from their manager after 12 months of employment (+) or not (-) 



---

## The ML loop

<br>
<br>

<center>

![w:700px](img/ml-loop.png)


</center>

<br>

**Measurement**: the process in which the state of the world is reduced to data tables. 


---

## Measurement

<br>
<br>


>The “pro-male evaluation bias”, as some call it, generally leads women being rated lower in performance than men, even in situations where their performance has been demonstrably equal to the men’s. One study of the quality of writing in essays (researchers “varying” the sex of the author) showed similar bias, with both male and female readers rating the essays as being of lower quality if they believed the author was female.

Snipes, Thomson and Oswald. *Gender bias in customer evaluations of service quality*. Journal of Services Marketing, 2006. 



---


## Discrimination?


So far, we have relied on an intuitive understanding of the ideas of **discrimination** and **bias**, mostly by looking at case studies.

While it is challenging to come up with an all-encompassing definition of discrimination, I will give a very short summary of what political philosophers, social scientists and lawmakers have been saying about this matter.

Covering this topic during a CS lecture is challenging for both you and me!

This will **not** be comprehensive in any shape or form.



---

## Discrimination

Sources for this lecture:

* Reuben Binns. "Fairness in Machine Learning: Lessons from Political Philosophy". Proceedings of Machine Learning Research. 2018.
* Lippert-Ramussen. "Born Free and Equal?". Oxford University Press. 2014.
* European Court of Human Rights. "Handbook on European Non-Discrimination Law". 2010.



---


## Discrimination?

A few relevant questions:

* What is discrimination, exactly?
* What makes it wrong?
* Is "algorithmic discrimination" any different from human-produced discrimination?



---


## What is discrimination?

Direct discrimination is the practice of **treating differently** people on the basis of membership to a salient social group.

Examples:

* Preferring to hire a male applicant over an equally-qualified female applicant
* Imposing stricter conditions for parole on applicants belonging to a minority group

In the EU, this is covered by Article 21 of the Charter of Fundamental Rights.

In the US and in the ML fairness literature, this concept is usually referred to as disparate treatment.


---


## Indirect discrimination

Indirect discrimination is the practice of offering **the same treatment** to people in **different salient social groups** if this leads to one group of people **being put at a particular disadvantage**. 

In the US and in the ML fairness literature, this concept is usually called disparate impact.

Examples (EU Handbook on Anti-Discrimination Law):

* *Hilde Schönheit v. Stadt Frankfurt am Main*. The pensions of part-time employees were calculated using a different rate to that of full-time employees. This different rate was not based on the differences of the time spent in work. Thus, part-time employees received a smaller pension than full-time employees, even taking into account the different lengths of service, effectively meaning that part-time workers were being paid less. This neutral rule on the calculation of pensions applied equally to all part-time workers. However, because around 88% of part-time workers were women, **the effect of the rule was disproportionately negative for women as compared to men**.


---


## Indirect discrimination

Examples:

* *D.H. and Others v. the Czech Republic*. A series of tests were used to establish the intelligence and suitability of pupils in order to determine whether they should be moved out of mainstream education and into special schools. These special schools were designed for those with intellectual disabilities and other sources of learning difficulty. The same test was applied to all pupils who were considered for placement in special schools. However, in practice the test had been designed around the mainstream Czech population with the consequence that Roma students were inherently more likely to perform badly – which they did, **with the consequence that between 50% and 90% of Roma children were educated outside the mainstream education system**.


---

## Indirect discrimination?

At some level, we could disagree that the cases above constitute discrimination - and some certainly will disagree.

One possible argument: nobody **intended** any harm on certain groups, and the tests/pension rates were not created/computed with these issues in mind. 

Thus, no discrimination actually happened.


---


## What makes discrimination wrong?

In moral and political philosophy, the existance of **systematic animosity** for or against certain salient social groups on the part of the decision maker has traditionally been the ground for defining discrimination as wrong.

If the decision-maker also has an explicitly negative intent or a lack of respect towards an individual, then the decision maker is engaging in **discrimination**. 

Thus, one possible ground which we may analyze is **intent**.



---


## Intent and Discrimination

If the possession of certain "hostile" mental states is necessary to the definition of discrimination, then the case studies we have seen so far **do not constitute discrimination**.

Of course, algorithms possess no mental states.

The data scientists behind the AI systems may easily claim that they possessed no ill intent towards certain groups.

We also notice one central issue: the mental states account **does not cover indirect discrimination** necessarily.

---


## Individuals and Discrimination

Another possible ground: relying on inferences about individuals which are **based on generalizations** about the groups of which they are a member of. 

In other words: **failing to treat people as individuals**.

Assume that an employer reads that smokers are on average less productive than non-smokers. Then, job applicants who smoke are rejected without fail.

---


## Individuals and Discrimination

Here, we run into several problems:

* The definition of **generalization**. Assume that the same employer instead uses a long productivity test to decide whether an applicant should be hired. This is still generalization, but the course of action now seems less discriminatory.
* Is the point about **insufficiently precise** means of generalization? This seems unsatisfactory. How precise should they be, exactly?
* Is **algorithmic decision-making** ever admissible, under these conditions? A machine learning system usually performs **induction**, which is in itself a form of **generalization** (arguably).


---

## Egalitarianism

Another proposal is to look at our issues with the algorithms presented so far under the lens of egalitarianism.

Broadly: egalitarianism is the idea that people should be treated equally and that - sometimes - certain valuable things should be equally distributed. 

Therefore, we may say that the systems we have observed so far **are not egalitarian**. 

---

## Equality of what?

Not all egalitarian positions agree on **what** should be equally distributed. 

Competing views: pleasure or preference-satisfaction (Cohen), resources such as income and assets (Rawls and Dworkin), the ability and resources necessary to do certain things (Sen).

Algorithms which fail to equally distribute these may be inflicting **harms** to people in a disparate way depending on which group they belong to.


---


## Harms

A discriminatory (non-egalitarian) algorithm may inflict two different kind of harms:

* **Allocative**. Certain groups are denied resources in a disparate way. Examples: COMPAS, Amazon hiring system.
* **Representational**. The AI system reinforces the subordination of some groups. Example: beauty.ai, automatic translation


---

## Luck and desert

One facet which we have not discussed so far is **desert**, i.e. the condition of being deserving of some resource due to choices, talent, hard work, etc..

The (luck) egalitarian position is that inequalities which are due to **luck** - circumstances out of our control - should be corrected. 

Of course, tracing a line between which features in a feature vector are due to pure luck and which are also due to an individual's **choices** is far from simple.

**Measuring** luck and desert may also be extremely difficult and costly.



---


## Egalitarianism and Indirect Discrimination

Nonetheless, one could argue that egalitarianism provides a reasonable grounding for the wrongness of **both** direct and indirect discrimination.





---


## Discrimination?

A few relevant questions:

* What is discrimination, exactly?
* What makes it wrong?
* **Is "algorithmic discrimination" any different from human-produced discrimination?**

Please discuss!


---


## Fairness by unawareness

One thing we need to be aware about is that **data-driven algorithms are able to recover sensitive information**. 

As an example, your zipcode gives away some information about your ancestry/ethnicity as the population statistics are usually publicly available. 



---


## Fairness by unawareness

<center>

![w:800px](img/unawareness.png)

**Left**: one particular feature has the same distribution but a mean difference of 0.5 across groups. **Right**: how well we can predict whether an individual belong to one group or the other as the number of these features grows.
</center>


---

## Fairness by unawareness


<br>


**Unawareness** is usually not a thing in real-world data, and is hard to claim if you look at the correlations.

---


## Statistical non-discrimination criteria

When dealing with a classification algorithm, we may be interested in **quantifying** the unbalance in its decisions **across different groups**. 

Our setup is similar to what we discussed in the last lecture:

$X$ covariates; $Y$ target variable; $\hat{Y}$ our estimation of the target via the classifier $f_{\theta}$; the risk score $R$.

We add: $A$ the sensitive attribute, a discrete feature representing the group an individual belongs to. Examples: 1 man, 0 woman; 2 asian, 1 black, 0 white...




---


## Statistical non-discrimination criteria


<br>

<center>

![](img/hiring-2.png)

</center>

We may be interested in balancing the **acceptance rate** of our classifier, i.e. $P(\hat{Y} = 1)$.


---


## Statistical non-discrimination criteria

<br>

Note that here we are assuming that one particular value of $Y$ - and therefore of $\hat{Y}$ represents a more desirable outcome - getting an interview, getting a loan, being released on parole...


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
\mathbb{P}(\hat{Y} = 1 \mid Y = 1, A = a) = \mathbb{P}(\hat{Y} = 1 \mid Y = 1, A = b) \\
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








