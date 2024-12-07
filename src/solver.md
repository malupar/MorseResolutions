# Solver - variant1

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
class Solver:
    """
    A class that represents a Solver

    Attributes
    ----------
    numVars : int
      A list of lists that represent the coordinates of the vectors that generate this ideal
    variables : list
    base : list
    dimBase : int
    cubes : list
    totalNodes : list
    Resolution : list
    typeSolver : int
    solverA : Solver
    solverB : Solver
    solverC : Solver
    gens : list

    Methods
    -------
    checkEdges(step, invariant)
    PruningAlgorithm(printLogs, invariant)
    printAllCubes(fancy)
    collectResolution()
    getResolution()
    BettiTable(ret, PrintBet, PrintInd)
    printBettiTableSeparado()
    printResolution()
    partialResolution(part1, simplify, printGenerators, printIndLogs)
    """

    def __init__(self, baseIdeals, variables: list[str] = [], typeSolver: int = 1, idx: list[int] = []) -> None:
      if variables == []:
        variables = ["x"+str(i) for i in range(len(baseIdeals[0]))]
      self.numVars: int = len(variables)
      self.variables: list[str] = variables
      self.base: list[int]
      if type(baseIdeals[0]) == str:
        self.base = parse_base(baseIdeals, variables)
      else:
        self.base = baseIdeals
      self.dimBase = len(baseIdeals)
      self.cubes = []
      self.totalNodes = []
      self.Resolution = None
      self.solverA = None
      self.solverB = None
      self.solverC = None
      self.typeSolver: int = typeSolver

      if typeSolver == 1:
        self.gens = np.zeros((self.dimBase), dtype=np.int32)
      else:
        self.gens = np.array([1 if i+1 in idx else 2 for i in range(self.dimBase)], dtype=np.int32)

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
              stepErased = self.checkEdges(self.dimBase, True)

            if printLogs:
              print("Step "+str(it-1)+": We erase these pairs")
              for p in stepErased:
                print(p[0])
                print(p[1])
                print("---")

    def printAllCubes(self, fancy: bool = True) -> None:
        """ We print all remaining vertex aggrupating by all but the first three digits. 
        """

        for it in range(len(self.cubes)):
            print("This is cube number: "+str(it+1))
            self.cubes[it].print_all_nodes(fancy, self.variables)

    def collectResolution(self) -> None:
      """ We collect the remaining vertex after pruning all the edges
      """

      cnt = [[] for _ in range(self.dimBase+1)]
      for ver in self.totalNodes:
          ind = sum(ver.coordCube)
          cnt[ind].append(ver)
      self.Resolution = cnt
      while [] in cnt:
        cnt.remove([])

    def getResolution(self):
      """ Check
      Returns
      -------
        The free resolution of the ideal
      """

      if self.Resolution is None:
        self.collectResolution
      return self.Resolution

    def BettiTable(self, ret: bool = False, printBet: bool = True, printInd: bool = False):
      """ Check
      Parameters
      ----------
      ret : boolean, optional
        If true, returns betti table
      printBet : boolean, optional
        If true, prints betti table
      printInd : boolean, optional
        If true, prints the split betti tables as well

      Returns
      -------
      It can return the Betti table of the resolution
      """

      if self.typeSolver == 3:
        BettiA = self.solverA.BettiTable(ret=True, printBet=printInd)
        BettiB = self.solverB.BettiTable(ret=True, printBet=printInd)
        BettiC = self.solverC.BettiTable(ret=True, printBet=printInd)
        mxDeg = max(len(BettiA), len(BettiB), len(BettiC)-1)
        width = max(len(BettiA[0]), len(BettiB[0]), len(BettiC[0])+1)
        BettiTable = [[0]*width for _ in range(mxDeg)]

        for i in range(mxDeg):
          for j in range(width):
            if i < len(BettiA) and j < len(BettiA[0]):
              BettiTable[i][j] += BettiA[i][j]
            if i < len(BettiB) and j < len(BettiB[0]):
              BettiTable[i][j] += BettiB[i][j]
            if i+1 < len(BettiC) and j > 0 and j-1 < len(BettiC[0]):
              BettiTable[i][j] += BettiC[i+1][j-1]
        BettiTable[0][0] = 1
        if printBet:
          print("Total Betti table:")
          for i in range(mxDeg):
            for j in range(width):
              print('{0:<4}'.format(BettiTable[i][j] if BettiTable[i][j] != 0 else '.'), end = '')
            print("")
        if ret:
          return BettiTable
      else:
        if self.Resolution is None:
          self.collectResolution()

        mxDeg = max([1+sum(v.coordBase)-sum(v.coordCube) for v in self.totalNodes])
        width = max([sum(v.coordCube) for v in self.totalNodes])+1

        BettiTable = [[0]*width for _ in range(mxDeg)]
        for i in range(width):
          for j in range(len(self.Resolution[i])):
            BettiTable[sum(self.Resolution[i][j].coordBase)-sum(self.Resolution[i][j].coordCube)][sum(self.Resolution[i][j].coordCube)] += 1

        if printBet:
          print("Betti table:")
          for i in range(mxDeg):
            for j in range(width):
              print('{0:<4}'.format(BettiTable[i][j] if BettiTable[i][j] != 0 else '.'), end = '')
            print("")
        if ret:
          return BettiTable

    def printBettiTableSeparado(self) -> None:
      """ Prints Betti table after splitting the ideal
      """

      assert self.typeSolver == 2, "printBettiTableSeparado: Option only available for Solver number 2"
      if self.Resolution is None:
        self.collectResolution()
      mxDeg = max([1+sum(v.coordBase)-sum(v.coordCube) for v in self.totalNodes])
      width = max([sum(v.coordCube) for v in self.totalNodes])+1

      for k in range(1, 4):
        BettiTable = [[0]*width for _ in range(mxDeg)]

        for i in range(width):
          for j in range(len(self.Resolution[i])):
            if self.Resolution[i][j].gen == k:
              BettiTable[sum(self.Resolution[i][j].coordBase)-sum(self.Resolution[i][j].coordCube)][sum(self.Resolution[i][j].coordCube)] += 1
        print("Betti table "+str(k)+":")
        for i in range(mxDeg):
            for j in range(width):
              print('{0:<4}'.format(BettiTable[i][j] if BettiTable[i][j] != 0 else '.'), end = '')
            print("")

    def printResolution(self) -> None:
      """ We print the surviving vertex for this resolution
      """

      if self.typeSolver == 3:
        print("Resolution for first ideal A")
        self.solverA.printResolution()
        print("Resolution for second ideal B")
        self.solverB.printResolution()
        print("Resolution for intersection")
        self.solverC.printResolution()
      else:
        if self.Resolution is None:
          self.collectResolution()
        for i in range(len(self.Resolution)):
            if i > 0 and len(self.Resolution[i-1]) == 0 and len(self.Resolution[i]) == 0:
              continue
            print("List for " + str(i))
            print("Betti number: "+ str(len(self.Resolution[i])))
            for ver in self.Resolution[i]:
                print(ver)

    def partialResolution(self, part1, simplify: bool = True, printGenerators: bool = False, printIndLogs: bool = False) -> None:
      """
      Parameters
      ----------
        part1 : list[int]
          A list that is conformed by the index of the generators for the first ideal
        simplify : Boolean, optional
          A boolean that indicates whether we eliminate the extra generators for the intersection
        printGenerators : Boolean, optional
          A boolean that indicates whether we print the generators for each ideal
        printIndLogs : Boolean, optional
          A boolean that indicates whether we print the edges removed for all algorithms
      """

      I = Ideal(self.base, self.variables)
      A, B, C = I.generateSubpruning(part1, simplify)
      if printGenerators:
        PrintList(A.baseIdeal, self.variables)
        PrintList(B.baseIdeal, self.variables)
        PrintList(C.baseIdeal, self.variables)
      solver1 = Solver(A.baseIdeal, self.variables)
      solver1.PruningAlgorithm(printLogs = printIndLogs)
      solver2 = Solver(B.baseIdeal, self.variables)
      solver2.PruningAlgorithm(printLogs = printIndLogs)
      solver3 = Solver(C.baseIdeal, self.variables)
      solver3.PruningAlgorithm(printLogs = printIndLogs)
      self.solverA = solver1
      self.solverB = solver2
      self.solverC = solver3

    def pruneWithFaces(self, facets) -> list[int]:
      """
      Parameters
      ----------
        facets : list[list[int]]
          A list that is conformed by the different facets that generate the simplicial complex

      Returns
      -------
        list[int]
          A list of the binary representation for the coordinates of the cubes that belong to the simplicial complex, with 1 meaning they belong and 0 meaning they do not
      """

      validVertex = [0]*(2**self.dimBase)
      for i in range(len(facets)):
        target = sum([2**x for x in facets[i]])
        validVertex[target] = 1

      for i in range(2**self.dimBase-1, 0, -1):
        for j in range(self.dimBase):
          if i&(2**j) > 0:
            validVertex[i-2**j] |= validVertex[i]

      return validVertex
```