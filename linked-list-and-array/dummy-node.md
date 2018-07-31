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

## Partition List

https://www.lintcode.com/problem/partition-list/

不要忘记把结果中最后一个node的next设为null！

```java
public class Solution {
    /**
     * @param head: The first node of linked list
     * @param x: An integer
     * @return: A ListNode
     */
    public ListNode partition(ListNode head, int x) {
        // write your code here
        ListNode dummyhead1 = new ListNode(0);
        ListNode dummyhead2 = new ListNode(x);
        ListNode cur1 = dummyhead1;
        ListNode cur2 = dummyhead2;
        ListNode cur = head;
        while(cur!=null){
            if(cur.val<x){
                cur1.next=cur;
                cur1=cur;
            }
            else{
                cur2.next=cur;
                cur2=cur;
            }
            cur=cur.next;
        }
        
        // Don't forget this!!!!!!!!!!!!!!
        cur2.next=null;
        
        cur1.next=dummyhead2.next;
        return dummyhead1.next;
    }
}
```