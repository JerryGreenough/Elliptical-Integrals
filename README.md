# Elliptic Integrals

## Introduction

Elliptic integrals and functions arise in many areas of mathematics. For instance, the following ellipse:

$$ {{x}\over{a}}^2  + {{y}\over{b}}^2 \ = \ 1  $$

has a circumferential length given by:

$$ L \ = \  a \int_0^{2\pi} {{\sqrt{1-k^2 \text{sin}^2\phi} } }  \ {d\phi}  \quad \quad k^2 = {{a^2-b^2}\over{a^2}}   \quad \quad b^2 < a^2  $$

which is an incomplete elliptical integral of the second kind (see later definition).

Another example is provided by the pendulum equation, which 
for large osciallation angles $\theta$ is a differential equation given by:

$$ \ddot{\theta} + {{g}\over{L}}  \text{sin} (\theta) \ = \ 0 $$

The solution to this equation can be written in terms of one of Jacobi's elliptic functions (for details of the mathematics see [Mathematics of the Pendulum](http://jgxsoft.com/examples/pendulum.html)). The period of oscillation can be expressed in terms of the complete elliptic integral of the first kind.

Finally, elliptic integrals of both the first and second kind are used in the determination of the large-deflection lateral force/deflection profile of the free end of a cantilever beam (for details see Bisshopp & Drucker: "Large Deflection of Cantilever Beams", Quarterly of Applied Mathematics, 3 272-275 (1945)).

This article is intended as a quick reference to those who wish to incorporate elliptic integrals and functions into a Python script.

<p align="center">
    <!--<img src="https://raw.githubusercontent.com/JerryGreenough/Mushrooms/master/images/fly_agaric.jpg" width="782" height="444">-->
    <img src="sn.png" width="782" height="444"> 
</p>

<p align="center">
    <strong><small>The sinus amplitudinis function ($\text{sn}) for $k=1$ </small></strong>
</p>

## Elliptic Integral of the First Kind

The incomplete elliptic integral of the first kind is defined:

$$ F(k,x) = \int_0^x {{d\phi}\over{\sqrt{1-k^2 \text{sin}^2\phi}}} $$ 

The complete elliptic integral of the first kind is defined:

$$ F(k) = \int_0^{{\pi}\over{2}} {{d\phi}\over{\sqrt{1-k^2 \text{sin}^2\phi}}} $$ 


## Elliptic Integral of the Second Kind

The incomplete elliptic integral of the second kind is defined:

$$ E(k,x) = \int_0^x {{\sqrt{1-k^2 \text{sin}^2\phi} } \ {d\phi}} $$

The complete elliptic integral of the second kind is defined:

$$ E(k) = \int_0^{{\pi}\over{2}} {{\sqrt{1-k^2 \text{sin}^2\phi}} \ {d\phi}} $$ 

## Python Implementation

The elliptic integrals of both the first kind and second kind can be incorporated into a Python script with the help of the ```scipy``` library.
<br>For a descripton of the ```scipy``` functionality for the elliptic integral of the first kind see
[scipy/special/ellipk](https://docs.scipy.org/doc/scipy/reference/generated/scipy.special.ellipk.html).
<br>For details of the elliptic integral of the second kind see
[scipy/special/ellipe](https://docs.scipy.org/doc/scipy/reference/generated/scipy.special.ellipe.html). 

<br>Note that the ```m``` parameter used by
the ```ellipk``` and ```ellipe``` functions is equal to the square of the $k$ parameter that is used in the mathematical literature. <br>

Here is an example:- given an angle $\theta$ and a parameter $k$ calculate $\alpha$ (which is defined in terms of the elliptic integrals of the first kind):
$$ \alpha \ =  \ F(k) - F(k,\theta) $$
and $\delta$ (which is defined in terms of the elliptic integrals of the second kind)
$$ \delta \  =  \ 1 - {{2}\over{\alpha}}(E(k) - E(k,\theta)) $$

```
# Assume that k and theta are defined previously.

from scipy.special import ellipk as F         # complete elliptical integral of the first kind
from scipy.special import ellipkinc as Finc   # incomplete elliptical integral of the first kind
from scipy.special import ellipe as E         # complete elliptical integral of the second kind
from scipy.special import ellipeinc as Einc   # incomplete elliptical integral of the second kind

m = k**2  # Calculate the m parameter

alpha = F(m) - Finc(theta, m)
dell  = E(m) - Einc(theta, m)
    
delta = 1.0 - 2.0 * dell / alpha
```




## Jacobi's Elliptic Functions


Jacobi's elliptic functions are defined (for a parameter $k, \ 0 \leq k < 1$)as follows:

1) <b>Amplitude</b>

$$ \text{am}(x,k) = F^{-1}(k,x)  $$

2) <b>Sinus Amplitudinis</b>

$$ \text{sn} (x, k) = \text{sin}[\ \text{am}(x, k) \ \] $$

3) <b>Cosinus Amplitudinis</b>

$$ \text{cn} (x, k) = \sqrt{   1 - \text{sn}^2(x, k)} = \text{cos}[\ \text{am}(x, k) \ \] $$

4) <b>Delta Amplitudinis</b>

$$ \text{dn} (x, k) = \sqrt{1 - k^2 \ \text{sn}^2(x, k)}   $$

Note that the amplitude function is equivalent to the inverse of the incomplete elliptic integral of the first kind.
<br>
The elliptic functions are easily incorporated into a Python script with the help of the ```scipy``` library - for details see
[scipy/special/ellipj](https://docs.scipy.org/doc/scipy/reference/generated/scipy.special.ellipj.html). The ```ellipj``` function returns a
list of four values, each corresponding to one of the elliptic functions introduced above. As was the case with the elliptic integrals, 
it should be noted that the ```m``` parameter used by
the ```ellipj``` function is equal to the square of the $k$ parameter used in the mathematical literature.

```
from scipy.special import ellipj as elliptic_fns

k = 0.5
x = 0.35

m = k**2

sncndn = elliptic_fns(x, m)
snxk = sncndn[0]  # sn(x, k)
cnxk = sncndn[1]  # cn(x, k)
dnxk = sncndn[2]  # dn(x, k)
amxk = sncndn[3]  # am(x, k)
```

Let's test the return value corresponding to the $\text{am}$ amplitude function - ```amxk```. It should satisfy:

$$ F[\text{am}(x,k)] = x $$

where $F$ is the incomplete elliptic integral of the first kind.

```
from scipy.special import ellipkinc as Finc

y = Finc(amxk, m)

print(y)  # y should be 0.35
```
