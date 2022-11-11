---
marp: true
---


## Mathematical Theory of Data Accuracy

Ask an analyst to find issues in a dataset and they'll come back in a few hours with a long list. Ask them instead to *validate* a dataset and they'll be busy for months. 

> Data inaccuracy is detectable, data accuracy is not. 

---

## What we will cover

What can we *know* about the data
* **Maximal Alert Theorem** - The *only* way to find issues is to look for constraint violations
* **Quality Coverage Equation** - What percent of issues are findable
  - There are always some unfindable issues 



---
## The Setup 
Let $\hat{X}=(x_1, x_2... x_N)$ be observable datapoints measuring some 'true' values $\hat{T}=(t_1, t_2... t_N)$. 

For now we assume that the data is discrete and only takes integer values.  

--- 

**Definition:** Data Accuracy Issue

A data accuracy issue is simply when $\hat{X} \neq \hat{T}$.

---

## Example

|Var| Definition|
|-|-|
$t_1$| The *true* total cars entering the ferry at the first stop.
$x_1$| The *observed* total cars entering the ferry at the first stop.
$t_2$| The *true* total cars leaving the ferry at the second stop.
$x_2$| The *observed* total cars leaving the ferry at the second stop.

You can't see $t_1$ and $t_2$ directly, but we hope that our volunteer did a dutiful job so that $x_1=t_1$ and $x_2=t_2$.

---


## Data Accuracy Issues at the Ferry

Let's take a look at some of the data from the ferry. 



|day|$x_1$|$x_2$|
|-|-|-|
Monday|9|5|
Tuesday|7|3|
Wednesday|5|7|

---

Now I'm going to append the true number of cars entering and exiting. We normally don't get to see this. 

|day|$t_1$|$t_2$|$x_1$|$x_2$| 
|-|-|-|-|-|
Monday|9|5|9|5|
Tuesday|8|3|7|3|
Wednesday|7|5|5|7|

---

A data accuracy issue is whenever $\hat{T}\neq \hat{X}$. Let's mark the rows with data accuracy issues. 

|day|$t_1$|$t_2$|$x_1$|$x_2$|  Is DQ Issue
|-|-|-|-|-|-|
Monday|9|5|9|5|No|
Tuesday|**8**|3|**7**|3|Yes|
Wednesday|**7**|**5**|**5**|**7**|Yes|

* For Tuesday $t_1 \neq x_1$ and Wednesday has $t_1 \neq x_1$ and $t_2 \neq x_2$ so these two days have data accuracy issues. 

---

## Adding Real World Assumptions


We introduce an assumption $g$, 
* $g(\hat{T})=\text{True}$ if the assumption holds
* $g(\hat{T})=\text{False}$ if the assumption is violated.

* Generally we can have many real-world assumptions: $g_1, g_2.. g_M$. 


---

**Definition:** Plausible

If $\hat{T}$ satisfies all constraints $g_1, g_2.. g_M$, we call $\hat{T}$ plausible. 

---


## Real World Assumptions Example 

Let's go back to the ferry example. 

* $t_1\geq t_2$ : conservation of cars

* $g(t_1,t_2) = t_1 \geq t_2$. 

---

## General Data Accuracy Alerts

A data accuracy alert monitors data and fires when a data accuracy issue is detected. 

Let's come up with a definition given our framework. 

---

**Definition:** Deterministic Alert 

An alert that fires only when there is a plausible data accuracy issue. 

More formally:

* A binary function of the data , $f$, is a *deterministic alert* if: 

* When $\hat{T}$ is plausible and $f(\hat{X})$ has value $\text{True}$ then $\hat{T} \neq \hat{X}$.


---

## Deterministic Alert Example

Let's come up with a deterministic alert for our ferry problem.

* We know that $t_1 \geq t_2$ by the conservation of cars assumption. 

* Therefore, if we see $x_1 \leq x_2$ then we know we have a data accuracy issue. 

* Let's define our deterministic alert $a$ as $a(x_1,x_2) = x_1 < x_2$. 

* When $a$ is $\text{True}$ we know there is a data accuracy issue. 

---


**Note:** We have not yet proven that $a$ satisfies the formal definition of a deterministic alert. The next section contains a proposition that shows that $a$ indeed satisfies the definition.  

---

Given the ferry problem setup, let's try to find data accuracy issues: 

|day|$x_1$|$x_2$|$a(x_1,x_2)$|
|-|-|-|-|
Monday|9|5|$\text{False}$|
Tuesday|7|3|$\text{False}$|
Wednesday|5|7|$\text{True}$|

---

Now let's join back with the true values to see how good this alert is. 

|day|$t_1$|$t_2$|$x_1$|$x_2$|  Is DQ Issue|$a(x_1,x_2)$|
|-|-|-|-|-|-|-|
Monday|9|5|9|5|No|$\text{False}$|
Tuesday|8|3|7|3|Yes|$\text{False}$|
Wednesday|7|5|5|7|Yes|$\text{True}$|

Our data accuracy alert caught the issue on Wednesday, but it did not catch the issue on Tuesday. 

--- 



## Maximal Alert 

It is natural to ask - what is the most comprehensive alert on the data? Is there an alert that will catch every issue?

---

**Proposition:** If $a_1$ and $a_2$ are deterministic alerts, so is $(a_1 \text{ or }a_2)$.

Proof omitted. 

---
We can imagine a *maximal deterministic alert* which fires if *any* other deterministic alert fires. If we magically knew all other deterministic alerts we could create this maximal alert by *or-ing* them all together. 

---

**Definition:** Maximal Alert

The alert constructed by 'or-ing' all other deterministic alerts together. Therefore the maximal alert fires iff one or more other deterministic alerts fire.


---

## Alert Example

**Proposition:**  $\neg g(\hat{X})$ is a deterministic alert for any constraint $g$. 


Proof: We need to show that if $\neg g$ fires and the setup is plausible that $\hat{T} \neq \hat{X}$.


- If the alert fires we have $g(\hat{X}) = \text{False}$. 

- If the setup is plausible we have $g(\hat{T}) = \text{True}$.

Therefore  $g(\hat{T}) \neq g(\hat{X})$. This implies that $\hat{T} \neq \hat{X}$.




---
## Alert Example: Assumption Violation Alert

* *assumption violation alert* (or $ava$) which fires when *any* assumption is violated.

* $ava(\hat{X}) = \neg g_1(\hat{X}) \text{ or } \neg g_2(\hat{X})... \text{ or }\neg g_M(\hat{X})$


--- 

**Proposition:** The assumption violation alert is a deterministic alert. 

Proof: 

Now that we have shown $\neg g(\hat{X})$ is a deterministic alert, since the $ava$ is an "or" operation on deterministic alerts, it is an alert. 

Q.E.D.

---

**Theorem:** Maximal Alert Theorem: *The Maximal Alert is the Assumption Violation Alert.* 

* We must show that if one of these alerts fire, so does the other one. 

---

**Step 1:** Maximal Alert fires when Assumption Violation Does

If the constraint violation alert fires, so does the maximal one since the maximal alert fires if any alert fires. 

---

**Step 2:** Assumption Violation Fires when Maximal Alert fires 

We can show this by contradiction:

Say there exists a $(T=\hat{T^*},X=\hat{X^*})$ where the maximal alert fires and the constraint violation alert doesn't. 

Now consider $(\hat{T}=\hat{X^*},\hat{X}=\hat{X^*})$. 
- The maximal alert will fire since $\hat{X}$ is unchanged. 
- There are no data accuracy issues since $\hat{X} = \hat{X^*} = \hat{T}$.
- $\hat{T}$ does not violate any constraints because $\hat{X}$ doesn't violate $ava$ and  $\hat{X} = \hat{T}$. In other words, $\hat{T}$ is *plausible*. 

The maximal alert can't fire in a plausible situation with no data accuracy issues since this contradicts the definition of a deterministic alert. Therefore the maximal alert can't fire when the constraint violation alert is silent.

Q.E.D.

---


## Implications

 

> The *only* way to be sure of data accuracy issues is to make real-world assumptions and look for violations. 


---

## Accuracy Coverage Equation


* Let $P$ denote the set of $(t_1... t_N)$ that are plausible (don't violate real-world assumptions). 

* Let $\lvert P \rvert$ denote the size of the plausible set.

* Let $\lvert \hat{X} \rvert$ be the number of states the data can be in. 
  * For example, in a binary system $x_1... x_N$ , we have $\lvert \hat{X} \rvert = 2^N$.  

---


**Theorem:** Accuracy Coverage Equation

*Fraction of Accuracy Issues Detected = $\frac{ \lvert \hat{X} \rvert- \lvert P \rvert}{  \lvert \hat{X} \rvert - 1}$*

---
Proof 

* Size of the state space of $(\hat{T},\hat{X})$ where $\hat{T}$ is plausible= $\lvert P \rvert$  $\lvert \hat{X} \rvert$
  * State space of plausible $\hat{T}$ times state space of $\hat{X}$

* States with no issues =  $\lvert P \rvert$
  * Every plausible $\hat{T}$ has only one state $\hat{X}$ without accuracy issues ($\hat{X}=\hat{T}$).

* Data Accuracy Issues = $\lvert P \rvert  \lvert \hat{X} \rvert - \lvert P \rvert$

  * Total system states minus states without issues

* Detectable Issues = $\lvert P \rvert(\lvert \hat{X} \rvert- \lvert P \rvert)$

  * State space of plausible $\hat{T}$ times state space of $\hat{X}$ which violates $ava$. 

* Fraction of Accuracy Issues Detected = $\frac{\lvert P \rvert(\lvert \hat{X} \rvert- \lvert P \rvert)}{ \lvert P \rvert  \lvert \hat{X} \rvert - \lvert P \rvert} = \frac{\lvert \hat{X} \rvert- \lvert P \rvert}{  \lvert \hat{X} \rvert - 1}$

---


## Data Issues are Detectable, Data Correctness is Not

Looking at the coverage equation  $\frac   {\lvert \hat{X} \rvert- \lvert P \rvert}{  \lvert \hat{X} \rvert - 1}$ we see that we get 100% coverage only in the case where  $\lvert P \rvert = 1$, which is a trivial case. In the case that $\lvert P \rvert = 1$, there was no reason to gather any data in the first place, since $t_1.. t_n$ is fully determined by the constraints. Therefore we can say the following: 



---


## Conclusion: What can we *know* about the data?


Maximal Alert Theorem: *The Maximal Alert is the Assumption Violation Alert.* 

> The *only* way to detect data accuracy issues is to make real-world assumptions and look for violations. 

Fraction of Accuracy Issues Detected = $\frac{\lvert P \rvert(\lvert \hat{X} \rvert- \lvert P \rvert)}{ \lvert P \rvert  \lvert \hat{X} \rvert - \lvert P \rvert} = \frac{\lvert \hat{X} \rvert- \lvert P \rvert}{  \lvert \hat{X} \rvert - 1}$

> All data systems have undetectable issues, which decrease linearly with the size of the plausible true states. 

