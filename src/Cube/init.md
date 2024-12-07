# Initialize a cube

### __Cube__ *(coordCube: list[int], coordBase: list[int], id: int, gen: int, vars: list[str])*

Initializes the cube with a set of coordinates and a value.

## Parameters

*__numVars__* : Integer, optional

<div style="margin-left: 30px;">
 Number of variables in which we will express each monomial.
</div>


*__dimBase__* : Integer, optional

<div style="margin-left: 30px;">
 Number of monomials of the initial ideal.
</div>

*__gens__* : List of monomials, optional

<div style="margin-left: 30px;">
List of monomials that generate the initial ideal.
</div>

## Implementation

```python
def __init__(self, typeGen = 1, numVars = None, dirs = None, other_cube = None, newStart = None, dimBase = None, id = None, gens = None):
    if typeGen == 1:
        self.numVars = numVars
        self.dimBase = dimBase
        self.dirs = dirs
        self.nodes = []
        self.gens = gens
        self.to = self.nodes[1:4]
        self.generateCube()
        self.id = 0
    else:
        self.numVars = other_cube.numVars
        self.dimBase = other_cube.dimBase
        self.id = id
        self.dirs = other_cube.dirs
        self.nodes = list(other_cube.nodes)
        self.setNewStart(newStart)
        self.gens = other_cube.gens
```

## Examples

```python
c = Cube([1, 0, 1], [1, 2, 0, 1], 1, 0)
```
