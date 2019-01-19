# Hash table

## LRU Cache

[https://www.lintcode.com/problem/lru-cache](https://www.lintcode.com/problem/lru-cache/description)

Doubly linked list + hash table

```java
public class LRUCache {
    /*
    * @param capacity: An integer
    */
    private class Node {
        Node prev;
        Node next;
        int key;
        int val;
        public Node(int key, int val){
            this.key=key;
            this.val=val;
        }
    }
    
    private int capacity;
    private Node head;
    private Node tail;
    private HashMap<Integer, Node> hash;
    
    public LRUCache(int capacity) {
        // do intialization if necessary
        this.capacity = capacity;
        hash = new HashMap<Integer, Node>();
        head = new Node(0,0);
        head.next = tail;
        tail = new Node(0,0);
        tail.prev = head;
    }

    /*
     * @param key: An integer
     * @return: An integer
     */
    public int get(int key) {
        // write your code here
        if(!hash.containsKey(key)) return -1;
        
        Node node = hash.get(key);
        node.prev.next = node.next;
        node.next.prev = node.prev;
        node.next = tail;
        node.prev = tail.prev;
        node.prev.next = node;
        tail.prev = node;
        return node.val;
    }

    /*
     * @param key: An integer
     * @param value: An integer
     * @return: nothing
     */
    public void set(int key, int value) {
        // write your code here
        if(hash.containsKey(key)){
            hash.get(key).val=value;
            //use get() to move key to tail(update most recent used)
            get(key);
            return;
        }
        if(hash.size() == capacity){
            //cache full, remove lease recently used node
            hash.remove(head.next.key);
            head.next = head.next.next;
            head.next.prev=head;
        }
        
        //insert new node before tail
        Node new_node = new Node(key, value);
        hash.put(key, new_node);
        new_node.next = tail;
        new_node.prev = tail.prev;
        new_node.prev.next = new_node;
        tail.prev = new_node;

    }
}
```

