## 24 Swap Nodes in Pairs

**Link:** [24 Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

**Thoughts:**

* It's important to draw a draft simulating the entire process when there are multiple pointers and swaps.
* A dummy head as a useful tool. Make sure the dummy head connects to the original head before the loop (to consider the edge case where there is only one node in the original list)

**Simulation:** 

First, bridge tmp to current's next's next.

<img width="930" alt="Screen Shot 2023-06-12 at 15 08 37" src="https://github.com/WhaleOMO/Leetcode-Training/assets/91104491/487823f2-bd7e-4e9b-9be4-ff1181e3d9bb">

Second, swap cur and cur's next.

<img width="930" alt="Screen Shot 2023-06-12 at 15 10 35" src="https://github.com/WhaleOMO/Leetcode-Training/assets/91104491/10fc600f-1696-4bd4-9271-5c6ffab17533">


Finally, update pointers.

<img width="930" alt="Screen Shot 2023-06-12 at 15 10 57" src="https://github.com/WhaleOMO/Leetcode-Training/assets/91104491/84a588ca-78ee-4feb-a1d5-def8c5e81f05">

**Code:**

```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode();
        dummyHead->next = head;
        ListNode* cur = head;
        ListNode* nxt = nullptr;
        ListNode* tmp = dummyHead;
        while(cur!=nullptr && cur->next!=nullptr)
        {
            tmp->next = cur->next;
            nxt = cur->next->next;
            cur->next->next = cur;
            cur->next = nxt;
            tmp = cur;
            cur = cur->next;
        }
        return dummyHead->next;
    }
};
```



**Time Complexity:** O(n)

**Space Complexity:** O(1)

<br/>

## 19 Remove Nth Node From End

**Link:** [19 Remove Nth Node From End](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

**Thoughts:**

* How to get to the nth node from the end? Using two pointers and a dummy head node, slow and fast pointers start at the dummy head, fast go first n steps, and then slow and fast go together until fast reaches null.
* Why n++? If we want to remove the nth node from the end, we need the pointer to be at (n+1)th node from the end so that we can skip the connection between n+1 and n and gap n+1 with n-1 th node from the end;

**Code:**

```c++
lass Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode();
        ListNode* slow = dummyHead;
        ListNode* fast = dummyHead;
        dummyHead->next = head;

        n++;    // n+1 for we want slow to end at n+1 th node from the end 
        while(n-- && fast!=nullptr)
        {
            fast = fast->next;  // fast go n+1 steps
        }

        while(fast!=nullptr)
        {
            slow = slow->next;  // slow and fast go together untill fast is null
            fast = fast->next;
        }

        ListNode* tmp = slow->next;
        slow->next = slow->next->next;
        delete tmp;
        return dummyHead->next;
    }
};
```

**Time Complexity:** O(n)

**Space Complexity:** O(1)

<br/>

## 160 Intersection of Two LinkedList

**Link:** [160 Intersection of Two LinkedList](https://leetcode.com/problems/intersection-of-two-linked-lists/)

**Thoughts:**

* Solution 1: two pointers traverse both lists, get their length, and find the length difference *d*. Reset the pointer for the long list to the *d*th node while the pointer for the shorter list to the head. Start traversing both lists and compare their nodes (return if same) until they both reach null (then return null)
* Solution 2: two pointers, both traverse twice; after the first traverse, traverse from the other list and compare nodes along the way. Since both pointers traverse both lists, if two lists intersect, they will definitely meet. If two lists don't intersect, the two pointers will also meet (at the end since they both will point to null).  See Video [Leetcod160](https://www.youtube.com/watch?v=IpBfg9d4dmQ&ab_channel=NickWhite)

**Code:**

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curA = headA;
        ListNode* curB = headB;

        int lenA = 0, lenB = 0;
        while(curA){
            curA = curA->next;
            lenA++;
        }

        while(curB)
        {
            curB = curB->next;
            lenB++;
        }

        int diff = abs(lenA-lenB);

        if(lenB>lenA)
        {
            curA = headB;
            curB = headA;
        }else{
            curA = headA;
            curB = headB;
        }

        // now cur A is (diff) longer than B
        // so move curA to diff th node
        while(diff--)
        {
            curA = curA->next;
        }

        while(curA)
        {
            if(curA == curB)
            {
                return curA;
            }
            curA = curA->next;
            curB = curB->next;
        }

        return nullptr;
    }
};
```

**Time Complexity:** O(m+n)

**Space Complexity:** O(1)

<br/>

## 142 Linked List Cycle II

**Link:** [142 Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

**Thoughts:**

* Slow and Fast pointers
* If there is a circle, slow and fast pointers will eventually meet **in the circle**. Why? Because their **speed difference is only 1**, so when they both enter the circle loop, fast will definitely crash with slow (Alternatively, Imagine the circle is being cut from somewhere and unfolded, this becomes a chasing problem, speed diff is 1, so fast will not move across slow).

![linkedlistcycle](https://github.com/WhaleOMO/Leetcode-Training/assets/91104491/a9aeb96c-0c9b-425f-8de1-b092a978b8b9)

* Ok, but how to find the beginning of the circle? When fast and slow pointer meets, move one of the pointers to the start of the linked list and then traverse both pointers by one every step; when they meet again, that will be the beginning point of the circle.<img width="742" alt="142 环形链表" src="https://github.com/WhaleOMO/Leetcode-Training/assets/91104491/48734efc-1dc0-48cc-9386-9afde576aa16">


  *Math*: 

  * let ``x `` be the length to the beginning of the circle, `` y `` be the distance slow traveled after entering the loop to meet fast, and ``z`` be the circle's length minus y.

  * when slow and fast meets, slow traveled ``x+y`` , so we know fast traveled ``2(x+y)``. And since fast entered the loop first and has gone at least one round, we can tell that 

    ``2(x + y) = x + y + n(y + z)``, where n is the number of loops fast have traversed in side the circle.

  * Simplify the equation to ``x + y = n(y + z)`` , from here, ``x = n(y + z) - y`` , which can be written as ``x = (n-1)(y + z) + z`` 

  * since ``n>=1``, we can tell that this equation indicates that when fast meets slow, fast need z more steps to meet a pointer that start from the beginning of the list, and that position is the x we are looking for.

**Code:**

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast!=nullptr and fast->next!=nullptr)
        {
            fast = fast->next->next;
            slow = slow->next;
            // slow meets fast, there is a circle
            if(fast == slow)
            {
                // reset one pointer to start
                // traverse slow and fast with pace 1
                // they will meet at the beginning of the circle
                slow = head;
                while(slow != fast)
                {
                    slow = slow->next;
                    fast = fast->next;
                }
                return slow;
            }
        }
        return nullptr;
    }
};
```

**Time Complexity:** O(n)

**Space Complexity:** O(1)







