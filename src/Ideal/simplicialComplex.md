# Generate a simplicial complex

### Ideal.__generateSimplicialComplex__ *(r, q)*

## Parameters

*__r__* : Integer

*__q__* : Integer

## Implementation

```python
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
```

## Examples

```python
I = Ideal()
I.generateSimplicialComplex(2, 5)
```

```python
I = Ideal(simplicialComplex = True, r = 2, q = 5)
```