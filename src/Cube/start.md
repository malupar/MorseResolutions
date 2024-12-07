# Modify bottom vertex

### Vertex.__setNewStart__ *(newStart)*

Modifies all the vertex of a cube, setting the \\(i\\)-th coordinate of each vertex to 1, where the \\(i\\)-th monomial corresponds to newStart.

## Parameters

*__newStart__* : Vertex

<div style="margin-left: 30px;">
 The monomial that we will add to each vertex of the cube.
</div>

## Implementation
```python
def setNewStart(self, newStart):
    """ For a given cube, we add the respective vector to the position (0,0,0,...)
    Parameters
    ----------
    """

    for i in range(len(self.nodes)):
        self.nodes[i] = self.nodes[i].addVec(newStart)
        self.nodes[i].id = self.id
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