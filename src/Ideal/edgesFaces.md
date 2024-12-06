# Generate a edges and faces from ideal

### Ideal.__generateEdgesFaces__ *()*

## Implementation

```python
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
```

## Examples

```python
I = Ideal(simplicialComplex = True, r = 2, q = 5)
edges, faces = I.generateEdgesFaces()
```