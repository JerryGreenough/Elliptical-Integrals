# Elliptic Integrals

## Jacobi's Elliptic Functions


Jacobi's elliptic functions are defined (for a parameter $k, \, 0 \leq k < 1$)as follows:

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
list of four values, each corresponding to one of the elliptical functions introduced above. As was the case with the elliptic integrals, 
it should be noted that ```m``` parameter used by
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
