# Introduction

This code consists on the implementation of splitting and pruning techniques applied to free resolutions of monomial ideals.

We will have an object representing an ideal \\(I\\). Let this ideal be generated by a series of monomials \\(I =x_1^{\alpha_1} ... x_n^{\alpha_n}\\)

# General description of the pruning algorithm

Let \\(n\\) be the number of monomials that generate our ideal and let us consider the hypercube of a dimension equal to \\(n\\). Each vertex of the hypercube will be correpondent with a set of coordinates \\( c \in [0, 1]^n\\) and will have a value associates with it. The value of each vertex will be calculated as \\(lcm(x_{i_1}^{\alpha_{i_1}},x_{i_2}^{\alpha_{i_2}}, ..., x_{i_k}^{\alpha_{i_k}})\\) where \\(c_{i_j} = 1 \ \forall j \in [1, k]\\). 

We will prune two vertices if and only if their values are the same.

To implement this algorithm efficiently we will start with a set of empty monomials and only one vertex with \\(n\\) coordinates equal to \\(0\\). In the \\(i\\)-th step we will create a copy of all existing vertex and setting the \\(i\\)-th coordinate to \\(1\\). Then we will iterate through all pairs that include one of the new vertex and differ only in one coordinate and prune the ones that have the same value.

> __Observation__: We do not need to create every single vertex from the start and will save up many operations by observing that if \\(a = b\\) then \\(lcm(a, c) = lcm(b, c)\\) and we will end up pruning the new vertices anyway.

