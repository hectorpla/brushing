# Java pitfalls

### 89. Gray Code

buggy: modifying the list when iterating through the list with descendingIterator

```java
class Solution {
    // idea: add and reverse, [0, 1] -> add one digit (+2) and reverse -> [3, 2]
    // iterative method very straightforward
    public List<Integer> grayCode(int n) {
        LinkedList<Integer> result = new LinkedList<>();
        result.add(0);
        
        // tests
        // 0 -> [0]
        // 1 -> [0, 1]
        // 2 -> [0, 1, 3, 2]
        for (int i = 0; i < n; i++) { // reverse&add n times
            Iterator<Integer> reversed = result.descendingIterator();
            int add = 1 << i;
            LinkedList<Integer> temp = new LinkedList<>();
            
            while (reversed.hasNext()) { // without temp, ConcurrentModificationException: add when traversing
                temp.add(add + reversed.next());
            }
            result.addAll(temp);
        }
        return result;
    }
}
```

