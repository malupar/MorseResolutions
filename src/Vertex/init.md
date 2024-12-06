# Vertex inicialization

### __Vertex__ *(coordCube: list[int], coordBase: list[int], id: int, gen: int, vars: list[str])*

Initializes the vertex with a set of coordinates and a value.

## Parameters

*__coordCube__* : List of strings or integers, optional

<div style="margin-left: 30px;">
 Needed if the <i>base</i> is a list of strings and will express the variables in which each monomial will be expressed in.
</div>


*__coordBase__* : List of strings, optional

<div style="margin-left: 30px;">
 Needed if the <i>base</i> is a list of strings and will express the variables in which each monomial will be expressed in.
</div>

*__id__* : Integer

<div style="margin-left: 30px;">
Integer to express in which step this vertex was added to the hypercube.
</div>

*__gen__* : Integer

<div style="margin-left: 30px;">
In case there has been a splitting in the base of the ideal, it identifies if the vertex belongs to the first part, the second or the intersection.
</div>

*__vars__* : List of strings, optional

<div style="margin-left: 30px;">
List of variables to use when representing the value of the vertex.
</div>

## Implementation

```python
def __init__(self, coordCube: list[int], coordBase: list[int], id: int, gen: int, vars: list[str] = []) -> None:
    """Initializes one instance of the vertex class
    Parameters
    ----------
        coordCube : list
        list that indicates the coordinate that this vertex has on the hypercube
        coordBase : list
        list that indicates the exponents of the monomials
        id : int
        integer that indicates to what cube this vertex
        gen : int
        integer that indicates to what part of the pruning this vertex belongs
    """
    self.vars = vars
    if vars == []:
        self.vars = ["x"+str(i) for i in range(len(coordBase))]
    self.coordCube = np.array(coordCube, dtype=np.int32)
    self.coordBase = np.array(coordBase, dtype=np.int32)
    self.id: int = id
    self.gen: int = gen
```

## Examples

```python
v = Vertex([1, 0, 1], [1, 2, 0, 1], 1, 0)
```
