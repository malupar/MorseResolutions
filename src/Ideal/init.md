# Ideal initialization

### __Ideal__ *(base = [], variables: list[str] = [], partialExtremal = False, extremal = False, simplicialComplex = False)*

Initializes the ideal with a base of the monomials and a list of variables that represents it.

## Parameters

*__base__* : List of strings or integers, optional

 - If this is a list of integers it will contain the coordinates of the monomials that generate this ideal.
 - If this is a list of strings it will contain the monomials.

*__variables__* : List of strings, optional

<div style="margin-left: 30px;">
 Needed if the <i>base</i> is a list of strings and will express the variables in which each monomial will be expressed in.
</div>

*__partialExtremal__* : Boolean, optional

<div style="margin-left: 30px;">
A boolean that should indicate if a partial extremal should be generated. If true, then <i>numVars</i> should be provided.
</div>

*__extremal__* : Boolean, optional

<div style="margin-left: 30px;">
A boolean that should indicate if an ideal extremal should be generated. If true, then <i>numVars</i> should be provided.
</div>

*__simplicialComplex__* : Boolean, optional

<div style="margin-left: 30px;">
A boolean that should indicate if an extremal ideal should be generated. If true, then <i>r</i> and <i>q</i> should be provided.
</div>

## Implementation

```python
def __init__(self, baseIdeal = [], vars: list[str] = []):
    if len(baseIdeal) > 0 and type(baseIdeal[0][0]) == str:
    baseIdeal = parse_base(baseIdeal, vars)
    self.baseIdeal = baseIdeal
```

## Examples

```python
I = Ideal()
```
