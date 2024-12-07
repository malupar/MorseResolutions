# Pruning algorithm

### Solver.__checkEdges__ *(step, invariant: bool = False, validVertex: list[int] = [])*

## Parameters

*__step__* : Integer

<div style="margin-left: 30px;">
 The step of the algorithm in which we are (the number of monomials that have been added).
</div>

*__invariant__* : Boolean, optional

<div style="margin-left: 30px;">
 Whether we should check the edges with the \\(\mu\\)-invariant rule.
</div>

*__validVertex__* : List of integers, optional

<div style="margin-left: 30px;">
  If a simplicial complex is being pruned, it contains the binary representation of the vertices that should be pruned
</div>

## Implementation

```python
def checkEdges(self, step, invariant: bool = False, validVertex: list[int] = []):
    """ Checks if the exponents of the monomials is the same between two vertex
    Parameters
    ----------
        step : int
        invariant : Boolean
        validVertex : list[int]

    Returns
    -------
        list[class(vertex)]
            A list of all the vertex that have been deleted during this step
    """

    toErase = []
    pairsErased = []
    for it in range(step):
    newDir = Vertex([1 if i == it else 0 for i in range(self.dimBase)], self.base[it], -1, self.gens[it])
    for j in range(len(self.totalNodes)):
        nextNode = self.totalNodes[j].addVec(newDir)
        curNode = self.totalNodes[j]
        pos = -1
        if nextNode in self.totalNodes:
            pos = self.totalNodes.index(nextNode)
        if pos != -1:
            nextNode = self.totalNodes[pos]

        curNum = sum([2**k for k in range(self.dimBase) if curNode.coordCube[k] == 1])
        nextNum = sum([2**k for k in range(self.dimBase) if nextNode.coordCube[k] == 1])

        if validVertex != [] and validVertex[curNum] == 0:
            toErase.append(curNode)
            continue
        if validVertex != [] and validVertex[nextNum] == 0:
            toErase.append(nextNode)
            continue

        if curNode not in toErase and nextNode not in toErase and nextNode in self.totalNodes and (curNode.check(nextNode) or (invariant and curNode.checkInv(nextNode))) and curNode.gen == nextNode.gen:
            toErase.append(nextNode)
            toErase.append(curNode)
            pairsErased.append([curNode, nextNode])

    for node in toErase:
        self.totalNodes.remove(node)

    return pairsErased
```

## Examples
```python
Ideal = I(["xy", "xz", "yz"], ["x", "y", "z"])
index = I.getPartition("x")
```