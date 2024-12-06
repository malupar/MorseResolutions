# Generate a simplicial complex

### Ideal.__generateSimplicialComplex__ *(r, q)*

Generates a cimplicial complex ideal with parameters *r* and *q*.

> __Definition__ : Let \\(r, q \geq 1\\) be two integers . We define \\(\mathbb{L_q^r}\\) to be the simplicial complex with vertex set \\(\mathcal{N_q^r}\\) whose faces are all subsets of the sets \\(\mathit{F_1^r,...,F_q^r,G_1^r,...,G_q^r}\\) defined as
\\[\mathit{F_i^r} =\lbrace (a_1,...,a_q) \in \mathcal{N_q^r} : a_i \leq max \lbrace r-1, s\rbrace \text{ and } a_j \leq s \text{ for } j \neq i\rbrace\\]
\\[\mathit{G_i^r} =\lbrace (a_1,...,a_q) \in \mathcal{N_q^r} : a_i \geq  r-1\rbrace = \lbrace (r-1)e_i+e_j : j \in \[q\] \rbrace\\]
for each \\(i \in \[q\]. \text{ Here } \mathcal{N_q^r} = \lbrace (a_1, ..., a_q) \in \mathbb{Z_{\geq 0}^q} : a_1 + ... + a_q = r\rbrace\\)
## Parameters

*__r__* : Integer

<div style="margin-left: 30px;">
 The sum of all the exponents should equal to <i>r</i> for each monomial of the base.
</div>

*__q__* : Integer

<div style="margin-left: 30px;">
 The number of variables of this simplicial complex.
</div>

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