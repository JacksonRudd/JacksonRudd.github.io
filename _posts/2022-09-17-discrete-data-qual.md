# Simple Theory of Data Quality Alerts: Discrete Systems

Anyone who works in data will eventually develop trust issues. Data is often different from the reality it models and a result of measurement errors. It can be difficult to determine what is actually true. 

If *data quality* is the delta between data and reality, poor data quality is the organizational equivalent of drunk goggles. 

But how can an organization discover if data is high quality or not? In this post I seek to answer this question for discrete data systems. The theory is simple and intuitive, and in my opinion adds some rigour to instincts that all data professionals develop eventually. The ideas from these discrete systems will give good insight into the continuos theory of data quality (which will be covered in a future post). 



## Lawn Mowing Example
Let's start with an example of a data acquisition system that is prone to quality issues.

You have two children who have housework to do while you're out of town. You call and ask the oldest *'Has the lawn been mowed?'*. Lets model this simple data system:

|Lawn Mowed|Eldest's Answer|
|-|-|
|True|True|
|True|False|
|False|True|
|False|False|

For example the first row describes where the lawn was mowed, and you got an honest answer. 

Let's append to this table the data quality states

|Lawn Mowed|Eldest's Answer|Data Quality|
|-|-|-|
|True|True|Good|
|True|False|Bad|
|False|True|Bad|
|False|False|Good|

This simple system can be in four states, of which two are bad data quality. We cannot detect any of these data quality issues without changing the setup.

## Lawn Mowing with Two Observations

Now let us also ask the youngest if the lawn has been mowed: 

|Lawn Mowed|Eldest's Answer|Youngest's Answer| Data Quality|
|-|-|-|-|
|True|True|True|Good|
|True|True|False|Bad|
|True|False|True|Bad|
|True|False|False|Bad|
|False|True|True|Bad|
|False|True|False|Bad|
|False|False|True|Bad|
|False|False|False|Good|

We will append to this table the *detectable* data quality issues. We will know something is wrong if the children don't agree. 


|Lawn Mowed|Eldest's Answer|Youngest's Answer| Data Quality| Detectable Issue|
|-|-|-|-|- |
|True|True|True|Good|No|
|True|True|False|Bad|Yes|
|True|False|True|Bad|Yes|
|True|False|False|Bad|No|
|False|True|True|Bad|No|
|False|True|False|Bad|Yes|
|False|False|True|Bad|Yes|
|False|False|False|Good|No|

We now have 6 states with data quality issues, and 4 of those are detectable by us, so we can now detect $$\frac{2}{3}$$ data quality issues in this system. 

## General Discrete Data Quality Setup

Let $${T_1... T_N}$$ be the true values of what we wish to observe and $${X_1... X_N}$$ be our observed values, subject to measurement error. 

For example, if you work in traffic modeling, and you wish to know the volume of traffic through a tunnel. You get a volunteer to observe the tunnel and write down over the course of the day how many cars enter and exit tunnel. Then you can have

|Var| Definition|
|-|-|
$$T_1$$| The *true* total cars entering the tunnel over the day.
$$X_1$$| The *observed* total cars entering the tunnel over the day.
$$T_2$$| The *true* total cars leaving the tunnel over the day.
$$X_2$$| The *observed* total cars leaving the tunnel over the day.

To complete our setup, we also add real world assumptions $$g_1... g_M$$ where we only consider $$T_1 .. T_N$$ where $$g_k(T_1..T_N)=True$$ for all $$k \in {1..M}$$. We can never observe $$T_i$$ that contradict our real world assumptions. 

For example, in our traffic problem we have the *conservation of cars constraint* $$g_1(T_1,T_2) = (T_1 \geq T_2) $$, since we know that cars can't exit the tunnel without entering it first.

In summary, a discrete data quality system consists of the following: 

*Definition:* **Plausible System**


$${X_1... X_N} : X_i \in \mathbb{N}$$ 

$$g_1... g_M \text{ where } g_k : (T_1...T_N) \rightarrow \{T,F\}$$  

$${T_1... T_N}: T_i \in \mathbb{N} $$


...

*Definition:* **Plausible State**

We call all states of a system where $$T_1... T_N$$ satisfies the constraints, *plausible*. We only consider plausible states when discussing systems, sometimes without making this explicit. 

...


For example, in our traffic system, $$T_1=5$$ and $$T_2 = 4$$ is plausible, while the reverse is not. 

*Example:* We can formalize the lawn mowing system example as a data quality system by using dummy variables: 

|Var| Definition|
|-|-|
$$T_1$$| The *true* state of the lawn.
$$X_1$$| The eldest's answer about the state of the lawn.
$$T_2$$| Another 'dummy' variable representing the true state of the lawn.
$$X_2$$| The youngest's answer about the state of the lawn.
$$g_1$$| $$T_1=T_2$$

...

## Definition Of Data Quality

For our discrete systems we can now define *Data Quality Issues*: 

*Definition:* **Data Quality Issues (Discrete System)**:

Any plausible realization of $${T_1... T_N}$$ , $${X_1... X_N}$$
where there exists an $$i$$ where $$T_i \neq X_i$$.   

...


## Trustworthy Alerts

Given a data quality system, define a *trustworthy alert* to be a function of the $$X_1... X_N$$ that fires only for data quality issues. Again we consider only plausible states.  


*Definition:*  **Trustworthy Alert**

$$a :(X_1... X_N) \rightarrow \{True, False\}$$ where $$ (a(X)==T) \implies (X_1.. X_N, T_1.. T_N)$$ is a data quality issue. 

...


By feeding our data $$X_i$$ into our real world constraints $$g_k$$ we can detect data quality issues. 
 
*Proposition:* Define $a(X_1... X_n) = \text{not } g(X_1... X_n)$ for some constraint $$g$$, this is a trustworthy alert.

$$a((X_1...X_n)= g(X_1...X_n) \neq T \implies g(X_1...X_N) \neq g(T_1...T_N) \implies  (X_1...X_N) \neq (T_1...T_N) \implies $$ data quality issue. 

...

Also, it is clear that if $$a$$ is an trustworthy alert and $$b$$ is an trustworthy alert than $$(a \text{ or } b )$$ is also a trustworthy alert. 

This leads us to define a *maximal alert* as : $$MA(X_1.. X_N) = (g_1(X_1...X_N) = F) \text{ or } (g_2(X_1...X_N) = F) ... \text{ or } (g_k(X_1...X_N) = F)$$. 

*Proposition:* the maximal alert is the best alert, in that any other trustworthy alert only fires when the maximal alert fires. 

Proof: Say a trustworthy alert $$a$$ fires when $$MA$$ is silent. Then there is some $$X_1... X_N$$ where $$a(X_1.. X_N) = True$$ and $$MA(X_1.. X_N)=False$$. Now let $$T_1 = X_1 .. T_N = X_N$$ then this is a plausible state with no data quality issues. Therefore $$a$$ is not trustworthy. 

...

In summary, the *only* way to detect data quality issues is by feeding our assumptions about the real world back into the data.

## Quality Coverage Equation

In a data quality system let $$P$$ denote the set of $$(T_1... T_N)$$ that are plausible. Let $$\lvert P \rvert$$ denote the size of the plausible set.

Let's take a binary system (this can be modeled as constraints). Then the $$X_1... X_N$$ can be in $$2^N$$ states, while the $$T_1... T_N$$ can be in $$\lvert P \rvert$$ many states. Therefore the system as a whole can be in $$\lvert P \rvert  2^N$$ states. 

There will only every be $$\lvert P \rvert$$ many good data quality states. Therefore there is $$\lvert P \rvert  2^N - \lvert P \rvert$$ bad data quality states. 

Finally since the maximal alert only fires for $$(X_1.. X_n)$$ that violates the constraints, we have $$ \lvert P \rvert(2^N- \lvert P \rvert)$$ states where the alarm fires. 

In summary: 

Plausible states = $$\lvert P \rvert  2^N$$

Data Quality Issues = $$\lvert P \rvert  2^N - \lvert P \rvert$$

Detectable Issues = $$ \lvert P \rvert(2^N- \lvert P \rvert)$$

Fraction of Quality Issues Detected = $$\frac{\lvert P \rvert(2^N- \lvert P \rvert)}{ \lvert P \rvert  2^N - \lvert P \rvert} = \frac{2^N- \lvert P \rvert}{  2^N - 1} $$

This equation easily generalizes to any discrete system, by simply replacing $$2^N$$ with the cardinality of the state space $$X_1...X_N$$. 

Let's try this equation on our lawn system with the dummy variables. 

We recall: 

|Var| Definition|
|-|-|
$$T_1$$| The *true* state of the lawn.
$$X_1$$| The eldest's answer about the state of the lawn.
$$T_2$$| Another 'dummy' variable representing the true state of the lawn.
$$X_2$$| The youngest's answer about the state of the lawn.
$$g_1$$| $$T_1=T_2$$

Therefore 

$$N=2$$

$$P = \{(T_1 = True, T_2=True), (T_1 = False, T_2=False)\} $$

$$\lvert P \rvert = 2$$ 

$$2^N = 4$$

Fraction of Quality Issues Detected = $$\frac{2^N- \lvert P \rvert}{  2^N - 1} = \frac{2}{3} $$, in accordance with what we observed above. 

## Data Issues are Detectable, Data Correctness is Not

Looking at the coverage equation  $$\frac{2^N- \lvert P \rvert}{  2^N - 1}$$ we see that we get 100% coverage only in the case where  $$\lvert P \rvert = 1$$, which is a trivial case. In the case that $$\lvert P \rvert = 1$$, there was no reason to gather any data in the first place, since $$T_1.. T_n$$ is fully determined by the constraints. Therefore we can say the following: 

*In non-trivial discrete data systems there will always be quality issues that are undetectable.*

Sadly, this also carries over to the continuos case, which we will cover in the future. 


As we gather more expectations about the real world, we can add constraints, which will lower $$\lvert P \rvert$$, and raise the data coverage. 



## Summary

We looked at discrete systems and found that the *only* way to detect data quality was by feeding our real world assumptions onto the data. A data quality alert is best when it simply takes all of the real world constraints in our system, and checks to see if any has been violated. We derived a simple equation for data quality coverage in discrete systems so that we know longer have to count truth tables. In accordance with our intuition, as the number of plausible states decreases, the coverage of data quality increases. 

This theory is simple and intuitive. It suggests a business process where real world constraints are systematically gathered in an organization and turned into a *maximal alert*. 

In a future post, we can instead look at continuous systems where the theory remains simple, but becomes more about degrees of freedom and geometry. 

Thanks for reading, and please reach out with comments or suggestions. 













