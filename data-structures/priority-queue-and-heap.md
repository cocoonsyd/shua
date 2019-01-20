# Priority queue & Heap

## Top k Largest Numbers II

https://www.lintcode.com/problem/top-k-largest-numbers-ii

注意有两种思路。

一种是把所有的数都放到priority queue里，最后从中取出top k。这个方法需要使用可以取最大值的priority queue。缺点是需要更多空间存储所有的数，并且最后需要klogn时间把top k个数取出来。

更好的一种方法是注意到这道题只需要return top k，k是提前知道的，并且不需要有remove\(\)这个功能。这就意味着我们可以只保存top k个数，剩下的数可以直接丢掉。注意这个方法需要使用可以取最小值的priority queue，因为每拿到一个数需要跟目前为止top k中最小的比较，如果比它小可以直接丢掉，比它大才需要放到top k queue里面。

## Merge K Sorted Lists

https://www.lintcode.com/problem/merge-k-sorted-lists/

也有多种思路。

第一种最容易想到，把每个linked list的head放到priority queue里面，每次取最小的。

第二种，divide & conquer。把合并k个链表层层分解，每次合并两个链表，最终合并成一个链表。（类似mergesort）


