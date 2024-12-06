# Check for prune

### Vertex.__check__ *(other)*

Checks if two vertex have the same value.

## Parameters

*__other__* : Vertex

<div style="margin-left: 30px;">
 The vertex that we are comparing with.
</div>

## Implementation
```python
def check(self, other) -> bool:
    """ Checks if the exponents of the monomials is the same between two vertex
    Parameters
    ----------
        other : Vertex

    Returns
    -------
        Boolean
            True if the monomials are the same and False otherwise
    """

    return (self.coordBase == other.coordBase).all()
```
## Examples

```python
u = Vertex([0, 1, 0], [1, 1, 0, 0], -1, 0)
v = Vertex([1, 0, 1], [0, 1, 1, 0], 0, 0)
v.check(u)
```
```python
u = Vertex([0, 1, 0], [0, 1, 1, 0], -1, 0)
v = Vertex([1, 0, 1], [0, 1, 1, 0], 0, 0)
v.check(u)
```