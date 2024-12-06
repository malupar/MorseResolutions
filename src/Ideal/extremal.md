# Generate an extremal ideal

### Ideal.__generateExtremal__ *(numVars : int)*

## Parameters

*__numVars__* : Integer

## Implementation
```python
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
```
## Examples

```python
I = Ideal()
I.generateExtremal(3)
```
```python
I = Ideal(extremal = True, numVars = 3)
```