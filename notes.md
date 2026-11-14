# Buckingham $\Pi$ Theorem

The **Buckingham $\Pi$ Theorem** is a method for simplifying relationships between physical quantities using dimensional analysis.

If a physical relationship involves $n$ dimensional quantities built from $k$ independent fundamental dimensions, it can be expressed using $n-k$ independent dimensionless groups, called **$\Pi$ groups**. The number of groups may be smaller if the quantities' dimensions are not independent.

For example, the fundamental dimensions commonly used in mechanics are mass ($M$), length ($L$), and time ($T$).

## Why use it?

- **Reduce complexity:** Replace many dimensional variables with a smaller set of dimensionless groups.
- **Check dimensional consistency:** Physical relationships must be consistent across unit systems.
- **Establish scaling laws:** Use results from a model, such as a wind-tunnel test, to predict full-scale behavior when the relevant dimensionless groups match.

## How to apply the theorem

1. List the dimensional quantities that affect the phenomenon; let their number be $n$.
2. Express each quantity in terms of fundamental dimensions and determine the rank $k$ of the resulting dimension matrix.
3. Form $n-k$ independent dimensionless groups.
4. Express the physical relationship in terms of those groups.

## Simple example: vertical motion under gravity

For an object launched vertically upward with initial speed $u$, its displacement $y$ after time $t$ is

$$
y = ut - \frac{1}{2}gt^2,
$$

assuming the initial displacement is zero and air resistance is negligible. This follows by integrating the constant-acceleration equation $\frac{d^2y}{dt^2}=-g$ with initial velocity $u$.

The variables have dimensions

$$
[y]=L,\qquad [u]=LT^{-1},\qquad [t]=T,\qquad [g]=LT^{-2}.
$$

There are $n=4$ quantities and $k=2$ independent dimensions (length and time), so the theorem gives two dimensionless groups. One possible choice is

$$
\Pi_1 = \frac{y}{gt^2},
\qquad
\Pi_2 = \frac{u}{gt}.
$$

Dividing the original equation by $gt^2$ gives the dimensionless relationship

$$
\Pi_1 = \Pi_2 - \frac{1}{2}.
$$

The theorem tells us the equation can be expressed in terms of dimensionless groups; the specific relationship and the factor $\frac{1}{2}$ come from the kinematics equation.

### Plotting the dimensionless relationship

Plot $\Pi_1$ against $\Pi_2$. The equation predicts a straight line, $\Pi_1=\Pi_2-\frac{1}{2}$. Different launch speeds and gravitational accelerations give the same dimensionless line, so separate panels make the overlapping cases easier to read. The points are evenly spaced in $\Pi_2$, rather than in time:

```python
import numpy as np
import matplotlib.pyplot as plt

cases = [(10.0, 9.81), (20.0, 9.81), (10.0, 1.62)]  # (u in m/s, g in m/s^2)
fig, axes = plt.subplots(3, 1, figsize=(7, 10), sharex=True, sharey=True,
                         constrained_layout=True)

pi2_line = np.linspace(0.5, 2.0, 200)
pi2_samples = np.linspace(0.5, 2.0, 7)

for ax, (u, g) in zip(axes, cases):
    t = u / (g * pi2_samples)
    y = u * t - 0.5 * g * t**2
    pi1 = y / (g * t**2)
    pi2 = u / (g * t)
    ax.plot(pi2_line, pi2_line - 0.5, "k--", label="Prediction")
    ax.plot(pi2, pi1, "o", markersize=7, label="Calculated")
    ax.set_title(f"u = {u:g} m/s, g = {g:g} m/s²")
    ax.set_ylabel(r"$\Pi_1 = y/(gt^2)$")
    ax.grid(True, alpha=0.3)

axes[0].legend()
axes[-1].set_xlabel(r"$\Pi_2 = u/(gt)$")
plt.show()
```

Time $t=0$ is excluded because both groups contain $t$ in the denominator. The chosen range shows the part of each flight from $t=u/(2g)$ until the object returns to its starting height at $t=2u/g$. Each panel shows the same dimensionless relationship.

## Example: drag on an object

Suppose the drag force $F$ depends on fluid velocity $v$, object size $L$, fluid density $\rho$, and dynamic viscosity $\mu$:

| Quantity | Dimensions |
|---|---|
| Drag force, $F$ | $[M L T^{-2}]$ |
| Velocity, $v$ | $[L T^{-1}]$ |
| Size, $L$ | $[L]$ |
| Density, $\rho$ | $[M L^{-3}]$ |
| Dynamic viscosity, $\mu$ | $[M L^{-1} T^{-1}]$ |

There are $n=5$ quantities and $k=3$ independent dimensions ($M$, $L$, and $T$), so there are $5-3=2$ independent dimensionless groups. One possible choice is:

$$
\Pi_1 = \frac{F}{\rho v^2 L^2},
\qquad
\Pi_2 = \frac{\rho v L}{\mu} = \mathrm{Re}.
$$

Here, $\Pi_1$ is a dimensionless drag coefficient (its precise conventional definition may include a constant factor), and $\mathrm{Re}$ is the Reynolds number. The relationship can therefore be written as $\Pi_1 = \Phi(\mathrm{Re})$ for some function $\Phi$.

## Should gravity be included?

Include gravity, represented by acceleration $g$, when it can affect the phenomenon. Whether a quantity is constant during a particular experiment does not determine whether it belongs in the dimensional analysis: what matters is whether it is relevant to the physical relationship.

For example, if gravity affects the drag problem, add $g$ to the variable list. The set then has $n=6$ quantities and still uses the three dimensions $M$, $L$, and $T$, giving three independent dimensionless groups. One possible additional group is

$$
\Pi_3 = \frac{gL}{v^2},
$$

which is the inverse square of the Froude number under the convention $\mathrm{Fr}=v/\sqrt{gL}$. If gravity has no meaningful effect on the problem, it should not be included.

## What belongs in the variable list?

Include dimensional quantities that may influence the outcome, such as:

- **Motion and forces:** velocity ($v$), force ($F$), pressure ($p$), or frequency ($\omega$).
- **Geometry:** length ($L$), diameter ($D$), or surface roughness ($\epsilon$).
- **Material and environmental properties:** density ($\rho$), viscosity ($\mu$), surface tension ($\sigma$), gravity ($g$), or elastic modulus ($E$).

Do not count pure numbers, such as $2$ or $\pi$, as dimensional quantities. Dimensionless ratios may still be useful in the final description, but they are not additional dimensional variables.
