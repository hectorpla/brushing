# Union Find

### 565. Array Nesting

In this problem, all edges are **directed** \(for which we cannot simply apply UF\), but the connected components always forms a **cycle** \(didn't prove it though\)

{% hint style="info" %}
Premises:

1. src, dest: \[0, n-1\]， \# edge = n
2. each node has exactly **one** out-degree

Observation:

start from a node, traverse alone the way, **always** terminate with meeting some node **visited** on the **linked list**

Why UF is applicable to this directed graph?
{% endhint %}

### 947. Most Stones Removed with Same Row or Column \(implicit graph\)

{% hint style="info" %}
Reasoning:

1. stones are connected by shared row or col
2. disjoint components can be taken care of independently
3. what is the max number for each component \(the move strategy?\)
{% endhint %}

### 721. Accounts Merge

good practice to implement long logic, **take-away**: planing before writing code  
be sure to get the types correct

```python
def accountsMerge(self, accounts):
    email2accounts = {} # {str: int[]}        
    
    # 1. group accounts by email address, O(n * v), v is the average # email per account
    for account, (name, *emails) in enumerate(accounts):
        for email in emails:
            email2accounts.setdefault(email, []).append(account)
    # print(email2accounts)
    
    # 2. union-find to connect accounts for email addresses, O(n^2 * logn), merge every pair of account, union takes O(logn)
    n = len(accounts)
    ## bad: didn't modulize
    id_ = list(range(n))
    sz = [1] * n
    
    def find(p):
        while id_[p] != p:
            id_[p] = id_[id_[p]]
            p = id_[p]
        return p
    
    def union(p, q):
        proot, qroot = find(p), find(q)
        if proot == qroot:
            return
        if sz[proot] >= sz[qroot]:
            id_[qroot] = proot
            sz[proot] += sz[qroot]
        else:
            id_[proot] = qroot
            sz[qroot] += sz[proot]
        
    for connectedList in email2accounts.values():
        for i in range(len(connectedList)-1):
            acnt1, acnt2 = connectedList[i], connectedList[i+1]
            union(acnt1, acnt2)
    # print(id_)
    
    # 3. output only unique ids, assume find() takes O(1), O(n*v*log(v)) ~ O(n*v*log(n*v))
    ## unplanned issue here, how to find all nodes in a components from a root
    result = []
    root2nodes = {} # {int: int[]}
    for acnt in range(n):
        root = find(acnt) # bug: cannot directly use id_, which only points to the direct parent
        root2nodes.setdefault(root, []).append(acnt)
        if len(root2nodes[root]) != sz[root]:
            continue
        # now all components are connected
        name, emails = accounts[acnt][0], { email for account in root2nodes[root] for email in accounts[account][1:] }
        result.append([name, *list(sorted(emails))])
        
    return result
```

