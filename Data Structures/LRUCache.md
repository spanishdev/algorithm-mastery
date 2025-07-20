# LRU Cache

## Description
* Uses 2 data structures: `HashMap` and `Doubly Linked List`
* Has a maximum capacity
* 2 operations, get and put
* When get, put the item to the last recently used (head of linked list)
* When put, put the item in the last recently used (head of linked list)
* If capacity is full, then remove the oldest item used (tail of linked list)

### Example

```kotlin

Input:
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]

Output:
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation:
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {3=3, 4=4}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4

```
## Approach

### Data Structure Design:
- HashMap<K, Node> for O(1) key lookup
- Doubly Linked List for O(1) insertion/deletion with order tracking
- Custom Node class to connect both structures

### Operations:

- get(key): HashMap lookup + move to head (mark as recently used)
- put(key, value): Add/update + move to head + evict tail if needed

### Complexity Analysis
- Time: O(1) for both get() and put() operations
- Space: O(capacity) for storing key-value pairs

## Implementation

```kotlin
class LRUCache<K, V>(private val capacity: Int) {
    
    // Node for doubly linked list
    internal data class Node<K, V>(
        val key: K,
        var value: V,
        var next: Node<K, V>? = null,
        var prev: Node<K, V>? = null
    )
    
    private val map = HashMap<K, Node<K, V>>()
    private var head: Node<K, V>? = null
    private var tail: Node<K, V>? = null
    
    fun get(key: K): V? {
        return map[key]?.let { node ->
            disconnectNode(node)
            addToHead(node)
            node.value
        }
    }
    
    fun put(key: K, value: V) {
        val node = map[key]
        
        if (node != null) {
            // Update existing node
            node.value = value
            if (head != node) {
                disconnectNode(node)
                addToHead(node)
            }
        } else {
            // Add new node
            val newNode = Node(key, value)
            
            if (map.size >= capacity) {
                removeTail()
            }
            
            map[key] = newNode
            addToHead(newNode)
        }
    }
    
    private fun disconnectNode(node: Node<K, V>) {
        node.prev?.next = node.next
        node.next?.prev = node.prev
        
        if (node == head) head = node.next
        if (node == tail) tail = node.prev
    }
    
    private fun addToHead(node: Node<K, V>) {
        node.next = head
        node.prev = null
        head?.prev = node
        head = node
        
        if (tail == null) tail = node
    }
    
    private fun removeTail() {
        tail?.let { oldTail ->
            map.remove(oldTail.key)
            disconnectNode(oldTail)
        }
    }
}
```
