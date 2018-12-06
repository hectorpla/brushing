# monotonic queue

## Recipe

actually an advanced version of monotonic **stack**, except that it allows the **popleft** operation, should be called a **deque**

* eliminate history **irrelevant** data using the stack property, data in the deque is **time sorted**
* **expire** data at the tail, ensure that every datum is inserted into the deque once and popped from the deque once \(either via pop\(\) or popleft\(\)\)

## Problems

### 239. Sliding Window Maximum

non-increasing queue \(maintain the necessary info inside the window\)

1. add - multiple popleft and one push
2. remove - 0 or 2 popleft

### 683. K Empty Slots

increasing queue, k-size window, min Queue to maintain the earliest blooming day in the window

