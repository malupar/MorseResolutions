# Ideal intersection

### Ideal.__getIntersection__ *(idealB : Ideal, simplify = False)*

## Parameters

*__idealB__* : Ideal

<div style="margin-left: 30px;">
Ideal with which we will intersect
</div>

*__simplify__* : Boolean, optional

<div style="margin-left: 30px;">
True if we keep the minimal base and False otherwise
</div>

## Implementation
```python
def intersect(self, idealB, simplify: bool = False):
    """ We intersect the base of this ideal and another and simplify if necessary
    Parameters
    ----------
        idealB : Ideal
        ideal with which we intersect this ideal
        simplify : Boolean, optional
        boolean True if we keep only the minimal generator and False otherwise

    Returns
    -------
        Ideal corresponding to the intersection of this ideal and idealB
    """

    ans = Ideal([lcm(x, y) for x in self.baseIdeal for y in idealB.baseIdeal])
    if simplify:
        ans.getLI()
    return ans
```
## Examples

```python
I = Ideal(["xy", "xz", "yz"], ["x", "y", "z"])
J = Ideal(["x", "xz"], ["x", "y", "z"])
I.intersect(J)
```
```python
I = Ideal(["xy", "xz", "yz"], ["x", "y", "z"])
J = Ideal(["x", "xz"], ["x", "y", "z"])
I.intersect(J, simplify = True)
```