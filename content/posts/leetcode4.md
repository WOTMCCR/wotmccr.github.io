---
tags:
  - 链表
  - leetcode
date: 2024-05-14
title: 2024-05-14刷题1
description: " 24. 两两交换链表中的节点 ，19.删除链表的倒数第N个节点 ， 面试题 02.07. 链表相交 ，142.环形链表II"
---
## 2024-05-14

### 24.两两交换链表中的节点
17:10 17:32 18:08
#### 思路
因为交换两个链表节点需要已知需交换两节点前面的节点，所以，为了保证运算的统一性，循环更新，前面的头节点，循环条件为，头节点之后两个需交换的节点都不为空。
之后，将更新节点指针指向头节点的下一个，进行更新操作。
```C++
        while(left->next!=nullptr && left->next->next!=nullptr)
        {
            right = left->next;
            left->next = right->next;
            right->next = right->next->next;
            left->next->next = right;
            left = left->next->next;
        }
```
#### 复杂度
- 时间复杂度：O(n)，其中 n 是链表的节点数量。需要对每个节点进行更新指针的操作。
- 空间复杂度：O(1)。
#### 注意⚠️
如果想要交换两个节点需要知道这两个节点的前一个节点
#### 题解
##### C++
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode *dummyhead = new ListNode(0,head);
        ListNode *left = dummyhead;
        ListNode *right = dummyhead;
        while(left->next!=nullptr && left->next->next!=nullptr){
            right = left->next;
            left->next = right->next;
            right->next = right->next->next;
            left->next->next = right;
            left = left->next->next;
        }
        return dummyhead->next;
    }
};
```
##### java
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummyhead = new ListNode(0,head);
        ListNode left = dummyhead;
        ListNode right = left;
        while(left.next!=null && left.next.next!=null){
            right = left.next;

            left.next = right.next;
            right.next =right.next.next;
            left.next.next = right;

            left = left.next.next;
        }
        return dummyhead.next;
    }
}
```
##### go
```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func swapPairs(head *ListNode) *ListNode {
    dummyHead := &ListNode{0,head}
    temp := dummyHead
    for temp.Next != nil && temp.Next.Next != nil {
        node1 := temp.Next
        temp.Next = node1.Next
        node1.Next = node1.Next.Next
        temp.Next.Next = node1

        temp = temp.Next.Next
    }
    return dummyHead.Next
}
```
##### swift
```swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init() { self.val = 0; self.next = nil; }
 *     public init(_ val: Int) { self.val = val; self.next = nil; }
 *     public init(_ val: Int, _ next: ListNode?) { self.val = val; self.next = next; }
 * }
 */
class Solution {
    func swapPairs(_ head: ListNode?) -> ListNode? {
        let dummyHead = ListNode(0, head)
        var current: ListNode? = dummyHead        
        while let first = current?.next, let second = first.next {
            first.next = second.next
            second.next = first
            current?.next = second
            
            current = first
        }
        
        return dummyHead.next
    }
}
```

### 19.删除链表的倒数第N个节点
#### 思路
我觉得这个题的思路可以用到很多题中，如果想要在倒数第n个元素的位置处添加指针，就可以让两个相差n个节点的指针一起往后移动，当后一个指针到达结尾的时，前一个指针正好到倒数第n个的位置。
#### 复杂度
时间复杂度为O(n)
空间复杂度为O(1)

#### 注意⚠️
java里 `&&` 不能用于将 `int` 与 `boolean` 进行比较或运算。
不能这么写
```java
        while(n-- && second->next != nullptr){
            second = second -> next;
        }
```
需要
```java
// Move second n steps ahead
        for (int i = 0; i < n; i++) {
            if (second.next != null) {
                second = second.next;
            } else {
                // If n is larger than the length of the list, return head
                return head;
            }
        }
```
#### 题解
##### C++
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummyhead = new ListNode(0,head);
        ListNode *first = dummyhead;
        ListNode *second = first;
        while(n-- && second->next != nullptr){
            second = second -> next;
        }
        while(second->next != nullptr){
            first = first->next;
            second = second->next;
        }
        first->next = first->next->next;
        return dummyhead->next;
    }
};
```
##### java
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummyHead = new ListNode(0,head);
        ListNode first = dummyHead;
        ListNode second = first;
        
        // Move second n steps ahead
        for (int i = 0; i < n; i++) {
            if (second.next != null) {
                second = second.next;
            } else {
                // If n is larger than the length of the list, return head
                return head;
            }
        }

        while(second.next!=null){
            first = first.next;
            second = second.next;
        }

        first.next = first.next.next;
        return dummyHead.next;
    }
}
```
### 链表相交
19:03 19:15
#### 思路
当一条链表遍历到结尾的时候，就从另一条链表的开始处重新遍历，可以看作是，将两段不等长的部分补到一起，然后进行遍历，如果，相交则会相同。
如果不想交，就会在最后都同时等于null
#### 复杂度
时间复杂度：O(m+n)
#### 题解
##### C++
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if(headA == nullptr || headB == nullptr){
            return nullptr;
        }
        ListNode *pa = headA,*pb = headB;
        while(pa!=pb){
            pa = pa == nullptr ? headB : pa->next;
            pb = pb == nullptr ? headA : pb->next;
        }
        return pa;
    }
};
```
##### java
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null){
            return null;
        }
        ListNode pa = headA,pb = headB;
        while(pa!=pb){
            pa = pa == null ? headB : pa.next;
            pb = pb == null ? headA : pb.next;
        }
        return pa;
    }
}
```
##### go
```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    if headA == nil || headB == nil {
        return nil
    }
    pa, pb := headA, headB
    for pa != pb {
        if pa == nil {
            pa = headB
        } else {
            pa = pa.Next
        }
        if pb == nil {
            pb = headA
        } else {
            pb = pb.Next
        }
    }
    return pa
}
```
##### swift
```swift
class Solution {
    func getIntersectionNode(_ headA: ListNode?, _ headB: ListNode?) -> ListNode? {    
      var currentA = headA
      var currentB = headB
      while currentA !== currentB {
        currentA = (currentA == nil) ? headB : currentA?.next
        currentB = (currentB == nil) ? headA : currentB?.next 
      }
      return currentA
    }
}
```

### 142.环形链表
19:44
#### 思路
快慢指针的方法，如果有环的话，快指针一定会遇到慢指针。
有a=c+(n−1)(b+c) 的等量关系，即
相遇之后，从相遇点到入环点的距离加上 n−1 圈的环长，恰好等于从链表头部到入环点的距离。
因此当发现 slow 与 fast相遇时，我们再额外使用一个指针 ptr。起始，它指向链表头部；随后，它和 slow每次向后移动一个位置。最终，它们会在入环点相遇
#### 复杂度
时间复杂度：O(N)，其中 N 为链表中节点的数目。在最初判断快慢指针是否相遇时，指针走过的距离不会超过链表的总长度；随后寻找入环点时，走过的距离也不会超过链表的总长度。因此，总的执行时间为 O(N)+O(N)=O(N)
空间复杂度：O(1)
#### 题解
##### C++
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
       ListNode *slow = head, *fast = head;
       while(fast != nullptr){
            slow = slow->next;
            if(fast -> next == nullptr){
                return nullptr;
            }
            fast = fast->next->next;
            if(fast == slow){
                ListNode *ptr = head;
                while(ptr != slow){
                    slow = slow->next;
                    ptr = ptr->next;
                }
                return ptr;
            }
       }
       return nullptr;
    }

};
```
##### java
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null)
        {
            return null;
        }
        ListNode fast = head,slow = head;
        while(fast!=null){
            slow = slow.next;
            if(fast.next!=null){
                fast = fast.next.next;
            }else{
                return null;
            }
            if(fast == slow){
                ListNode ptr = head;
                while(ptr!=slow){
                    slow = slow.next;
                    ptr = ptr.next;
                }
                return ptr;
            }
        }
        return null;
    }
}
```

##### go
```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func detectCycle(head *ListNode) *ListNode {
    slow,fast := head,head
    for fast != nil {
        slow = slow.Next
        if fast.Next == nil {
            return nil
        }
        fast = fast.Next.Next
        if fast == slow {
            p := head
            for p != slow {
                p = p.Next
                slow = slow.Next
            }
            return p
        }
    }
    return nil
}
```










