# Simple Theory of Data Quality Alerts: Discrete Systems

Anyone who works in data will eventually develop trust issues. Data is often different from the reality it models and a result of measurement errors. It can be difficult to determine what is actually true. 

If *data quality* is the delta between data and reality, poor data quality is the organizational equivalent of drunk goggles. 

But how can an organization discover if data is high quality or not? In this post I seek to answer this question for discrete data systems. The theory is simple and intuitive, and in my opinion adds some rigour to instincts that all data professionals develop eventually. The ideas from these discrete systems will give good insight into a more general theory of data quality. 



## Lawn Mowing Example
Let's start with an example of a data acquisition system that is prone to quality issues.

You have two children who have housework to do while your out of town. You call and ask the oldest *'Has the lawn been mowed?'*. Lets model this simple data system:

|Lawn Mowed|Eldest's Answer|
|-|-|
|T|T|
|T|F|
|F|T|
|F|F|

Let's append to this table the data quality states

|Lawn Mowed|Eldest's Answer|Data Quality|
|-|-|-|
|T|T|Good|
|T|F|Bad|
|F|T|Bad|
|F|F|Good|

This simple system can be in four states, of which two are bad data quality. We cannot detect any of these data quality issues without changing the setup.

## Lawn Mowing with Two Observations

Now let us also ask the youngest if the lawn has been mowed: 

|Lawn Mowed|Eldest's Answer|Youngest's Answer| Data Quality|
|-|-|-|-|
|T|T|T|Good|
|T|T|F|Bad|
|T|F|T|Bad|
|T|F|F|Bad|
|F|T|T|Bad|
|F|T|F|Bad|
|F|F|T|Bad|
|F|F|F|Good|

We will append to this table the *detectable* data quality issues. We will know something is wrong if the children don't agree. 


|Lawn Mowed|Eldest's Answer|Youngest's Answer| Data Quality| Detectable Issue|
|-|-|-|-|- |
|T|T|T|Good|No|
|T|T|F|Bad|Yes|
|T|F|T|Bad|Yes|
|T|F|F|Bad|No|
|F|T|T|Bad|No|
|F|T|F|Bad|Yes|
|F|F|T|Bad|Yes|
|F|F|F|Good|No|

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

To complete our setup, we also add real world assumptions $$g_1... g_M$$ where we only consider $T_1 .. T_N$ where $$g_k(T_1..T_N)=True$$ for all $$k \in {1..M}$$. We can never observe $$T_i$$ that contradict our real world assumptions. 

For example, in our traffic problem we have the *conservation of cars constraint* $$g_1(T_1,T_2) = (T_1 \geq T_2) $$, since we know that cars can't exit the tunnel without entering it first.

In summary, a discrete data quality system consists of the following: 

*Definition:* **Discrete Data Quality System**
 
A data quality system is the of states of discrete variables

$${T_1... T_N}: T_i \in \N$$ 

$${X_1... X_N} : X_i \in \N$$ 

$$g_1... g_M$$ where $$g_k : (T_1...T_N) \rightarrow \{T,F\}$$  

...

*Definition:* **Plausible State**

We call all states of a system where $$T_1... T_N$$ satisfies the constraints, *plausible*. 

...


For example, in our traffic system, $$T_1=5$$ and $$T_2 = 4$$ is plausible, while the reverse is not. 

*Example:* We can formalize the lawn mowing system example as a data quality system by using dummy variables: 

|Var| Definition|
|-|-|
$$T_1$$| The *true* state of the lawn.
$$X_1$$| The eldest's
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

Given a data quality system, define a *trustworthy alert* to be a function of the data that fires only for data quality issues. 

In other words 

*Definition:*  **Trustworthy Alert**

$$a :(X_1... X_N) \rightarrow \{True, False\}$$ where $$ a(X) == True \implies (X_1.. X_N, T_1.. T_N)$$ is a data quality issue. 

...

by feeding our data $$X_i$$ into our real world constraints $$g_k$$ we can detect data quality issues. 

In mathematical terms: 

$$g(X_1...X_n) \neq T \implies g(X_1...X_N) \neq g(T_1...T_N) \implies  (X_1...X_N) \neq (T_1...T_N) \implies $$ data quality issue. 

Also, it is clear that if $$a$$ is an alert and $$b$$ is an alert than $$(a \mid b )$$ is an alert, where $$\mid $$ is 'or'. 

This leads us to define a *maximal alert* as : $$MA(X_1.. X_N) = (g_1(X_1...X_N) = F) | (g_2(X_1...X_N) = F) ... | (g_k(X_1...X_N) = F)$$. 

*Proposition:* the maximal alert is the best alert, in that any other trustworthy alert only fires when the maximal alert fires. 

Proof: Say a trustworthy alert $$a$$ fires when $$MA$$ is silent. Then there is some $$X_1... X_N$$ where $$a(X_1.. X_N) = True$$ and $$MA(X_1.. X_N)=False$$. Now let $$T_1 = X_1 .. T_N = X_N$$ then this is a plausible state with no data quality issues. Therefore $$a$$ is not trustworthy. 

...

In summary, the *only* way to detect data quality issues is by feeding our assumptions about the real world back into the data.

## Quality Coverage Equation

In a data quality system let $$P$$ denote the set of $$(T_1... T_N)$$ that are plausible. Let $$\lvert P \rvert$$ denote the size of the plausible set.

Let's take a binary system (this can be modeled as constraints). Then the $$X_1... X_N$$ can be in $$2^N$$ states, while the $$T_1... T_N$$ can be in $$\lvert P \rvert$$ many states. Therefore the system as a whole can be in $$\lvert P \rvert  2^N$$ states. 

There will only every be $$|P|$$ many good data quality states. Therefore there is $$\lvert P \rvert  2^N - \lvert P \rvert$$ bad data quality states. 

Finally since the maximal alert only fires for $$(X_1.. X_n)$$ that violates the constraints, we have $$ \lvert P \rvert(2^N- \lvert P \rvert)$$ states where the alarm fires. 

In summary: 

Plausible states = $$\lvert P \rvert  2^N$$

Data Quality Issues = $$\lvert P \rvert  2^N - \lvert P \rvert$$

Detectable Issues = $$ \lvert P \rvert(2^N- \lvert P \rvert)$$

Fraction of Quality Issues Detected = $$\frac{\lvert P \rvert(2^N- \lvert P \rvert)}{ \lvert P \rvert  2^N - \lvert P \rvert} = \frac{2^N- \lvert P \rvert}{  2^N - 1} $$

This validates our assumptions that the smaller the plausible states, the more data issues we can catch. 

Let's try this equation on our lawn system with the dummy variables. 

We recall: 

|Var| Definition|
|-|-|
$$T_1$$| The *true* state of the lawn.
$$X_1$$| The eldest's
$$T_2$$| Another 'dummy' variable representing the true state of the lawn.
$$X_2$$| The youngest's answer about the state of the lawn.
$$g_1$$| $$T_1=T_2$$

Therefore 

$$N=2$$,

$$P = \{(T_1 = True, T_2=True), (T_1 = False, T_2=False)\} $$

$$\lvert P \rvert = 2$$ 

$$2^N = 4$$

Fraction of Quality Issues Detected = $$\frac{2^N- \lvert P \rvert}{  2^N - 1} = \frac{2}{3} $$, in accordance with what we observed above. 


# Summary

We looked at discrete systems and found that the *only* way to detect data quality was by feeding our real world assumptions onto the data. A data quality alert is best when it simply takes all of the real world constraints in our system, and checks to see if any has been violated. We derived a simple equation for data quality coverage in discrete systems so that we know longer have to count truth tables. In accordance with our intuition, as the number of plausible states decreases, the coverage of data quality increases. 

This theory is simple and intuitive. It suggests a business process where real world constraints are systematically gathered in an organization and turned into a *maximal alert*. 

Thanks for reading, and please reach out with any suggestions or ideas. 










