# PITFALL \(dedup in children or cur\)

### 863. All Nodes Distance K in Binary Tree

Notice: annotating parents introduces **cycles**, should keep track of visited nodes when doing BFS

#### dedup in current level vs. dedup in children

If we start from node 5

| iter \ cat | dedup in current level | dedup in children |
| :--- | :--- | :--- |
| init | q: \[5\], v: {} | q: \[5\], v: {5} |
| 1 | q: \[2,3,6\], v: {5} | q: \[2,3,6\], v: {2,3,5,6} |
| 2 | q: \[7,4,5, 5, 5,1\], {2,3,5,6} | q: \[1,4,7\], v: {1,2,3,4,5,6,7} |

**duplication**!!! for the first approach



### 286. Walls and Gates

should populate distance in **children** level

pitfall if doing in cur level

```text
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
```

q = \[\(0, 2\), \(3, 0\)\]

```text
INF  -1  0   1
INF INF  1  -1
  1  -1 INF -1
  0  -1 INF INF
```

q = \[**\(1,0\)**, \(1,1\), \(2,2\)\]

```text
INF  -1  0   1
  2* 2  1  -1
  1  -1  2 -1
  0  -1 INF INF
  
*: revisit next level
```

q = \[**\(1,0\)**, **\(0,0\)**, \(3,2\)\]

```text
  3* -1  0   1
  2   2  1  -1
  1  -1  2 -1
  0  -1  3 INF
  
*: revisit next level
```

q = \[**\(0,0\)**, \(3,3\)\]

