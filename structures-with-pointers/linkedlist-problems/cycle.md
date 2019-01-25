# Cycle

### 142.cycle II

image: the length of the non-cyclic path is N, and the cyclic path is P + Q

the distances walked until slow and fast pointers meet

{% hint style="info" %}
slow: N + P

fast: N + P + k \* \(P + Q\)

fast is twice as slow, so:

`2*(N + P) = N + P + k * (P+Q)`

=&gt; N = k \* \(P + Q\) - P = \(k-1\) \* \(P+Q\) + P, where P+Q is the cycle length
{% endhint %}

