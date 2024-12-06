# Add a vector to a vertex

### Vertex.__addVec__ *(x)*

Adds a certain monomial from the base to a vertex.

## Parameters

*__x__* : Vertex

<div style="margin-left: 30px;">
 The monomial from the base that is being added.
</div>

## Implementation
```python
def addVec(self, x):
    """ We add x to the node
    Parameters
    ----------
        x : Vertex
        other vertex that we add

    Returns
    -------
        Vertex

    """

    assert len(self.coordCube) == len(x.coordCube), "addVec: Both vectors should have same length"

    return Vertex(self.coordCube+x.coordCube, np.maximum(self.coordBase,x.coordBase), self.id, self.gen | x.gen, self.vars)
```
## Examples

```python
u = Vertex([0, 1, 0], [1, 1, 0, 0], -1, 0)
v = Vertex([1, 0, 1], [0, 1, 1, 0], 0, 0)
v.addVec(u)
```