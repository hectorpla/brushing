# Tree DP

### 968. Binary Tree Cameras

at the contest: 1. figured out somehow DP 2. let a node's information include parent info, very messy 3. 

lesson learned: 1. don't mix data flow 2. analyze states \(..\) 3. make sure the idea before coding



impl:

1. base case: null -&gt; \(0, 0, inf\)



| cur states \  children states  \|   \[0: uncovered, 1: covered, 2: installed\] | \(left, right\) |
| :--- | :--- |
| 0. uncovered: need to satisfied both the children | \(1, 1\) |
| covered: both children satisfied and at least one of them installed | \(2,1\), \(1,2\), \(2,2\) |
| installed: either child not satisfied | \(0, x\), \(x, 0\) |

```python
def minCameraCover(self, root):    
    # 1. the current node is not covered by its children
    # 2. the current node is covered but the camera is not installed
    # 3. a camera is installed in the current node
    def computeTriplet(root):
        if root is None:
            # bug: installed shuold be inifinite <- null cannot cover its parent
            return 0, 0, float('inf')  
        lUncovered, lCovered, lInstalled = left = computeTriplet(root.left)
        rUncovered, rCovered, rInstalled = right = computeTriplet(root.right)
        
        uncovered = lCovered + rCovered
        covered = min(
            lInstalled + rCovered,
            lInstalled + rInstalled,
            lCovered + rInstalled
        )
        # install camera
        installed = 1 + min(
            lUncovered + min(right),
            min(left) + rUncovered
        )
        # print(root.val, (uncovered, covered, installed))
        return uncovered, covered, installed
    
    # top-level
    uncovered, covered, installed = computeTriplet(root)
    return min(covered, installed)  # cannot leave the root uncovered
    
# tests
# [0,1,null,2,3]
# [0,1,null,2,null,3,null,null,4]
# [0,1,null,2,null,3,null,null,4,5]
# [0,1,2]
# [0,1,2,3,4,5]
```



