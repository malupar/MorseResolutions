# Ideal exponentiation

### Ideal.__exp__ *(exponent: int)*

Computes the ideal raised to a certain exponent.

## Parameters

*__exponent__* : Integer

<div style="margin-left: 30px;">
 The power to which this ideal is raised.
</div>

## Implementation
```python
def __pow__(self, power):
    """Override ** function"""
    I = Ideal(self.exp(power), self.vars)
    return I

def exp(self, exponent: int) -> list[list[int]]:
    """ Exponentiate this ideal to an exponent - A recursive binary exponentiation
    Parameters
    ----------
        exponent : int
        integer to which we elevate this ideal
    """

    if exponent == 1:
        return self.baseIdeal
    half = self.exp(exponent//2)
    numMonomials, _ = half.shape
    cur = np.array([half[i,:]+half[j,:] for i in range(numMonomials) 
                    for j in range(i, numMonomials)])
    if exponent&1:
        cur = np.array([i+j for i in cur for j in self.baseIdeal])
    self.getLI()
    return cur
```
## Examples

```python
I = Ideal(["xy", "xz", "yz"], ["x", "y", "z"])
J = I.exp(2)
```
```python
I = Ideal(["xy", "xz", "yz"], ["x", "y", "z"])
J = I**3
```