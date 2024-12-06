# Vertex class

The class for representing a vertex has different attributes.

| Attributes | <div style="width:400px"> Description </div> |
|---------| ---------- |
| coordCube | Initializes the vertex object |
| coordBase| Adds a certain monomial to this vertex and modifies the coordinates and value respectively|
| id | Checks whether two vertex have the same value |
| gen | Checks whether two vertex have the same value or it is off by one (\\(\mu-invariant\\)) |

The methods that have been implemented are:

| Methods | <div style="width:400px"> Description </div> |
|---------| ---------- |
| [Ideal](Vertex/init.md) | Initializes the vertex object |
| [addVec](Vertex/addVec.md) | Adds a certain monomial to this vertex and modifies the coordinates and value respectively|
| [check](Vertex/check.md) | Checks whether two vertex have the same value |
| [checkInv](Vertex/checkInv.md) | Checks whether two vertex have the same value or it is off by one (\\(\mu-invariant\\)) |


## Implementation

```python
class Vertex:
    """
    A class that represents a vertex

    Attributes
    ----------
    coordCube : list
    A list of lists that represent the coordinates of the vectors that generate this ideal
    coordBase : list
    id : int
    gen : int

    Methods
    -------
    addVec(coordCube, coordBase, id, gen)
    check(other)
    checkInv(other)
    PrintVer(fancy, vars)
    == (__eq__)
    """

    def __init__(self, coordCube: list[int], coordBase: list[int], id: int, gen: int) -> None:
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

        self.coordCube: list[int] = coordCube
        self.coordBase: list[int] = coordBase
        self.id: int = id
        self.gen: int = gen

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

        return Vertex(sumVecs(self.coordCube,x.coordCube), lcm(self.coordBase,x.coordBase), self.id, self.gen | x.gen)

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

        return self.coordBase == other.coordBase

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

        return sum(self.coordBase)+1 == sum(other.coordBase)

    def __eq__(self, other) -> bool:
        """
        Checks if two objects are equal
        Parameters
        ----------
            other : Vertex

        Returns
        -------
            Boolean
            True if both coordinates are the same and False otherwise
        """

        return self.coordBase == other.coordBase and self.coordCube == other.coordCube

    def PrintVer(self, fancy: bool = True, vars: list[str] = []) -> bool:
        """ Prints a vertex
        Parameters
        ----------
            fancy : Boolean, optional
            Boolean that tells if we print the coordinates(False) or the monomial(True)
            vars : list, optional
            List of variables in which we express the "Fancy" output
        """

        print(self.coordCube, end=' ')
        if not fancy:
            print(self.coordBase)
        else:
            for j in range(len(vars)):
                if self.coordBase[j] > 0:
                    print(vars[j], end='')
                    if self.coordBase[j] > 1:
                        print("^"+str(self.coordBase[j]), end='')
            print("", end = ' ')

        print(self.gen)
```