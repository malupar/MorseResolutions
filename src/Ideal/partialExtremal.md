# Generate a partial extremal ideal

### Ideal.__generatePartialExtremal__ *(numVars : int)*

## Parameters

*__numVars__* : Integer

## Implementation
```python
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
```
## Examples

```python
I = Ideal()
I.generatePartialExtremal(4)
```
```python
I = Ideal(partialExtremal = True, numVars = 4)
```