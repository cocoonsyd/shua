# Dummy Node

做需要修改链表结构的题时，为了方便处理head的情况，通常可以在链表前加一个dummy node，然后再对链表进行相应的处理。最后返回dummy node的next，即新链表的head

ListNode dummy = new ListNode\(0\);

dummy.next = head;

...

return dummy.next

