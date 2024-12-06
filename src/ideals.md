# Ideal's class

The class for representing ideals has different methods.

The only attribute that it holds is a list of coordinates (one per monomial that generate this ideal) that show the different exponents of the variables of the monomials.

| Methods | <div style="width:400px"> Description </div> |
|---------| ---------- |
| [Ideal](Ideal/init.md) | Initializes the ideal object |
| [exp](Ideal/exp.md) | Elevates this ideal to a certain power|
| [intersect](Ideal/intersect.md) | Returns the intersection between this ideal and another ideal|
| [getPartition](Ideal/get_partition.md) | Returns two partitions from this ideal based on a certain variable |
| [generatePartialExtremal](Ideal/partialExtremal.md) | Generates a partial extremal of a certain number of variables |
| [generateExtremal](Ideal/extremal.md) |Generates an extremal ideal of a certain number of variables |
| [generateSimplicialComplex](Ideal/simplicialComplex.md) | Generates a simplicial complex of a certain number of variables |
| [generateEdgesFaces](Ideal/edgesFaces.md) | Returns all the edges, faces and ... from the simplicial complex|


## Class implementation
```python
class Ideal:
    """
    A class that represents an ideal

    Attributes
    ----------
    self.baseIdeal : list
        A list of lists that represent the coordinates of the vectors that generate this ideal

    Methods
    -------
    exp(exponent)
    intersect(idealB, simplify)
    getPartition()
    generatePartialExtremal(numVars)
    generateExtremal(numVars)
    allCombinations(i, numVars)
    generateFi(j, i, s, q, r, curList)
    generateSimplicialComplex(r, q, simplify)
    generateEdgesFaces()
    getLI()
    generateSubpruning()
    """
    def __init__(self, baseIdeal = [], vars: list[str] = []):
        if len(baseIdeal) > 0 and type(baseIdeal[0][0]) == str:
            baseIdeal = parse_base(baseIdeal, vars)
        self.baseIdeal = baseIdeal

    def __pow__(self, power):
        I = Ideal(self.exp(power))
        I.getLI()
        return I

    def exp(self, exponent: int) -> list[list[int]]:
        """ Exponentiate this ideal to an exponent - A recursive binary exponentiation
        Parameters
        ----------
            exponent : int
            integer to which we elevate this ideal
        """

        if exponent == 1:
            return self.baseIdeal
        cur = self.exp(exponent//2)
        cur = [sumVecs(cur[i], cur[j]) for i in range(len(cur)) for j in range(i, len(cur))]
        if exponent&1:
            cur = [sumVecs(i, j) for i in cur for j in self.baseIdeal]

        return cur


    def intersect(self, idealB, simplify: bool = False):
        """ We intersect the base of this ideal and another and simplify if necessary
        Parameters
        ----------
            idealB : Ideal
            ideal with which we intersect this ideal
            simplify : Boolean, optional
            boolean True if we keep only the minimal generator and False otherwise

        Returns
        -------
            Ideal corresponding to the intersection of this ideal and idealB
        """

        ans = Ideal([lcm(x, y) for x in self.baseIdeal for y in idealB.baseIdeal])
        if simplify:
            ans.getLI()
        return ans

    def getPartition(self, variable = None) -> list[int]:
        """ We will return all the index that contain a particular variable
        Parameters
        ----------
        variable : str, optional

        Returns
        -------
        Set of all index that contain a particular variable
        """

        assert self.baseIdeal != [], "getPartition: Ideal cannot be empty"

        ans = []
        for i in len(self.baseIdeal[0]):
            found = False
            for j in range(len(self.baseIdeal)):
                if self.baseIdeal[j][i] > 0:
                    found = True
                    ans.append(j)
                    break
            if found:
                break
        return ans

    def generatePartialExtremal(self, numVars: int):
        """ We generate ...
        Parameters
        ----------
            numVars : int
        Returns
        -------
        list
            A list of ideals
        """

        PosVars = self.allCombinations(0, numVars)
        lstIdeals = [[PosVars[i] for i in range(len(PosVars)) if sum(PosVars[i]) == j+1] for j in range(numVars)]
        return lstIdeals

    def generateExtremal(self, numVars: int) -> None:
        """ We generate an extremal ideal
        Parameters
        ----------
            numVars : int
            integer that tells us
        """

        self.baseIdeal = [[0]*(2**numVars-1) for _ in range(numVars)]

        for i in range(1, 2**numVars):
            for j in range(numVars):
                if i&(2**j) > 0:
                    self.baseIdeal[j][i-1] = 1

    def allCombinations(self, i: int, numVars: int):
        """
        Parameters
        ----------
            i : int
            numVars : int

        Returns
        -------
        list
            List of all possible combinations
        """

        if i == numVars:
            return [[0]*numVars]
        lst = []
        for num in self.allCombinations(i+1, numVars):
            lst += [num, sumVecs(num,[1 if j == i else 0 for j in range(numVars)])]
        return lst

    def generateFi(self, j: int, i: int, s: int, q: int, r: int, curList):
        """
        Parameters
        ----------
            j : int
            i : int
            s : int
            q : int
            r : int
            curList : list

        Returns
        -------
        list
        """

        ans = []
        if j == len(curList) and sum(curList) < r:
            return []
        elif j == len(curList):
            return [curList]
        newList = list(curList)
        if j == i:
            for k in range(min(r-sum(curList), max(r-1, s))):
                ans += self.generateFi(j+1, i, s, q, r, newList)
                newList[j] += 1
        else:
            for k in range(min(r-sum(curList), s)):
                ans += self.generateFi(j+1, i, s, q, r, newList)
                newList[j] += 1
        return ans+self.generateFi(j+1, i, s, q, r, newList)

    def generateSimplicialComplex(self, r: int, q: int, simplify: bool = True):
        """
        Parameters
        ----------
            r : int
            q : int
            simplify : Boolean, optional

        Returns
        -------
        list
        faces
        """

        faces = []
        self.baseIdeal = []
        if r == 1 or q == 1:
            Nq = [[r if i == j else 0 for j in range(q)] for i in range(q)]
            faces += [Nq]
            self.baseIdeal = Nq
        else:
            s = (r+1)//2
            G = [[[r if (j == i and j == k) else 0 if i == j else r-1 if i == k else 1 if j == k else 0 for k in range(q)] for i in range(q)] for j in range(q)]
            self.baseIdeal += sum(G, [])
            faces += [G]
            if r > 3 and q >= 2:
                F = [self.generateFi(0, i, s, q, r, [0]*q) for i in range(q)]
                faces += F
                for i in range(q):
                self.baseIdeal += F[i]
            if (r == 3 and q >= 2) or (r == 2 and q > 2):
                B = self.generateFi(0, -1, s, q, r, [0]*q)
                self.baseIdeal += B
                faces += [B]

        if simplify:
            self.getLI()
        return faces

    def generateEdgesFaces(self):
        """
        Returns
        -------
        list, list

        """

        N = len(self.baseIdeal)
        adjMatrix = [[0]*N for i in range(N)]
        edges, faces = [], []
        for i in range(N):
            for j in range(i):
                lst = sumVecs(self.baseIdeal[i], [-self.baseIdeal[j][k] for k in range(len(self.baseIdeal[j]))])
                if sum(abs(lst[k]) for k in range(len(lst))) == 2:
                adjMatrix[i][j], adjMatrix[j][i] = 1, 1
                edges.append([i, j])

        for i in range(N):
            for j in range(i):
                for k in range(j):
                if adjMatrix[i][j] > 0 and adjMatrix[i][k] > 0 and adjMatrix[j][k] > 0:
                    faces.append([i, j, k])
        return edges, faces

    def getLI(self):
        """
        """

        found = []
        numVars = len(self.baseIdeal[0])
        exp = [max([self.baseIdeal[j][i] for j in range(len(self.baseIdeal))])+1 for i in range(numVars)]
        for i in range(1,numVars):
            exp[i] = exp[i-1]*exp[i]
        for i in range(exp[-1]):
            found += [[0]*len(self.baseIdeal)]

        found[0] = [1]*len(self.baseIdeal)
        toRem = []
        pot = [1]+exp[:numVars-1]
        dec = [1]+exp

        for j in range(len(self.baseIdeal)):
            num = sum(multVecs(pot, self.baseIdeal[j]))
            if found[num][j] > 0:
                toRem.append(j)
                continue
            for i in range(exp[-1]):
                enc = [0]*numVars
                for k in range(numVars):
                    enc[k] = (i//dec[k])%(dec[k+1]//dec[k])
                nextCoord = [max(self.baseIdeal[j][k], enc[k]) for k in range(numVars)]
                to = sum(multVecs(pot, nextCoord))

                found[to] = [1 if (found[to][k] == 1 or (found[i][k] == 1 and k != j)) else 0 for k in range(len(self.baseIdeal))]

        for i in range(len(self.baseIdeal)):
            if i not in toRem:
                num = sum(multVecs(pot, self.baseIdeal[i]))
                if found[num][i] == 1:
                    toRem.append(i)

        self.baseIdeal = [self.baseIdeal[j] for j in range(len(self.baseIdeal)) if j not in toRem]
        self.baseIdeal = sorted(self.baseIdeal, key=lambda x: sum(x))

    def generateSubpruning(self, parts, simplify:bool = False):
        """
        Parameters
        ----------
            parts : list
            List of integers that contains the index of one ideal
            simplify : Boolean, optional
            Boolean that

        Returns
        -------
        Ideal, Ideal, Ideal
            Three ideals I, J and the intersection of I and J
        """

        A = Ideal([self.baseIdeal[j] for j in range(len(self.baseIdeal)) if j+1 in parts])
        B = Ideal([self.baseIdeal[j] for j in range(len(self.baseIdeal)) if j+1 not in parts])
        C = A.intersect(B, simplify)
        return A, B, C

    def stressSplitting(self, parts, n: int, simplify: bool = False):
        """
        Parameters
        ----------
            parts : list
            List of integers that contains the index of one ideal
            simplify : Boolean, optional
            Boolean that

        Returns
        -------
        Ideal, Ideal, list(Ideal)
            Three ideals I, J and several intersections of I and J
        """

        A = Ideal([self.baseIdeal[j] for j in range(len(self.baseIdeal)) if j+1 in parts])
        B = Ideal([self.baseIdeal[j] for j in range(len(self.baseIdeal)) if j+1 not in parts])

        C = [A.intersect(B, simplify, i) for i in range(n)]
        return A, B, C
```
