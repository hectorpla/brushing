# String matching

### 777. Swap Adjacent in LR String

consider the following pattern:

```text
start: RRXRXX
end:   XRXXRR
```

If we zip the start and the end to get \(start\[i\], end\[i\]\) pairs. For `(R, X)`, it is like we have an open parenthesis, and we have `(X, R)` for closing it. `(R, R)` or `(X, X)`in between doesn't have impact on the validity; however, `L` - either in the start or end - in between is invalid, like following.

```text
start: RLX
end:   XXR
```

We have the symmetricity for the `(X, L)` case.  
One tricky cases is `(L, R) or (R, L)`. 

For those, we can prove that they invalid patterns. Proof:

```text
XXXX L XXRX   <- R can be somewhere on the right
XLXX R XXXX   <- L can be somewhere on the left

then L in the start can make its way to the left, but R cannot
```

Implementation:

```python
def canTransform(start, end): # (str, str) -> bool   
	current = 0
	for s, e in zip(start, end):
		if (s, e) in [('L', 'R'), ('R', 'L')]:
			return False
		if current > 0 and 'L' in (s, e):
			return False
		if current < 0 and 'R' in (s, e):
			return False
		if (s, e) == ('R', 'X'):
			current += 1
		elif (s, e) == ('X', 'R'):
			if current < 0: return False
			current -= 1
		elif (s, e) == ('X', 'L'):
			current -= 1
		elif (s, e) == ('L', 'X'):
			if current > 0: return False
			current += 1
	return current == 0
```

