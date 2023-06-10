## Linked List

* Compares to Array, Linked List is best when there is a high frequency of insert/delete and low frequency of search: 
  * Length is easily changeable, insert or delete has O(1) complexity
  * Searching is O(n)

![链表-链表与数据性能对比](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806195200276.png)

* To define a basic LinkedList structure in C++

  ```C++
  struct ListNode {
      int val;  // the element stored in this node
      ListNode *next;  // the pointer to the next node
      ListNode(int x) : val(x), next(NULL) {}  // construction method for structure
  };
  ```

  

## 203 Remove Linked List Elements

**Link:** [203 Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements)

**Thoughts:**

* Use a dummy head node => no need for special case if the original node should be deleted
* delete the node in C++ manually to free up memory...

**Code:**

```c++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        // create a dummy head node and point to original head
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        
        ListNode* cur = dummyHead;
        while(cur->next!=nullptr)
        {
            if(cur->next->val == val)
            {
                ListNode* tmp = cur->next;
                cur->next = cur->next->next;
                delete tmp;     // freeing memory after "delete"
            }else{
                cur = cur->next;
            }
        }
        return dummyHead->next;
    }
};
```



**Time Complexity:** O(n)

**Space Complexity:** O(1)



<br/>

## 707 Design Linked List

**Link:** [707 Design Linked List](https://leetcode.com/problems/design-linked-list)

**Thoughts:** 

* Use of a dummy head is very helpful for increasing / deleting and searching
* When deleted an element, also set the temp pointer to that element  ``nullptr`` or it will points to an random location (which might be bad)

**Code:**

```C++
class MyLinkedList {
public:
    // node structure
    struct LinkedListNode
    {
        int val;
        LinkedListNode* next;
        LinkedListNode(int val): val(val), next(nullptr){}
    };

    MyLinkedList() {
        _dummyHead = new LinkedListNode(0);
        _size = 0;
    }
    
    int get(int index) {
        if(index < 0 || index > (_size-1))
        {
            return -1;
        }
        LinkedListNode* cur = _dummyHead->next;
        while(index)
        {
            cur = cur->next;
            index--;
        }
        return cur->val;
    }
    
    void addAtHead(int val) {
        LinkedListNode* newNode = new LinkedListNode(val);
        newNode->next = _dummyHead->next;
        _dummyHead->next = newNode;
        _size++;
    }
    
    void addAtTail(int val) {
        LinkedListNode* newNode = new LinkedListNode(val);
        LinkedListNode* cur = _dummyHead;
        while(cur->next != nullptr)
        {
            cur = cur->next;
        }
        cur->next = newNode;
        _size++;
    }
    
    void addAtIndex(int index, int val) {
        if(index < 0 || index > _size)
        {
            return;
        }

        LinkedListNode* newNode = new LinkedListNode(val);
        LinkedListNode* cur = _dummyHead;
        // get to the node before the index node
        while(index)
        {
            cur = cur->next;
            index--;
        }
        
        newNode->next = cur->next;
        cur->next = newNode;
        _size++;
    }
    
    void deleteAtIndex(int index) {
        if(index < 0 || index >= _size)
        {
            return;
        }

        LinkedListNode* cur = _dummyHead;
        // get to the node before the index node
        while(index)
        {
            cur = cur->next;
            index--;
        }

        LinkedListNode* tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;
        // delete frees up the memory but the pointer now points to a random location
        // so let the tmp pointer to be a null ptr
        tmp = nullptr;
        _size--;
    }

private:
    LinkedListNode* _dummyHead;
    int _size;
};
```



**Time Complexity:** 

* Get: worst case O(n)
* Add at Head: O(1)
* Add at Tail: O(n)
* Add at Index: worst case O(n)
* Delete at Index: worst case O(n)

**Space Complexity**: O(n)

<br/>

## 206 Reverse Linked List

**Link: ** [206 Reverse Linked List](https://leetcode.com/problems/reverse-linked-list)

**Thoughts: ** 

* two pointers, one pointer for storing previous node, one for storing current node
* reverse each node to prev, untill cur has no next

![img](https://code-thinking.cdn.bcebos.com/gifs/206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.gif)

**Code:** 

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* cur = head;
        ListNode* prev = nullptr;
        while(cur!=nullptr)
        {
            ListNode* tmp = cur->next;
            cur->next = prev;       // reverse current
            prev = cur;             // move prev pointer right
            cur = tmp;              // move cur pointer right
        }
        return prev;
    }
};
```



**Time Complexity: ** O(n)

**Space Complexity:** O(1)

