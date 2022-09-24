# Tipping Point in Optimal Rocket Paths

Often times the best way to understand a system is to watch it break down. Aspects of a system's internal logic that might be invisible to us under normal conditions can become apparent when we put that system under stress. 

In this post we are going to explore a very simple system - an 1d rocket programmed to find optimal flying patterns. We are going to put the rocket system under stress, asking it to do increasingly ambitious tasks and see how it responds. We will observe a tipping point in the behavior of the rocket.

## Rocket Optimization Problem


Let's start with a basic setup of the rocket system we will be studying. The rocket starts on the ground of a planet and needs to travel to a height $$H$$ in time $$T$$. It has mass $$m$$ and is under the influence of a gravitational force $$g$$.

Fuel is very scarce on the planet we are trying to escape, therefore, we choose to minimize work done by the rocket engine. 

In Summary: 

Choose an optimal $$F(t)>0$$ where $$\int Fds$$ is minimized given the rocket travels to height $$H$$ in time $$T$$ with weight $$m$$ under gravity $$g$$. 

As we increase $$H$$, and therefore give the rocket more work to do, we will see the rocket's behavior hit a tipping point when $$H \geq gT^2/2$$.

# The Projectile Point

We can use an insight about the problem to simplify the problem statement. 

A *projectile point* is any point where the engine is turned off for the rest of the path. In mathematical terms: 

**Definition:** *Projectile Point*


Any time $$t\in [0,T]$$ on a rocket path where $$s>t \implies F(s) = 0$$. 

...

We call this point the *projectile point* because at this point the rocket is only under the influence of gravity and is essentially a projectile. 

Note that any point after a projectile point is also a projectile point. Also every optimal path has a projectile point at $$T$$.



## Calculating Work on the Projectile Point

Since the engine is off after the projectile point,  we can measure the total work done by the engine by measuring work at any projectile point. 

Every projectile point has a velocity $$v_p$$, a height, $$h_p$$, and occurs at a time $$t_p$$. In order to measure the work of a projectile point we simply have: 

$$W(v_p,h_p,t_p) = m\frac{v_p^2}{2} + mgh_p$$.

Since we can calculate total work on the path by using projectile points, we no longer need to consider $$F$$. 

# Deriving Constraints

## The Crossing Constraint

What can we say about these projectile points? Given a projectile point $$(v_p,h_p,t_p)$$, the rockets inertia needs to carry it a distance $$H-h_p$$ in time $$T-t_p$$. Using basic projectile physics we have: 

$$H-h_p > v_p(T-t_p) - \frac{g(T-t_p)^2}{2}$$

Since we are interested in optimal paths which conserve fuel, we can assume equality:

**Definition:** *The Crossing Constraint*

$$H-h_p = v_p(T-t_p) - \frac{g(T-t_p)^2}{2}$$

We call this condition the *crossing constraint* because the satisfaction of this constraint implies the rocket crosses height $$H$$ just in time. 

## The Cruising Constraint

At $$(v_p,h_p,t_p)$$ we have: 

$$v_p= \int_0^{t_p} Fdt- gt_p$$

Lets define $$m(x)=\int_0^{x} Fdt$$. Since $$F>0$$ we have $$m$$ as monotonic.

Now we have:

$$h_p = gt_p^2/2+\int_0^{t_p} m(t)dt$$. 

Therefore,

$$v_p-gt_p = m(t_p) \geq \frac{\int_0^{t_p} m(t)dt}{t_p} = \frac{h_p - gt_p^2/2}{t_p}$$.

Where the inequality follows because $$m$$ is monotonic and therefore greater than it's average value at any point. 

Rearranging we get:

**Definition:** *The Cruising Constraint*

$$v_pt_p - \frac{gt_p^2}{2} \geq h_p$$

...

## Impulse Constraint

Taking our last two constraints, solving for $$h$$ and canceling we get: 

**Definition: *Impulse Constraint***

$$H+\frac{T^2}{2}-gTt_p \leq Tv_p$$

...
# Interpretation of the Impulse Constraint


The impulse constraint also describes a path itself, simply divide the both sides of the equation by $$T$$ and replace the inequality with equality: 

**Definition:** *Impulse Path*

$$v_i(t) = \frac{H}{T}+\frac{gT}{2}-gt$$

...

This path is only accelerated by gravity and therefore every point is a projectile point.

We have $$\int_0^Tvdt=D$$ so this satisfies the crossing constraint at every point.

The impulse path obviously solves the impulse constraint.
Since it solves impulse and crossing, it must also solve the cruising constraint (though you are welcome to check this for yourself). 

The impulse path is therefore a valid path where all of the acceleration happens in the first instant. The impulse constraint says that the projectile point of a path must travel at least as fast as the impulse path at that time. 

Remember, only the projectile points of a path are subject to these constraints. 

# Unconstrained Elevator Paths

We don't yet know if we will ever hit the minimum energy, which we know from basic physics is $$W_{min}=mgH$$. 

Let's see under what conditions we can achieve a minimum:

## Taking Gradients

We start by taking the gradient of the work function. First we use the crossing contraint to get partials of $$h$$, ending up with: 
$$h_v=-(T-t), h_t=v-g(T-t)$$

So, 

$$W_t=v-g(T-t), W_v=g(v-g(T-t))$$

These constraints are redundant, and setting either to 0 will yield: 

$$v_p=g(T-t)$$. 

Remember, this is just the velocity of a path at the projectile point. 

## Validity of Unconstrained Optimum

Let's see when this solution is valid. We plug $$v_p=g(T-t)$$ into our impulse equation, and after canceling we get: 

$$H< gT^2/2$$.

This is worth reflecting on, we will be able to hit minimum energy $$W_{min}=mgH$$ as long as $$H< gT^2/2$$. 

When $$H$$ gets too big, we will no longer be able to achieve minimum energy. But for now we will assume that $$W_{min}=mgH$$, and compute some optimal paths. 

It is interesting to note, that  $$H= gT^2/2$$ when $$T$$ is the time it would take for an object to fall from height $$H$$, in this way this is a very natural dimensionless tipping point. 

## Simplest Path
Since $$v_p=g(T-t_p)$$, we have $$t=\frac{gT}{a+g}$$. 

We also recall the crossing constraint:

$$H-h_p = v_p(T-t_p) - \frac{g(T-t_p)^2}{2}$$

Substituting $$(T-t)=v/g$$ and multiplying by $$mg$$ we get

$$mv_p^2/2+ mgh_p = mgH = W_{min}$$.  

This is no surprise, we expect unconstrained optimal paths to hit $$W_{min}$$.

Let's find a path with constant acceleration. 

If $$v=at, h=at^2/2$$, we by plugging into the above equation (and diving by $$m$$) we get 

$$a^2t^2/2+gat^2=gH$$, 

solving for $$a$$ with the quadratic equation we have: 

$$a=\frac{gH}{T^2g/2-H}$$. 

## Plotting the Simply Paths. 

<iframe src="https://www.desmos.com/calculator/y1dhvwiupd?embed" width="500" height="500" style="border: 1px solid #ccc" frameborder=0></iframe>



# Tipping Point

The above Diagram shows the acceleration of a simple path in red, and the time of the projectile point in $$t$$. In Black is the tipping point, where the acceleration of simple optimal paths go to infinity, and therefore the projectile point time becomes 0. 

We have now found and understood optimal paths that his minimum work, we now assume that  $$H< gT^2/2$$, and find optimal paths given the constraint.

# Constrained Elevator Paths

We now assume $$H>gT^2/2$$. 


## Guessing the Constrained Path

It seems that the period is getting smaller and smaller. A smart guess would be the impulse path itself, since this path represents all of the acceleration happening in the first minute, and satisfies all constraints. 

## Confirming the Constrained Path
We now prove the the impulse path indeed minimizes for $$H>gT^2/2$$. 

We being by recalling the problem statement: 

*Find the minimum $$W$$ path where both the cruising constraint, and the crossing constraint are satisfied at projectile points and  H>gT^2/2.* 

**Proposition:** *The cruising constraint is satisfied with equality when $$ H>gT^2/2$$ for optimal paths.* 

Proof: this is a combination of the following facts:
- $$W$$ is convex in $$h$$, $$v$$
- $$W_{min}$$ is not accessible while $$H>gT^2/2$$
- we only have one inequality constraint, we have the cruising constraint 

... 

Therefore the impulse constraint is satisfied with equality. 

**Proposition**: *Any path satisfying the crossing condition with a projectile point on the impulse path is a minimizer*

Proof: The impulse path is the path of an projectile, and therefore work is constant along the impulse curve as a function of $$(v,t,h)$$. Any solution with a point on the impulse curve shares a projectile point $$(v,t,h)$$ with the impulse path since the crossing condition lets us solve for $$h$$.

The impulse path itself is therefore a minimizer. 

## Work of the optimal constrainted path. 
Calculating work at $$t=0, we get$$
$$W(v_i(0), h_i(0), t_i(0))=mv_i(0)^2/2 = \frac{m}{2}(\frac{H}{T}+\frac{gT}{2})^2$$. 



# Work as a function of T


<iframe src="https://www.desmos.com/calculator/2moewqco59?embed" width="500" height="500" style="border: 1px solid #ccc" frameborder=0></iframe>
