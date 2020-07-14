# Thoughts on consistency mask for temporal and spatial warpings

## Temporal consistency

$p_i \rightarrow p_j$ 

$p_j = \Pi(K T_{ij} D_i K^{-1} p_i)$

$p_j \rightarrow p_i$

$p_i = \Pi(K T_{ji} D_j K^{-1} p_j)$


If the scene is static and $T_{ij}$, $T_{ji}$, $D_i$, and $D_j$ are exact, then following cyclic consistency should be established.

$p_i \rightarrow p_j \rightarrow p^\prime_i$

We can make a consistency map by computing $| p_i - p^\prime_i |$

Note that all the computation is done on coordinates not on intensity or color

