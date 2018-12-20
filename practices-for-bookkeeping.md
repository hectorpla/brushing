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

