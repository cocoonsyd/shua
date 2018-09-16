# Sort List

Merge Sort: 时间复杂度O\(nlogn\)，空间复杂度O\(n\) （指在Array上，这里在链表上不需要额外空间）

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

# Convert Sorted List to Binary Search Tree

https://www.lintcode.com/problem/convert-sorted-list-to-binary-search-tree/

```java
public class Solution {
    /*
     * @param head: The first node of linked list.
     * @return: a tree node
     */
    public TreeNode sortedListToBST(ListNode head) {
        // write your code here
        if(head==null) return null;
        if(head.next==null) return new TreeNode(head.val);
        ListNode dummy = new ListNode(0);
        dummy.next=head;
        ListNode beforeMid = findBeforeMid(dummy);
        TreeNode root = new TreeNode(beforeMid.next.val);
        root.right = sortedListToBST(beforeMid.next.next);
        
        if(head!=beforeMid.next){
            beforeMid.next=null;
            root.left = sortedListToBST(head);
        }
        
        return root;
    }
    
    private ListNode findBeforeMid(ListNode head){
        ListNode slow = head;
        ListNode fast = head.next;
        ListNode beforemid = head;
        while(fast!=null && fast.next!=null){
            slow = slow.next;
            fast = fast.next.next;
            beforemid=slow;
        }
        return beforemid;
    }
}
```

# Convert Binary Search Tree to Doubly Linked List

https://www.lintcode.com/problem/convert-binary-search-tree-to-doubly-linked-list/

```java
class ResultType{
    DoublyListNode first;
    DoublyListNode last;
    public ResultType(DoublyListNode first, DoublyListNode last){
        this.first=first;
        this.last=last;
    }
}

public class Solution {
    /*
     * @param root: The root of tree
     * @return: the head of doubly list node
     */
    public DoublyListNode bstToDoublyList(TreeNode root) {
        // write your code here
        if(root==null) return null;
        ResultType result = helper(root);
        return result.first;
    }
    
    public ResultType helper(TreeNode root){
        if(root==null) return null;
        
        ResultType left = helper(root.left);
        ResultType right = helper(root.right);
        DoublyListNode node = new DoublyListNode(root.val);
        ResultType result = new ResultType(null,null);
        
        if(left==null) result.first = node;
        else {
            result.first = left.first;
            left.last.next = node;
            node.prev = left.last;
        }
        
        if(right==null) result.last = node;
        else {
            result.last = right.last;
            right.first.prev = node;
            node.next = right.first;
        }
        
        return result;
    }
}
```
