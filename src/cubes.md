# Cube class

The class for representing a vertex has different attributes.

| Attributes | <div style="width:400px"> Description </div> |
|---------| ---------- |
| numVars | Initializes the vertex object |
| dimBase| Adds a certain monomial to this vertex and modifies the coordinates and value respectively|
| dirs | Checks whether two vertex have the same value |
| nodes | Checks whether two vertex have the same value or it is off by one (\\(\mu-invariant\\)) |
| gens | Checks whether two vertex have the same value or it is off by one (\\(\mu-invariant\\)) |
| id | Checks whether two vertex have the same value or it is off by one (\\(\mu-invariant\\)) |

The methods that have been implemented are:

| Methods | <div style="width:400px"> Description </div> |
|---------| ---------- |
| [Cube](Cube/init.md) | Initializes the cube object |
| [copyCube](Cube/start.md) | Adds a certain monomial to this cube and modifies the coordinates and value respectively|

## Implementation

```python
class Cube:
    """
    A class that represents a Cube

    Attributes
    ----------
    numVars : int
        A list of lists that represent the coordinates of the vectors that generate this ideal
    dimBase : int
    dirs : list
    nodes : list
    gens : list
    to : list
    id : int

    Methods
    -------
    setNewStart(newStart)
    generateCube()
    print_all_nodes(fancy, vars)
    """

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

    def setNewStart(self, newStart):
        """ For a given cube, we add the respective vector to the position (0,0,0,...)
        Parameters
        ----------
        """

        for i in range(len(self.nodes)):
            self.nodes[i] = self.nodes[i].addVec(newStart)
            self.nodes[i].id = self.id

    def generateCube(self):
        """  We generate the first cube
        Parameters
        ----------
        """

        for i in range(2):
            for j in range(2):
                for k in range(2):
                    coord = [0]*self.dimBase
                    coord[0], coord[1], coord[2] = i,j,k
                    coord2 = [0]*self.numVars
                    cur_gen = 0
                    if i > 0:
                        coord2 = lcm(coord2, self.dirs[0])
                        cur_gen |= self.gens[0]
                    if j > 0:
                        coord2 = lcm(coord2, self.dirs[1])
                        cur_gen |= self.gens[1]
                    if k > 0:
                        coord2 = lcm(coord2, self.dirs[2])
                        cur_gen |= self.gens[2]
                    self.nodes.append(Vertex(coord, coord2, 0, cur_gen))

    def print_all_nodes(self, fancy = True, vars = []):
        """ We print all the nodes in a cube
        Parameters
        ----------
        """

        for i in range(len(self.nodes)):
            self.nodes[i].PrintVer(fancy, vars)
```