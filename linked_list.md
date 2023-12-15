# Linked List

---

## Approach - Tortoise and Hare

### 1. Find middle of a LL

[https://leetcode.com/problems/middle-of-the-linked-list](https://leetcode.com/problems/middle-of-the-linked-list)

```cpp
ListNode* middleNode(ListNode* head) {
    // Hare and tortoise both start at the same time,
    // but hare runs at double speed.
    ListNode* slow = head;
    ListNode* fast = head;
    while(fast != nullptr && fast->next != nullptr){
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

### 2. LL cycle

[https://leetcode.com/problems/linked-list-cycle-ii/](https://leetcode.com/problems/linked-list-cycle-ii/)

```cpp
ListNode *detectCycle(ListNode *head) {
    ListNode* slow = head;
    ListNode* fast = head;

    while(fast != NULL && fast->next != NULL){
        slow = slow->next;
        fast = fast->next->next;
        if(slow == fast){
            slow = head;
            while(slow != fast){
                slow = slow->next;
                fast = fast->next;
            }
            return slow;
        }
    }
    return NULL;
}
```

---

## Approach - Reverse LL

### 1. LL palindrome

[https://leetcode.com/problems/palindrome-linked-list/](https://leetcode.com/problems/palindrome-linked-list/)

```cpp
ListNode* findMid(ListNode* head){
    if(head == nullptr){
        return head;
    }
    ListNode* fast = head;
    ListNode* slow = head;
    while(fast->next != nullptr && fast->next->next != nullptr){
        fast = fast->next->next;
        slow = slow->next;
    }
    return slow;
}

ListNode* reverseLL(ListNode* head){
    ListNode* newHead = nullptr;
    while(head != nullptr){
        ListNode* next = head->next;
        head->next = newHead;
        newHead = head;
        head = next;
    }
    return newHead;
}

bool isPalindrome(ListNode* head) {
    ListNode* middle = findMid(head);
    middle->next = reverseLL(middle->next);
    middle = middle->next;

    while(head != nullptr && middle!=nullptr){
        if(head->val != middle->val){
            return false;
        }
        head = head->next;
        middle = middle->next;
    }
    return true;
}
```

---

## Approach - 2 pointer

### 1. Odd-Even LL

[https://leetcode.com/problems/odd-even-linked-list/](https://leetcode.com/problems/odd-even-linked-list/)

```cpp
ListNode* oddEvenList(ListNode* head) {
    if(head == NULL) return NULL;

    ListNode* odd = head;
    ListNode* even = head->next;
    ListNode* evenHead = even;

    while(even != NULL && even->next != NULL){
        odd->next = even->next;
        odd = odd->next;

        even->next = odd->next;
        even = even->next;
    }
    odd->next = evenHead;
    return head;
}
```

### 2. Remove nth node from the end

[https://leetcode.com/problems/remove-nth-node-from-end-of-list/](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

```cpp
ListNode* optimal(ListNode* head, int n){
    ListNode *newHead = new ListNode();
    newHead->next = head;

    ListNode *fast = newHead, *slow = newHead;
    while(fast != NULL && n > 0){
        fast = fast->next, n--;
    }
    while(fast != NULL && fast->next != NULL){
        fast = fast->next;
        slow = slow->next;
    }
    slow->next = slow->next->next;
    return newHead->next;
}

ListNode* removeNthFromEnd(ListNode* head, int n) {
    return optimal(head, n);
}
```

### 3. Intersection of two LL

[https://leetcode.com/problems/intersection-of-two-linked-lists/](https://leetcode.com/problems/intersection-of-two-linked-lists/)

```cpp
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    ListNode *tempHeadA = headA, *tempHeadB = headB;
    while(tempHeadA != NULL && tempHeadB != NULL){
        tempHeadA = tempHeadA->next;
        tempHeadB = tempHeadB->next;
    }

    if(tempHeadA == NULL) tempHeadA = headB;
    else tempHeadB = headA;

    while(tempHeadA != NULL && tempHeadB != NULL){
        tempHeadA = tempHeadA->next;
        tempHeadB = tempHeadB->next;
    }

    if(tempHeadA == NULL) tempHeadA = headB;
    else tempHeadB = headA;

    while(tempHeadA != tempHeadB){
        tempHeadA = tempHeadA->next;
        tempHeadB = tempHeadB->next;
    }
    return tempHeadA;
}
```

---

## Approach - n pointers

### 1. Sort LL of 0s, 1s and 2s

[https://www.codingninjas.com/studio/problems/sort-linked-list-of-0s-1s-2s_1071937](https://www.codingninjas.com/studio/problems/sort-linked-list-of-0s-1s-2s_1071937)

```cpp
Node* sortList(Node *head){
    Node* zero = new Node(), *one = new Node(), *two = new Node();
    Node* tempZero = zero, *tempOne = one, *tempTwo = two;

    while(head != NULL){
        if(head->data == 0){
            tempZero->next = head;
            tempZero = tempZero->next;
        }
        else if(head->data == 1){
            tempOne->next = head;
            tempOne = tempOne->next;
        }
        else{
            tempTwo->next = head;
            tempTwo = tempTwo->next;
        }
        head = head->next;
    }
    tempZero->next = one->next;
    tempOne->next = two->next;
    tempTwo->next = NULL;
    return zero->next;
}
```
