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


# Glossary

* **Intervention**:
    * Interchangeable with "treatment", "exposure", or sometimes "action". Indicates a
