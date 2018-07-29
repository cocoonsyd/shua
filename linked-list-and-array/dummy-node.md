# Dummy Node

做需要修改链表结构的题时，为了方便处理head的情况，通常可以在链表前加一个dummy node，然后再对链表进行相应的处理。最后返回dummy node的next，即新链表的head

ListNode dummy = new ListNode\(0\);

dummy.next = head;

...

return dummy.next

## Reverse Nodes in k-Groups

https://www.lintcode.com/problem/reverse-nodes-in-k-group/

```java

public class Solution {
    /**
     * @param head: a ListNode
     * @param k: An integer
     * @return: a ListNode
     */
    public ListNode reverseKGroup(ListNode head, int k) {
        // write your code here
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;
        boolean end = false;
        while(prev.next!=null){
            ListNode first = prev.next;
            ListNode curr = first;
            for(int i=1;i<k;i++){
                curr=curr.next;
                if(curr==null){
                    end=true;
                    break;
                }
            }
            if(end) break;
            ListNode last = curr;
            System.out.println(prev.val+" "+ first.val+" "+last.val);
            prev.next = reverse(prev,first,last);
            prev=first;
        }
        return dummy.next;
    }
    
    private ListNode reverse(ListNode prev, ListNode first, ListNode last){
        ListNode next=last.next;
        ListNode curr=first;
        while(curr!=last){
            ListNode tmp = curr.next;
            curr.next=next;
            next=curr;
            curr=tmp;
        }
        curr.next=next;
        return curr;
    }
}
```

