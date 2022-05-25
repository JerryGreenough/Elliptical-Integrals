# Elliptical-Integrals

## Jacobi's Elliptical Functions


Jacobi's elliptical functions are defined (for a parameter $k, \hspace 0 \leq k < 1$)as follows:

1) <b>Amplitude</b>

$$ \text{am}(x,k) = F^{-1}(k,x)  $$

2) <b>Sinus Amplitudinis</b>

$$ \text{sn} (x, k) = \text{sin}[\ \text{am}(x, k) \ \] $$

3) <b>Cosinus Amplitudinis</b>

$$ \text{cn} (x, k) = \sqrt{   1 - \text{sn}^2(x, k)} = \text{cos}[\ \text{am}(x, k) \ \] $$

4) <b>Delta Amplitudinis</b>

$$ \text{dn} (x, k) = \sqrt{1 - k^2 \ \text{sn}^2(x, k)}   $$

Note that the amplitude function is equivalent to the inverse of the incomplete elliptical integral of the first kind.
<br>
The elliptical functions are easily incorporated into a Python script with the help of the ```scipy``` library - for details see
[scipy/special/ellipj](https://docs.scipy.org/doc/scipy/reference/generated/scipy.special.ellipj.html). The ```ellipj``` function returns a
list of four values, each corresponding to one of the elliptical functions introduced above. As was the case with the elliptical integrals, 
it should be noted that ```m``` parameter used by
the ```ellipj``` function is equal to the square of the $k$ parameter used in the mathematical literature.

```
from scipy.special import ellipj as elliptical_fns

k = 0.5
x = 0.25

m = k**2

sncndn = elliptical_fns(x, m)
snxk = sncndn[0]  # sn(x, k)
cnxk = sncndn[1]  # cn(x, k)
dnxk = sncndn[2]  # dn(x, k)
amxk = sncndn[3]  # am(x, k)
```
