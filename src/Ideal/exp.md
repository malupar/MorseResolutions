# Ideal exponentiation

### Ideal.__exp__ *(exponent: int)*

## Parameters

*__exponent__* : Integer

<div style="margin-left: 30px;">
The integer to which we will elevate this ideal
</div>

## Implementation
```python
def exp(self, exponent: int) -> list[list[int]]:
    """ Exponentiate this ideal to an exponent - A recursive binary exponentiation
    Parameters
    ----------
        exponent : int
            integer to which we elevate this ideal
    """

    if exponent == 1:
        return self.baseIdeal
    cur = self.exp(exponent//2)
    cur = [sumVecs(cur[i], cur[j]) for i in range(len(cur)) for j in range(i, len(cur))]
    if exponent&1:
        cur = [sumVecs(i, j) for i in cur for j in self.baseIdeal]
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