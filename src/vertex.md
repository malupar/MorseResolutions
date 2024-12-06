# Vertex class

The class for representing a vertex has different attributes.

| Attributes | <div style="width:400px"> Description </div> |
|---------| ---------- |
| coordCube | Coordinates of this vertex in the hypercube |
| coordBase| Coordinates of the value of this vertex with respect to the variables |
| id | Integer that identifies in which step of the algorithm this vertex was added |
| gen | Integer to identify to if this vertex belong to \\(A, B \text{ or } A\bigcap B\\) if the ideal for a resolution has been partitioned into two parts |
| vars | Variables used to represent *coordBase* |

The methods that have been implemented are:

| Methods | <div style="width:400px"> Description </div> |
|---------| ---------- |
| [Vertex](Vertex/init.md) | Initializes the vertex object |
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
    """

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

    def __repr__(self):
      rep = ""
      rep += np.array2string(self.coordCube) + " "
      for j in range(len(self.vars)):
          if self.coordBase[j] > 0:
              rep += self.vars[j]
              if self.coordBase[j] > 1:
                  rep += "^" + str(self.coordBase[j])
      rep += " " + str(self.gen)
      return rep

    def __str__(self):
      rep = ""
      rep += np.array2string(self.coordCube) + " "
      for j in range(len(self.vars)):
          if self.coordBase[j] > 0:
              rep += self.vars[j]
              if self.coordBase[j] > 1:
                  rep += "^" + str(self.coordBase[j])
      rep += " " + str(self.gen)
      return rep

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

        return Vertex(self.coordCube+x.coordCube, np.maximum(self.coordBase,x.coordBase), self.id, self.gen | x.gen)

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

        return (self.coordBase == other.coordBase).all() and (self.coordCube == other.coordCube).all()
```