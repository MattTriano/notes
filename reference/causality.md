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

# Glossary

* **The fundamental problem of causal inference**:
    * The fundamental problem of causal inference is that we can only observe one potential outcome for a given treated or untreated unit.

* **Intervention**:
    * Interchangeable with "treatment", "exposure", or sometimes "action". In a medical context, a common example of an intervention would be a literal treatment (eg medication, surgery, dietary change, etc). In governmental context, a common example would be inclusion in a program.  
