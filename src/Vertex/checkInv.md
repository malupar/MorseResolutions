# Check for invariant prune

### Vertex.__checkInv__ *(other)*

Checks if two vertex have the same value under \\(\mu\\)-invariant.

## Parameters

*__other__* : Vertex

<div style="margin-left: 30px;">
 The vertex that we are comparing with.
</div>

## Implementation
```python
def checkInv(self, other) -> bool:
    """ Checks if the exponents of the monomials differns in 1 between two vertex
    Parameters
    ----------
        other : Vertex

    Returns
    -------
        Boolean
            True if the monomials differs in 1 and False otherwise
    """

    return np.sum(self.coordBase)+1 == np.sum(other.coordBase)
```
## Examples

```python
u = Vertex([0, 1, 0], [1, 1, 0, 0], -1, 0)
v = Vertex([1, 0, 1], [0, 1, 1, 0], 0, 0)
v.check(u)
```
```python
u = Vertex([0, 1, 0], [0, 1, 1, 0], -1, 0)
v = Vertex([0, 1, 1], [0, 1, 1, 1], 0, 0)
v.check(u)
```