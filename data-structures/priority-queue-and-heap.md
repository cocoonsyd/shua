# Priority queue & Heap

## Top k Largest Numbers II

[https://www.lintcode.com/problem/top-k-largest-numbers-ii](https://www.lintcode.com/problem/top-k-largest-numbers-ii)

注意有两种思路。

一种是把所有的数都放到priority queue里，最后从中取出top k。这个方法需要使用可以取最大值的priority queue。缺点是需要更多空间存储所有的数，并且最后需要klogn时间把top k个数取出来。

更好的一种方法是注意到这道题只需要return top k，k是提前知道的，并且不需要有remove\(\)这个功能。这就意味着我们可以只保存top k个数，剩下的数可以直接丢掉。注意这个方法需要使用可以取最小值的priority queue，因为每拿到一个数需要跟目前为止top k中最小的比较，如果比它小可以直接丢掉，比它大才需要放到top k queue里面。下面程序用的就是这种方法。

```java
public class Solution {
    /*
    * @param k: An integer
    */
    private int k;
    private PriorityQueue<Integer> pq;
    public Solution(int k) {
        //by default head of PriorityQueue is the minimum
        //if want max, need to provide a comparator, like Collections.reverseOrder()
        pq = new PriorityQueue<Integer>();
        this.k = k;
    }

    /*
     * @param num: Number to be added
     * @return: nothing
     */
    public void add(int num) {
        if(pq.size()<k){
            pq.offer(num);
            return;
        }
        if(num>pq.peek()){
            pq.poll();
            pq.offer(num);
        }
    }

    /*
     * @return: Top k element
     */
    public List<Integer> topk() {
        ArrayList<Integer> result = new ArrayList<Integer>(pq);
        Collections.sort(result, Collections.reverseOrder());
        return result;
    }
}
```

## Merge K Sorted Lists

[https://www.lintcode.com/problem/merge-k-sorted-lists/](https://www.lintcode.com/problem/merge-k-sorted-lists/)

也有多种思路。

第一种最容易想到，把每个linked list的head放到priority queue里面，每次取最小的。

```java
public class Solution {
    /**
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    public ListNode mergeKLists(List<ListNode> lists) {  
        // write your code here
        PriorityQueue<ListNode> pq = new PriorityQueue<>(new Comparator<ListNode>(){
                public int compare(ListNode left, ListNode right) {
                return left.val - right.val;
        }
            });
        for(ListNode head : lists){
            if(head!=null){
                pq.offer(head);
            }
        }
        ListNode head = new ListNode(0);
        ListNode cur = head;
        while(!pq.isEmpty()){
            cur.next = pq.poll();
            if(cur.next.next!=null){
                pq.offer(cur.next.next);
            }
            cur = cur.next;
        }
        return head.next;
    }
}
```

第二种，divide & conquer。把合并k个链表层层分解，每次合并两个链表，最终合并成一个链表（非常类似mergesort过程）。这种思路又有两种实现方式（类似mergesort也有两种实现），一种是用recursion不断分解直到base情况即合并两个链表，另一种是用循环两两合并，一轮结束后得到k/2个链表再两两合并（可能用到queue？）。

Recursion:

```java
public class Solution {

    public ListNode mergeKLists(List<ListNode> lists) {  
        if(lists==null || lists.size()==0){
            return null;
        }
        return mergeIndex(lists, 0, lists.size()-1);
    }

    private ListNode mergeIndex(List<ListNode> lists, int start, int end){
        if(start==end){
            return lists.get(start);
        }

        int mid = (start+end)/2;
        ListNode left = mergeIndex(lists, start, mid);
        ListNode right = mergeIndex(lists, mid+1, end);
        return mergeTwoLists(left, right);
    }

    private ListNode mergeTwoLists(ListNode list1, ListNode list2){
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        while(list1!=null && list2!=null){
            if(list1.val<list2.val){
                cur.next = list1;
                list1=list1.next;
                cur=cur.next;
            }
            else{
                cur.next = list2;
                list2=list2.next;
                cur=cur.next;
            }
        }
        if(list1!=null){
            cur.next=list1;
        }
        if(list2!=null){
            cur.next=list2;
        }
        return dummy.next;
    }
}
```

