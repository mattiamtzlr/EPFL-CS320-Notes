#math 
# Definition
Similarly to how we defined [[02_lexical_analyzers#Language derivatives|derivatives w.r.t. a letter]], we can also define **derivatives w.r.t. a word**. Let $L \subseteq A^*$ be a language and let $v \in A$ be a *word*. Then
$$\partial_vL = \{w \in A^* \ | \ vw \in L\}$$

Observe by definition:
- $w \in \partial_vL \quad \Leftrightarrow \quad vw \in L$
- $w \in \partial_{\varepsilon}L \quad \Rightarrow \quad \varepsilon w \in L \quad \Rightarrow \quad \partial_{\varepsilon}L = L$

Thus, we get
$$w \in \partial_{av}L \quad \Leftrightarrow \quad avw \in L \quad \Leftrightarrow \quad vw \in \partial_{a}L \quad \Leftrightarrow \quad w \in \partial_v(\partial_aL)$$

giving us the *recursive definition* $\partial_{av}L = \partial_v(\partial_aL)$.

<br>

# Rules for Derivatives of Reg-Exs
In the following $+$ denotes union. We have the following derivative rules for regular expressions:
- $\partial_a\varepsilon = \emptyset$
- $\partial_a\emptyset = \emptyset$
- $\partial_ab = \{$if $b = a$ then $\varepsilon$ else $\emptyset\}$
- $\partial_a(r_1 + r_2) = (\partial_ar_1) + (\partial_ar_1)$
- $\partial_a(r_1 r_2) = \{$if $\text{nullable}(r_1)$ then $((\partial_ar_1)r_2) + \partial_ar_2$ else $((\partial_ar_1)r_2\}$
- $\partial_a(r^*) = (\partial_ar)r^*$

The derivative of a reg-ex is a reg-ex.

## Checking if a Word Belongs to a Reg-Ex
Given the previous rules and observations, we get the following equivalences:
$$w \in L \quad \Leftrightarrow \quad w\varepsilon \in L \quad \Leftrightarrow \quad \varepsilon \in \partial_wL \quad \Leftrightarrow \quad \text{nullable}(\partial_wL)$$
$$w_1 ... w_n \in L \quad \Leftrightarrow \quad  \varepsilon \in \partial_{w_1 ... w_n}L \quad \Leftrightarrow \quad \text{nullable}(\partial_{w_n} \  ...\ \partial_{w_1} \ L)$$

These give us the following **algorithm** to check whether a word belongs to $L$:
1. compute the derivative of $L$ with respect to the all letters of the word in order,
2. check whether the final reg-ex is nullable, for which we have seen an inductive algorithm [[02_regular_expressions#Nullable|here]].