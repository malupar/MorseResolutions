# Different solvers for different examples

We have a general solver which will execute the following algorithm:

> __General pruning algorithm__
>
> __*Input*__ --- Set of monomials of an ideal, let number of monomials be \\(n\\)
>
> __*Output*__ --- Edges that have been removed
>
> __*Description*__ </br> Loop for each monomial, \\(i = 1, 2, ..., n\\) : </br>\\(\quad\\) Copy all existing vertex and set the new ones with the \\(i\\)-th coordinate to 1.<br/> \\(\quad\\) Loop for each remaining vertex: </br> \\(\quad \quad\\) Check for equality for all remaining adjacent vertex </br> \\(\quad \quad\\) Remove both vertex if equality holds