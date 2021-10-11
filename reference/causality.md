# Causality

## Causal Effects

Actions often precede changes in conditions. If the change is sufficiently desirable or undesireable, it becomes important to understand what actions **caused** the change, so that we can deliberately produce more positive change or avoid negative change. 

### Hypothetical Worlds approach

Imagine you're considering a potential intervention to treat for some disease, and you could create a perfect duplicate the world. On the original world, no one in the population of interest receives the treatment ($A=0$), and on duplicate world, everyone in that population receives the treatment ($A=1$). If you took the average outcome ($\text{mean}(Y)$) in the treatment world and subtract the average outcome in the no-treatment world, you get the **average causal effect**. In practice, it's not possible to duplicate the world, so we need to make assumptions to utilize this thought experiment.

$$E(Y^1-Y^0)$$

In general, that expected value is not the same as the expected value of the treated population less the expected value of the untreated population, or

$$E(Y^1-Y^0) \neq E(Y|A=1) - E(Y|A=0)$$

because $E(Y|A=1)-E(Y|A=0)$ is looking at different subpopulations receiving treatment, wheras $E(Y^1-Y^0)$ uses the hypothetical worlds thought framework where a population is duplicated and only one receives treatment.

#### Notation Note $E(Y|A=1)$

$E(Y|A=1)$ means "expected value of $Y$ given $A=1$", which implicitly restricts to the subpopulation of people recieving treatment $A$.

### Terms

* Causal relative risk
    * $E(\frac{Y^1}{Y^0})$
* Causal effect of treatment on the treated
    * $E(Y^1-Y^0|A=1)$
* Average causal effect in the subpopulation with covariate $V=v$
    * $E(Y^1-Y^0|V=v)$
    
## Causal Assumptions

### SUTVA: Stable Unit Treatment Value Assumption
This assumption allows us to express the potential outcome for the $i^{th}$ person in terms of only that person's treatments. It assumes the following
* No Interference
    * Units do not interfere with each other
    * Treatment assignment of one unit doesn't impact the outcome of any other unit
* One version of treatment

### Consistency Assumption

$Y=Y^a$ if $A=a \forall a$

### Ignorability Assumption

"Given pre-treatment covariates $X$, treatment assignment is independent from the potential outcomes."

$$Y^0,Y^1 \Perp A|X$$

Basically, this says that for a subpopulation with the same pre-treatment covariates $X$, treatment A is randomly assigned. 

This assumption is sometimes called the "no unmeasured confounders" assumption (although that seems to jump past the part where we determine what pre-treatment covariates must be included in the set $X$).

### Positivity Assumption

$$P(A=a|X+x)>0 \forall a, x$$

Or, that it's possible that there's a non-zero probability that any set of covariates $x$ could receive any of the treatements $a$. Stated differently, this assumption says that there is no set of covariates $x$ that would always receive treatment $a$ (so an out-of-sample error becomes impossible with a sufficiently large sample).


Satisfy assumptions allows us to formally declare effects to be causal.

$E(Y|A=a, X=x)$: this involves only observed data, no hypothetical worlds needed.
$E(Y|A=a, X=x) = E(Y^a|A=a, X=x)$ by consistency,
$E(Y|A=a, X=x) = E(Y^a|X=x)$ by ignorability,

Averaging over $X$ gives the marginal causal effect.

## Stratification

If we want the marginal causal effect, we can average over the distribution of X. 

For example, let there be a single categorical $X$ variable. 

$$E(Y^a) = \Sum_x E(Y|A=a, X=x)P(X=x)$$

Standardization Example:
Compute rate of a given outcome for two different subpopulations: patients with prior exposure to potential causal condition, and patients without any prior exposure. Then, take a weighted average where weights are based on proportion of people in each subpopulation. 

eg 

|  | MACE = yes | MACE = no | Total |
| --- | --- | --- | --- |
| Saxa = yes | 350 | 3650 | 4000 |
| Saxa = no | 500 | 6500 | 7000 |
| Total | 750 | 10250 | 11000 |

Probability of MACE given Saxa=yes: 350 / 4000 = 0.088
Probability of MACE given Saxa=no: 500 / 7000 = 0.071

#### Stratifying on our $X$ variable

$X$ = prior OAD use
Prior OAD use=no
|  | MACE = yes | MACE = no | Total |
| --- | --- | --- | --- |
| Saxa = yes | 50 | 950 | 1000 |
| Saxa = no | 200 | 3800 | 4000 |
| Total | 250 | 4750 | 5000 |

probability of MACE if Saxa=yes: 50/1000 = 0.05
probability of MACE if Saxa=no: 200/4000 = 0.05

Prior OAD use=yes
|  | MACE = yes | MACE = no | Total |
| --- | --- | --- | --- |
| Saxa = yes | 300 | 2700 | 3000 |
| Saxa = no | 200 | 2700 | 3000 |
| Total | 600 | 5400 | 6000 |

probability of MACE if Saxa=yes: 300/3000 = 0.10
probability of MACE if Saxa=no: 300/3000 = 0.10

Note: Disproportionate Saxagliptin use by those with prior exposure to OAD ([3k Saxa=yes+OAD=yes, 1k Saxa=yes+OAD=no] vs [3k Saxa=no+OAD=yes, 4k Saxa=no+OAD=no]).

So, with stratification on prior OAD, the probability of MACE does not appear to be impacted Saxagliptin.

$$ E(Y^{saxa})=E(Y|A=\text{saxa}, X=\text{prior OAD use: yes})P(\text{prior OAD use: yes}) + E(Y|A=\text{saxa}, X=\text{prior OAD use: no})P(\text{prior OAD use: no}) $$

$$ E(Y|A=\text{saxa}, X=\text{prior OAD use: yes}) = \frac{300}{3000} $$
$$ P(prior OAD use: yes) = \frac{6000}{11000} $$
$$ E(Y|A=\text{saxa}, X=\text{prior OAD use: no}) = \frac{50}{1000} $$
$$ P(prior OAD use: no) = \frac{5000}{11000} $$

$$ E(Y^{saxa}) = (0.10)(6/11) + (0.05)(5/11) = 0.05454 + 0.02272 = 0.07727 $$

And for Sitagliptin
$$ E(Y^{sita}) = E(Y|A=\text{sita}, X=\text{prior OAD use: yes})P(\text{prior OAD use: yes}) + E(Y|A=\text{sita}, X=\text{prior OAD use: no})P(\text{prior OAD use: no}) $$
$$ E(Y^{sita}) = (\frac{300}{3000})(\frac{6000}{11000}) + (\frac{200}{4000})(\frac{5000}{11000}) = 0.07727 $$

Outcome:
By stratifying on prior OAD exposure, we were able to determine that Sitagliptin and Saxagliptin didn't change the incidence of MACE.

A problem with stratification is that there are often multiple factors that must be stratified on. Failure to identify all necessary factors can result in incorrect predictions, and as more factors are added, we increase the likelihood that we'll have combinations of factors for which we have no data.


## Incident User design (aka New User design)
Restrict the treated population just to those newly initiating treatment. This eliminates some complexities that would come from having vastly different levels of treatment in different members of the sample population.

A common issue with this design emerges when the comparison treatment is **no treatment**, as it's not obvious when $t=0$ is for that treatment group.

Having an Active Comparator design (where the comparison treatment is an active treatment, ie not no-treatment). 

For example, if the treatment is yoga and the variable of interest is blood pressure, an active comparator design could have spin class as the comparison treatment. This would also reduce the number of confounding factors, as the populations of people who do yoga or do spin class likely have more in common than the populations of people who do yoga and those who do not excercise at all.


# Glossary

* **The fundamental problem of causal inference**:
    * The fundamental problem of causal inference is that we can only observe one potential outcome for a given treated or untreated unit.

* **Intervention**:
    * Interchangeable with "treatment", "exposure", or sometimes "action". In a medical context, a common example of an intervention would be a literal treatment (eg medication, surgery, dietary change, etc). In governmental context, a common example would be inclusion in a program.  
