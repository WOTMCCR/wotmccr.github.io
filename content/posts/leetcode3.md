---
title: 2024-05-10刷题3
tags:
  - leetcode
  - 链表
description: 203.移除链表元素 707.设计链表 206.反转链表
date: 2024-05-10
time: 22:49
---
## 2024-05-10
### 203.移除链表元素
#### 思路
使用一个虚拟头节点，来简化操作的逻辑，将dummyhead指向头节点，然后，使用temp进行遍历，当temp的next的val为val时候，就将next指向`next->next`
如果不等，则向后移动temp指针
#### 复杂度
- 时间复杂度：O(n)，其中 n 是链表的长度。需要遍历链表一次。
- 空间复杂度：O(1)。
#### 注意⚠️
也可能忘记了C++的创建类指针的操作
```C++
ListNode *dummyhead = new ListNode(0,head);
```
注意使用虚拟头节点，非常常见
```C++
        ListNode *dummyhead = new ListNode(0,head);
        ListNode *temp = dummyhead;
```

#### 题解
##### C++
```C++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode *dummyhead = new ListNode(0,head);
        ListNode *temp = dummyhead;
        while(temp->next != nullptr)
        {
        
            if(temp->next->val == val){
                ListNode *d = temp->next;
                temp->next = temp->next->next;
                delete d;
            }else{
                temp = temp->next;
            }
        }
        return dummyhead->next;
    }
};
```
##### java
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummyhead = new ListNode(0,head);
        ListNode temp = dummyhead;
        while(temp.next!= null){
            if(temp.next.val == val)
            {
                temp.next = temp.next.next;                
            }else{
                temp=temp.next;
            }
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
func removeElements(head *ListNode, val int) *ListNode {
    dummyHead := &ListNode{Next:head}
    for temp := dummyHead; temp.Next != nil; {
        if temp.Next.Val == val {
            temp.Next = temp.Next.Next
        } else {
            temp = temp.Next
        }
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
    func removeElements(_ head: ListNode?, _ val: Int) -> ListNode? {
        let dummyNode = ListNode(0,head)
        var temp = dummyNode
        while let curNext = temp.next {
            if curNext.val == val {
                temp.next = curNext.next
            }else{
                temp = curNext
            }
        }
        return dummyNode.next
    }
}
```

### 707.设计链表
#### 注意⚠️
对边界条件以及元素前后的处理
#### 题解
##### C++
```C++
class MyLinkedList {
private:
    int size;
    ListNode *head;
public:
    MyLinkedList() {
        this->size = 0;
        this->head = new ListNode(0);
    }
    
    int get(int index) {
        if(index<0||index>=size)
            return -1;
        ListNode *p = head;
        for(int i=0;i<=index;i++)
            p = p->next;
        return p->val;
    }
    
    void addAtHead(int val) {
        size++;
        ListNode *h = new ListNode(val,head->next);
        head->next = h;
        // addAtIndex(0,val);
    }
    
    void addAtTail(int val) {
        size++;
        ListNode *tail = new ListNode(val,nullptr);
        ListNode *p = head;
        while(p->next!=NULL)
        {
            p = p->next;
        }
        p->next = tail;
    }
    
    void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        index = max(0,index);
        size++;
        ListNode *pred = head;
        for(int i=0;i<index;i++)
            pred = pred->next;

        ListNode *add = new ListNode(val,pred->next);
        pred->next = add;

    }
    
    void deleteAtIndex(int index) {
        if(index<0||index>=size)
            return ;
        ListNode *p = head;        
        for(int i=0;i<index;i++)
            p = p->next;
        ListNode *temp = p->next;
        p->next = p->next->next;
        delete temp;
        size--;
    }
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```
##### java
```java
class MyLinkedList {
    int size;
    ListNode head;

    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }
    
    public int get(int index) {
        if(index<0 || index>=size){
            return -1;
        }
        ListNode cur = head;
        for(int i = 0 ; i<=index;i++){
            cur = cur.next;
        }
        return cur.val;
    }
    
    public void addAtHead(int val) {
        addAtIndex(0,val);
    }
    
    public void addAtTail(int val) {
        addAtIndex(size,val);
    }
    
    public void addAtIndex(int index, int val) {
        if(index > size){
            return;
        }
        index = Math.max(0,index);
        size++;
        ListNode p = head;
        for(int i = 0;i<index;i++){
            p = p.next;
        }
        ListNode node = new ListNode(val);
        node.next = p.next;
        p.next = node;
    }
    
    public void deleteAtIndex(int index) {
        if(index >= size || index < 0){
            return;
        }
        size--;
        ListNode p = head;
        for(int i = 0; i<index;i++){
            p=p.next;
        }
        p.next = p.next.next;
    }
}
```
##### go
```go
type MyLinkedList struct {
    head *ListNode
    size int
}


func Constructor() MyLinkedList {
    return MyLinkedList{&ListNode{},0}
}


func (this *MyLinkedList) Get(index int) int {
    if index < 0 || index >= this.size {
        return -1
    }
    cur := this.head
    for i := 0; i<=index ;i++ {
        cur = cur.Next
    }
    return cur.Val
}


func (this *MyLinkedList) AddAtHead(val int)  {
    this.AddAtIndex(0,val);
}


func (this *MyLinkedList) AddAtTail(val int)  {
    this.AddAtIndex(this.size,val);
}


func (this *MyLinkedList) AddAtIndex(index int, val int)  {
    if index > this.size {
        return
    }
    index = max(index,0)
    this.size++
    p := this.head
    for i:=0 ; i<index ;i++ {
        p = p.Next
    }
    node := &ListNode{val, p.Next}
    p.Next = node
}


func (this *MyLinkedList) DeleteAtIndex(index int)  {
    if index >= this.size || index < 0 {
        return
    }
    this.size--
    p := this.head
    for i := 0 ; i<index ;i++ {
        p = p.Next
    }
    p.Next = p.Next.Next

}


/**
 * Your MyLinkedList object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Get(index);
 * obj.AddAtHead(val);
 * obj.AddAtTail(val);
 * obj.AddAtIndex(index,val);
 * obj.DeleteAtIndex(index);
 */
```
##### swift
```swift
class ListNode<T> {
    var value: T
    var next: ListNode?

    init(_ value: T) {
        self.value = value
        self.next = nil
    }
}

class MyLinkedList {
    var dummyHead: ListNode<Int>?
    var size : Int

    init() {
        dummyHead = ListNode(0)
        size = 0
    }
    
    func get(_ index: Int) -> Int {
        if index >= size || index < 0 {
            return -1
        }

        var curNode = dummyHead?.next
        var curIndex = index

        while curIndex > 0 {
            curNode = curNode?.next
            curIndex -= 1
        }
        return curNode?.value ?? -1
    }
    
    func addAtHead(_ val: Int) {
        let newHead = ListNode(val)
        newHead.next = dummyHead?.next
        dummyHead?.next = newHead
        size += 1
    }
    
    func addAtTail(_ val: Int) {
        let newNode = ListNode(val)
        var curNode = dummyHead
        while curNode?.next != nil {
            curNode = curNode?.next
        }
        curNode?.next = newNode
        size += 1
    }
    
    func addAtIndex(_ index: Int, _ val: Int) {
        if index > size {
            return
        }
        let newNode = ListNode(val)
        var curNode = dummyHead
        var curIndex = index
        while curIndex > 0 {
            curNode = curNode?.next
            curIndex -= 1
        }
        newNode.next = curNode?.next
        curNode?.next = newNode
        size += 1
    }
    
    func deleteAtIndex(_ index: Int) {
        if index >= size || index < 0 {
            return
        }        
        var curNode = dummyHead
        for _ in 0..<index {
            curNode = curNode?.next        
        }

        curNode?.next = curNode?.next?.next
        size -= 1
    }
}

```

### 206.反转链表
#### 思路
保存下一个
将当前的下一个指向prev
将prev移到当前的位置
当前的位置移动到下一个
#### 复杂度
- 时间复杂度：O(n)，其中 n 是链表的长度。需要遍历链表一次。
- 空间复杂度：O(1)。
#### 题解
##### C++
```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *prev = nullptr;
        ListNode *cur = head;
        while(cur){
            ListNode *next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
};
```
##### java
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode cur = head;
        ListNode prev = null;
        while(cur!=null){
            ListNode next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
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
func reverseList(head *ListNode) *ListNode {
    cur := head
    var prev *ListNode
    for cur != nil {
        next := cur.Next
        cur.Next = prev
        prev = cur
        cur = next
    }
    return prev
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
    func reverseList(_ head: ListNode?) -> ListNode? {
        if head == nil || head?.next == nil {
            return head
        }
        var pre: ListNode? = nil
        var cur: ListNode? = head
        var next: ListNode? = nil
        while cur != nil {
            next = cur?.next
            cur?.next = pre
            pre = cur
            cur = next
        }
        return pre
    }
}
```










