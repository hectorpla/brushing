# Trie

Three ways to store info in TrieNode:

1. isWord: boolean \(traverse when query, save space\)
2. reference to the word: int \(easy to implement then one\)
3. descendent references: \*Word\[\] \(more memory, fast retrieval when query, high cost to update\)



### 208

specs, both search and startwiths return boolean.

