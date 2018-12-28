# Practices for bookkeeping

### Paging Airbnb listings

page listings on basis of 12 and no listings with same hostID should be inside a single page

```python
class Listing(object):
    def __init__(self, hostId, listingId, score, city):
        self.hostId = hostId
        self.listingId = listingId
        self.score = score
        self.city = city

    def __str__(self):
        return "<host: {}, id: {}, score: {}, city: {}>".format(self.hostId, self.listingId, self.score, self.city)


def pageListing(listings):  # Listing[] -> Listing[][]
    pages = []  # Listing[][]
    id2nextpage = {}  # {int: int}
    currentAvailPage = 0

    for listing in listings:
        key = listing.hostId
        page = max(currentAvailPage, id2nextpage.get(key, 0))
        if len(pages) <= page:
            assert len(pages) == page
            pages.append([])
        # invariant: no page contains two same key
        assert key not in [l.hostId for l in pages[page]]
        pages[page].append(listing)
        id2nextpage[key] = page + 1

        # invariant: the size of the current page is < 12
        while currentAvailPage < len(pages) and len(pages[currentAvailPage]) == 12:
            currentAvailPage += 1

    return pages
```





### 498. Diagonal Traverse

!!!! corner cases, the top-right and the bottom-left

```python
def findDiagonalOrder(self, matrix):
    m, n = len(matrix), len(matrix[0]) if matrix else 0
    
    # two types of turns: 
    # 1. up -> down:  
    #   a. i < 0
    #   b. j = n
    # 2. down -> up
    #   a. j < 0
    #   b. i = m
    dirs = [[-1,1], [1,-1]]
    
    result = []
    i, j = 0, 0
    d = 0
    while len(result) < m * n:
        # turnings
        # bugs: top-right corner!!! order matters!!!
        if j == n:  # check j first because the top-right corner is included into this case
            assert d == 0
            i, j = i + 2, j - 1
            d = 1
            continue
        if i < 0:
            assert d == 0
            i += 1
            d = 1
            continue
        if i == m:
            assert d == 1
            i, j = i - 1, j + 2
            d = 0
            continue
        if j < 0:
            assert d == 1
            j += 1
            d = 0
            continue
        
        # invariant: i in [0, m), j in [0, n)
        assert 0 <= i < m and 0 <= j < n
        result.append(matrix[i][j])
        di, dj = dirs[d]
        i, j = i + di, j + dj
        
    return result

# tests
# []
# [[]]
# [[1]]
# [[1,2,3]] # single row
# [[1],[2],[3]] -- single col, test top-right corner
# [[1,2,3],[4,5,6]] -- bottom-left corner
# [[1,2,3],[4,5,6],[7,8,9]]
```

