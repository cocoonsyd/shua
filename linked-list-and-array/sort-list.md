# Sort List

Merge Sort: 时间复杂度O\(nlogn\)，空间复杂度O\(n\)

Quick Sort: 时间复杂度O\(nlogn\)，空间复杂度O\(1\)

https://www.lintcode.com/problem/sort-list/

Merge Sort:

```java

public class Solution {

    public ListNode sortList(ListNode head) {
        if(head==null || head.next==null) return head;
        
        ListNode mid = findMiddle(head);
        ListNode right = sortList(mid.next);
        // don't forget, very important!
        mid.next = null;
        ListNode left = sortList(head);
        
        return merge(left, right);
    }
    
    private ListNode findMiddle(ListNode head){
        ListNode slow = head;
        ListNode fast = head.next;
        while(fast!=null && fast.next!=null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
    
    private ListNode merge(ListNode head1, ListNode head2){
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        while(head1!=null && head2!=null){
            if(head1.val<head2.val){
                cur.next = head1;
                head1 = head1.next;
            }
            else{
                cur.next = head2;
                head2 = head2.next;
            }
            cur = cur.next;
        }
        if(head1!=null) cur.next = head1;
        else cur.next = head2;
        return dummy.next;
    }
}
```
