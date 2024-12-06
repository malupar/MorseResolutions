# Generate an extremal ideal

### Ideal.__generateExtremal__ *(numVars : int)*

Generates an extremal ideal with a certain number of variables.

> Definition: Let \\(q\\) be a positive integer. For every set \\(A\\) with \\(\emptyset \neq A \subset \[q\]\\), introduce a variable \\(x_A\\), and then set \\(S_{\mathcal{E}}\\) be the polynomial ring over a field \\(K\\) shown below:
\\[S_{\mathcal{E}} = K \lbrack x_A |\text{ } \emptyset \neq A \subset \lbrack q\rbrack \rbrack\\]

## Parameters

*__numVars__* : Integer

<div style="margin-left: 30px;">
 The number of variables of this extremal ideal.
</div>

## Implementation
```python
def generateExtremal(self, numVars: int) -> None:
    """ We generate an extremal ideal
    Parameters
    ----------
        numVars : int
            Total number of variables
    """

    self.baseIdeal = [[0]*(2**numVars-1) for _ in range(numVars)]

    for i in range(1, 2**numVars):
        for j in range(numVars):
            if i&(2**j) > 0:
                self.baseIdeal[j][i-1] = 1
```
## Examples

```python
I = Ideal()
I.generateExtremal(3)
```
```python
I = Ideal(extremal = True, numVars = 3)
```