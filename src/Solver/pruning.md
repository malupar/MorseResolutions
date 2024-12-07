# Pruning algorithm

### Solver.__PruningAlgorithm__ *(printLogs: bool = False, invariant: bool = False, validVertex: list[int] = [])*

## Parameters

*__printLogs__* : Boolean, optional

<div style="margin-left: 30px;">
 If True it prints the removed edges on standard output.
</div>

*__invariant__* : Boolean, optional

<div style="margin-left: 30px;">
 Whether we should check the edges with the mu-invariant rule.
</div>

*__validVertex__* : List of integers, optional

<div style="margin-left: 30px;">
  If a simplicial complex is being pruned, it contains the binary representation of the vertices that should be pruned
</div>

## Implementation

```python
def PruningAlgorithm(self, printLogs: bool = False, invariant: bool = False, validVertex: list[int] = []) -> None:
    """ Check
    Parameters
    ----------
    printLogs : Boolean, optional
        A boolean that indicates whether we print the edges removed for the algorithm or not
    invariant: Boolean, optional
        A boolean that indicates whether we do a second run of the algorithm with the mu-invariant variation
    validVertex : list[int], optional
        If a simplicial complex is being pruned, it contains the binary representation of the vertices that will be pruned
    """

    self.totalNodes += [Vertex([0]*self.dimBase, [0]*self.numVars, 0, 0, self.variables)]
    for it in range(0, self.dimBase):
        newGen = self.gens[it]
        newDir = Vertex([1 if i == it else 0 for i in range(self.dimBase)], self.base[it], -1, newGen)
        newVertex = [Vertex(self.totalNodes[j].coordCube, self.totalNodes[j].coordBase,
                            self.totalNodes[j].id, self.totalNodes[j].gen | newGen, self.variables)
                    for j in range(len(self.totalNodes))]
        newVertex = [v.addVec(newDir) for v in newVertex]
        self.totalNodes += newVertex
        stepErased = self.checkEdges(it+1, False)

        if invariant:
            stepErased += self.checkEdges(self.dimBase, True)

        if printLogs:
            print("Step "+str(it-1)+": We erase these pairs")
            for p in stepErased:
                print(p[0])
                print(p[1])
                print("---")
```

## Examples
```python
Ideal = I(["xy", "xz", "yz"], ["x", "y", "z"])
index = I.getPartition("x")
```