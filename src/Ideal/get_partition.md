# Get an ideal partition

### Ideal.__getPartition__ *(variable: int)*

## Parameters

*__variable__* : Integer

<div style="margin-left: 30px;">
 The variable that all monomials of the partition should have in common.
</div>

## Implementation

```python
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
```

## Examples
```python
Ideal = I(["xy", "xz", "yz"], ["x", "y", "z"])
index = I.getPartition("x")
```