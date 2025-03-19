### Finding the Smallest Missing Positive Integer in an Unsorted Array

**Problem Statement:**  
Given an unsorted array of integers, find the smallest missing positive integer. The solution should run in O(n) time and use O(1) space.

**Solution:**

To solve this problem in O(n) time and O(1) space, we can use the array itself to keep track of the presence of positive integers. Here's how:

1. **Segregate Positive and Non-Positive Numbers:**  
   First, we can ignore non-positive numbers (since we are only interested in positive integers). We can segregate positive numbers to the left side of the array and non-positive numbers to the right.

2. **Mark Presence of Positive Integers:**  
   For each positive number `x` in the array, we mark the presence of `x` by negating the value at index `x-1` (if it exists within the bounds of the array). This way, we use the array indices to represent the presence of positive integers.

3. **Find the Smallest Missing Positive Integer:**  
   After marking, the first index `i` where the value is still positive indicates that `i+1` is the smallest missing positive integer.

**Code Implementation:**

```cpp
#include <vector>
#include <algorithm>

int firstMissingPositive(std::vector<int>& nums) {
    int n = nums.size();
    int i = 0;
    while (i < n) {
        if (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
            std::swap(nums[i], nums[nums[i] - 1]);
        } else {
            i++;
        }
    }
    for (i = 0; i < n; i++) {
        if (nums[i] != i + 1) {
            return i + 1;
        }
    }
    return n + 1;
}
```

**Explanation:**

- **Step 1:** We move all positive numbers to the front of the array.
- **Step 2:** We use the array indices to mark the presence of positive integers by negating the value at the corresponding index.
- **Step 3:** We iterate through the array to find the first index where the value is still positive, which indicates the smallest missing positive integer.

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Checking if a Small String is a Subsequence of a Large String

**Problem Statement:**  
Given a large string and a small string, determine whether the small string is a subsequence of the large string. Optimize the solution.

**Solution:**

A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements. To check if the small string is a subsequence of the large string, we can use a two-pointer approach:

1. **Initialize Pointers:**  
   Use two pointers, one for the large string (`i`) and one for the small string (`j`).

2. **Traverse Both Strings:**  
   Traverse the large string with pointer `i` and the small string with pointer `j`. If the characters at both pointers match, move both pointers forward. If they don't match, only move the pointer `i` forward.

3. **Check Completion:**  
   If we reach the end of the small string (`j`), it means all characters of the small string were found in order in the large string, so it is a subsequence.

**Code Implementation:**

```cpp
#include <string>

bool isSubsequence(const std::string& small, const std::string& large) {
    int i = 0, j = 0;
    while (i < large.size() && j < small.size()) {
        if (large[i] == small[j]) {
            j++;
        }
        i++;
    }
    return j == small.size();
}
```

**Explanation:**

- We use two pointers to traverse both strings.
- If characters match, we move both pointers forward.
- If they don't match, we only move the pointer of the large string forward.
- If we reach the end of the small string, it is a subsequence.

**Time Complexity:** O(n), where `n` is the length of the large string.  
**Space Complexity:** O(1)

---

### Implementing a Circular Queue Using an Array

**Problem Statement:**  
Implement a circular queue using an array and explain its advantages over a normal queue.

**Solution:**

A circular queue is a linear data structure that follows the FIFO (First In First Out) principle, but the last position is connected back to the first position to make a circle. This allows efficient use of space.

**Advantages of Circular Queue Over Normal Queue:**

1. **Efficient Space Utilization:**  
   In a normal queue, once the rear reaches the end of the array, even if there is space at the front (due to dequeuing), new elements cannot be added. A circular queue overcomes this limitation by reusing the empty spaces.

2. **Better Performance:**  
   Circular queues avoid the need to shift elements when the queue is full, which is a common operation in a normal queue implemented with an array.

**Implementation:**

```cpp
#include <vector>
#include <iostream>

class CircularQueue {
private:
    std::vector<int> queue;
    int front, rear, k;

public:
    CircularQueue(int size) : k(size), front(-1), rear(-1) {
        queue.resize(k);
    }

    bool enqueue(int value) {
        if ((rear + 1) % k == front) {
            std::cout << "Queue is full\n";
            return false;
        }
        if (front == -1) {
            front = rear = 0;
        } else {
            rear = (rear + 1) % k;
        }
        queue[rear] = value;
        return true;
    }

    int dequeue() {
        if (front == -1) {
            std::cout << "Queue is empty\n";
            return -1;
        }
        int temp = queue[front];
        if (front == rear) {
            front = rear = -1;
        } else {
            front = (front + 1) % k;
        }
        return temp;
    }

    void display() {
        if (front == -1) {
            std::cout << "Queue is empty\n";
            return;
        }
        if (rear >= front) {
            for (int i = front; i <= rear; i++) {
                std::cout << queue[i] << " ";
            }
        } else {
            for (int i = front; i < k; i++) {
                std::cout << queue[i] << " ";
            }
            for (int i = 0; i <= rear; i++) {
                std::cout << queue[i] << " ";
            }
        }
        std::cout << "\n";
    }
};
```

**Explanation:**

- **Initialization:** We initialize the queue with a fixed size `k` and set `front` and `rear` to `-1`.
- **Enqueue:** We add elements to the queue. If the queue is full, we print a message. Otherwise, we update the `rear` pointer and add the element.
- **Dequeue:** We remove elements from the queue. If the queue is empty, we print a message. Otherwise, we update the `front` pointer and return the element.
- **Display:** We display the elements of the queue, handling the circular nature by checking if `rear` is behind `front`.

**Time Complexity:** O(1) for both enqueue and dequeue operations.  
**Space Complexity:** O(n), where `n` is the size of the queue.

---

### Detect Loop in a Circular Linked List

**Explanation:**  
To detect a loop in a circular linked list, we can use Floyd's Cycle Detection Algorithm (also known as the Tortoise and Hare algorithm). The idea is to use two pointers: a slow pointer that moves one step at a time and a fast pointer that moves two steps at a time. If there is a loop, the fast pointer will eventually meet the slow pointer inside the loop.

**Code:**

```cpp
#include <iostream>

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

bool hasCycle(ListNode* head) {
    if (!head || !head->next) return false; // Edge case: empty list or single node

    ListNode* slow = head; // Slow pointer (moves 1 step at a time)
    ListNode* fast = head; // Fast pointer (moves 2 steps at a time)

    while (fast && fast->next) {
        slow = slow->next;          // Move slow pointer by 1 step
        fast = fast->next->next;   // Move fast pointer by 2 steps
        if (slow == fast) {         // If they meet, there's a loop
            return true;
        }
    }
    return false; // No loop detected
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Reverse a Linked List in Groups of K Nodes

**Explanation:**  
To reverse a linked list in groups of `k` nodes, we use a recursive or iterative approach. Here, we use an iterative method. We maintain a dummy node to handle edge cases, and we reverse each group of `k` nodes by adjusting the pointers. If the remaining nodes are less than `k`, they are left as is.

**Code:**

```cpp
#include <iostream>

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

ListNode* reverseKGroup(ListNode* head, int k) {
    if (!head || k == 1) return head; // Edge case: empty list or k = 1

    ListNode* dummy = new ListNode(0); // Dummy node to handle edge cases
    dummy->next = head;
    ListNode* prev = dummy; // Pointer to the node before the current group
    ListNode* curr = dummy; // Pointer to traverse the list
    ListNode* next = nullptr; // Pointer to the next node in the group

    int count = 0;
    while (curr->next) {
        curr = curr->next;
        count++; // Count the total number of nodes
    }

    while (count >= k) {
        curr = prev->next; // Start of the current group
        next = curr->next; // Next node in the group
        for (int i = 1; i < k; i++) {
            curr->next = next->next; // Adjust pointers to reverse the group
            next->next = prev->next;
            prev->next = next;
            next = curr->next;
        }
        prev = curr; // Move prev to the end of the reversed group
        count -= k;  // Reduce the count by k
    }
    return dummy->next; // Return the new head
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Implement LRU Cache Using a Doubly Linked List and Hashing

**Explanation:**  
An LRU (Least Recently Used) Cache is a data structure that stores a fixed number of items. When the cache is full and a new item is added, the least recently used item is removed. We use a doubly linked list to maintain the order of usage and a hash map (unordered_map) for quick access to nodes.

**Code:**

```cpp
#include <iostream>
#include <unordered_map>

struct Node {
    int key, value;
    Node* prev;
    Node* next;
    Node(int k, int v) : key(k), value(v), prev(nullptr), next(nullptr) {}
};

class LRUCache {
private:
    int capacity;
    std::unordered_map<int, Node*> cache;
    Node* head;
    Node* tail;

    void addToFront(Node* node) {
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }

    void removeNode(Node* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void moveToHead(Node* node) {
        removeNode(node);
        addToFront(node);
    }

    Node* popTail() {
        Node* node = tail->prev;
        removeNode(node);
        return node;
    }

public:
    LRUCache(int cap) : capacity(cap) {
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        head->next = tail;
        tail->prev = head;
    }

    int get(int key) {
        if (cache.find(key) == cache.end()) return -1; // Key not found
        Node* node = cache[key];
        moveToHead(node); // Move the accessed node to the front
        return node->value;
    }

    void put(int key, int value) {
        if (cache.find(key) != cache.end()) {
            Node* node = cache[key];
            node->value = value; // Update the value
            moveToHead(node);    // Move to the front
        } else {
            if (cache.size() == capacity) {
                Node* node = popTail(); // Remove the least recently used node
                cache.erase(node->key);
                delete node;
            }
            Node* newNode = new Node(key, value);
            cache[key] = newNode;
            addToFront(newNode); // Add the new node to the front
        }
    }
};
```

**Time Complexity:** O(1) for both `get` and `put` operations.  
**Space Complexity:** O(capacity)

---

### Merge Two Sorted Linked Lists Without Using Extra Space

**Explanation:**  
To merge two sorted linked lists without using extra space, we use a dummy node and adjust the pointers of the existing nodes to create a merged list. We compare the values of the nodes from both lists and link them in ascending order.

**Code:**

```cpp
#include <iostream>

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode dummy(0); // Dummy node to simplify the merge process
    ListNode* tail = &dummy; // Pointer to the last node of the merged list

    while (l1 && l2) {
        if (l1->val < l2->val) {
            tail->next = l1;
            l1 = l1->next;
        } else {
            tail->next = l2;
            l2 = l2->next;
        }
        tail = tail->next;
    }

    // Attach the remaining nodes of l1 or l2
    if (l1) tail->next = l1;
    if (l2) tail->next = l2;

    return dummy.next; // Return the merged list
}
```

**Time Complexity:** O(n + m), where `n` and `m` are the lengths of the two lists.  
**Space Complexity:** O(1)

---

### Implement a Queue Using Only Stacks

**Explanation:**  
A queue can be implemented using two stacks. One stack (`stack1`) is used for enqueue operations, and the other stack (`stack2`) is used for dequeue operations. When dequeuing, if `stack2` is empty, all elements from `stack1` are transferred to `stack2` to reverse their order, making the oldest element the top of `stack2`.

**Code:**

```cpp
#include <stack>
#include <iostream>

class QueueUsingStacks {
private:
    std::stack<int> stack1; // For enqueue operations
    std::stack<int> stack2; // For dequeue operations

public:
    void enqueue(int x) {
        stack1.push(x); // Push to stack1 for enqueue
    }

    int dequeue() {
        if (stack2.empty()) {
            // Transfer all elements from stack1 to stack2
            while (!stack1.empty()) {
                stack2.push(stack1.top());
                stack1.pop();
            }
        }
        if (stack2.empty()) {
            std::cout << "Queue is empty\n";
            return -1;
        }
        int x = stack2.top(); // Pop from stack2 for dequeue
        stack2.pop();
        return x;
    }

    bool isEmpty() {
        return stack1.empty() && stack2.empty();
    }
};
```

**Time Complexity:**  
- **Enqueue:** O(1)  
- **Dequeue:** Amortized O(1) (worst case O(n) when `stack2` is empty)  
**Space Complexity:** O(n)

---

### Check if an Expression with Brackets is Balanced

**Explanation:**  
To check if an expression with brackets is balanced, we use a stack. We push opening brackets onto the stack and pop them when a corresponding closing bracket is encountered. If the stack is empty at the end, the expression is balanced.

**Code:**

```cpp
#include <stack>
#include <iostream>
#include <unordered_map>

bool isBalanced(const std::string& expression) {
    std::stack<char> stack;
    std::unordered_map<char, char> brackets = {
        {')', '('},
        {'}', '{'},
        {']', '['}
    };

    for (char ch : expression) {
        if (brackets.find(ch) != brackets.end()) {
            // Closing bracket
            if (stack.empty() || stack.top() != brackets[ch]) {
                return false;
            }
            stack.pop();
        } else if (brackets.count(ch)) {
            // Opening bracket
            stack.push(ch);
        }
    }
    return stack.empty(); // If stack is empty, the expression is balanced
}
```

**Time Complexity:** O(n), where `n` is the length of the expression.  
**Space Complexity:** O(n)

---

### Implement a Min Stack with O(1) Push and Pop Operations

**Explanation:**  
A min stack supports `push`, `pop`, and `getMin` operations in O(1) time. We use two stacks: one to store the actual values and another to store the minimum values at each step.

**Code:**

```cpp
#include <stack>
#include <iostream>

class MinStack {
private:
    std::stack<int> mainStack; // Stores actual values
    std::stack<int> minStack;  // Stores minimum values

public:
    void push(int x) {
        mainStack.push(x);
        if (minStack.empty() || x <= minStack.top()) {
            minStack.push(x); // Push to minStack if it's the new minimum
        }
    }

    void pop() {
        if (mainStack.empty()) {
            std::cout << "Stack is empty\n";
            return;
        }
        if (mainStack.top() == minStack.top()) {
            minStack.pop(); // Pop from minStack if the top element is the minimum
        }
        mainStack.pop();
    }

    int top() {
        if (mainStack.empty()) {
            std::cout << "Stack is empty\n";
            return -1;
        }
        return mainStack.top();
    }

    int getMin() {
        if (minStack.empty()) {
            std::cout << "Stack is empty\n";
            return -1;
        }
        return minStack.top(); // Get the current minimum
    }
};
```

**Time Complexity:**  
- **Push:** O(1)  
- **Pop:** O(1)  
- **Top:** O(1)  
- **GetMin:** O(1)  
**Space Complexity:** O(n)

---

### AVL Tree and Rotations

**Explanation:**  
An AVL Tree is a self-balancing Binary Search Tree (BST) where the difference between the heights of the left and right subtrees (balance factor) of any node is at most 1. If the balance factor exceeds 1, rotations are performed to restore balance. There are four types of rotations:
1. **Left Rotation (LL):** Performed when the right subtree is heavier.
2. **Right Rotation (RR):** Performed when the left subtree is heavier.
3. **Left-Right Rotation (LR):** A combination of left and right rotations.
4. **Right-Left Rotation (RL):** A combination of right and left rotations.

**Code:**

```cpp
#include <iostream>

struct Node {
    int key;
    Node* left;
    Node* right;
    int height;
    Node(int k) : key(k), left(nullptr), right(nullptr), height(1) {}
};

class AVLTree {
private:
    Node* root;

    int height(Node* node) {
        if (!node) return 0;
        return node->height;
    }

    int balanceFactor(Node* node) {
        if (!node) return 0;
        return height(node->left) - height(node->right);
    }

    Node* rightRotate(Node* y) {
        Node* x = y->left;
        Node* T2 = x->right;

        x->right = y;
        y->left = T2;

        y->height = std::max(height(y->left), height(y->right)) + 1;
        x->height = std::max(height(x->left), height(x->right)) + 1;

        return x;
    }

    Node* leftRotate(Node* x) {
        Node* y = x->right;
        Node* T2 = y->left;

        y->left = x;
        x->right = T2;

        x->height = std::max(height(x->left), height(x->right)) + 1;
        y->height = std::max(height(y->left), height(y->right)) + 1;

        return y;
    }

    Node* insert(Node* node, int key) {
        if (!node) return new Node(key);

        if (key < node->key)
            node->left = insert(node->left, key);
        else if (key > node->key)
            node->right = insert(node->right, key);
        else
            return node; // Duplicate keys not allowed

        node->height = 1 + std::max(height(node->left), height(node->right));

        int balance = balanceFactor(node);

        // Left Left Case
        if (balance > 1 && key < node->left->key)
            return rightRotate(node);

        // Right Right Case
        if (balance < -1 && key > node->right->key)
            return leftRotate(node);

        // Left Right Case
        if (balance > 1 && key > node->left->key) {
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }

        // Right Left Case
        if (balance < -1 && key < node->right->key) {
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }

        return node;
    }

public:
    AVLTree() : root(nullptr) {}

    void insert(int key) {
        root = insert(root, key);
    }
};
```

---

### Inorder Predecessor and Successor in a BST

**Explanation:**  
In a BST, the inorder predecessor of a node is the largest node in its left subtree, and the inorder successor is the smallest node in its right subtree. If the left or right subtree is empty, we traverse up the tree to find the predecessor or successor.

**Code:**

```cpp
#include <iostream>

struct Node {
    int key;
    Node* left;
    Node* right;
    Node(int k) : key(k), left(nullptr), right(nullptr) {}
};

void findPreSuc(Node* root, Node*& pre, Node*& suc, int key) {
    if (!root) return;

    if (root->key == key) {
        if (root->left) {
            Node* temp = root->left;
            while (temp->right)
                temp = temp->right;
            pre = temp;
        }
        if (root->right) {
            Node* temp = root->right;
            while (temp->left)
                temp = temp->left;
            suc = temp;
        }
        return;
    }

    if (root->key > key) {
        suc = root;
        findPreSuc(root->left, pre, suc, key);
    } else {
        pre = root;
        findPreSuc(root->right, pre, suc, key);
    }
}
```

---

### Convert a Binary Tree into its Mirror Image

**Explanation:**  
To convert a binary tree into its mirror image, we swap the left and right children of every node recursively.

**Code:**

```cpp
#include <iostream>

struct Node {
    int key;
    Node* left;
    Node* right;
    Node(int k) : key(k), left(nullptr), right(nullptr) {}
};

void mirror(Node* root) {
    if (!root) return;

    // Swap left and right children
    Node* temp = root->left;
    root->left = root->right;
    root->right = temp;

    // Recur for left and right subtrees
    mirror(root->left);
    mirror(root->right);
}
```

---

### Lowest Common Ancestor (LCA) in a Binary Tree

**Explanation:**  
The LCA of two nodes in a binary tree is the deepest node that has both nodes as descendants. We use a recursive approach to find the LCA.

**Code:**

```cpp
#include <iostream>

struct Node {
    int key;
    Node* left;
    Node* right;
    Node(int k) : key(k), left(nullptr), right(nullptr) {}
};

Node* findLCA(Node* root, int n1, int n2) {
    if (!root) return nullptr;

    if (root->key == n1 || root->key == n2)
        return root;

    Node* left = findLCA(root->left, n1, n2);
    Node* right = findLCA(root->right, n1, n2);

    if (left && right) return root; // Current node is LCA
    return left ? left : right; // Return non-null child
}
```

---

### Print the Top View of a Binary Tree

**Explanation:**  
The top view of a binary tree is the set of nodes visible when the tree is viewed from the top. We use a map to store the first node at each horizontal distance.

**Code:**

```cpp
#include <iostream>
#include <map>
#include <queue>

struct Node {
    int key;
    Node* left;
    Node* right;
    Node(int k) : key(k), left(nullptr), right(nullptr) {}
};

void topView(Node* root) {
    if (!root) return;

    std::map<int, int> m; // Horizontal distance -> node value
    std::queue<std::pair<Node*, int>> q; // Node and its horizontal distance
    q.push({root, 0});

    while (!q.empty()) {
        auto p = q.front();
        q.pop();
        Node* node = p.first;
        int hd = p.second;

        if (m.find(hd) == m.end())
            m[hd] = node->key;

        if (node->left) q.push({node->left, hd - 1});
        if (node->right) q.push({node->right, hd + 1});
    }

    for (auto& pair : m)
        std::cout << pair.second << " ";
}
```

---

### Threaded Binary Trees

**Explanation:**  
A Threaded Binary Tree is a binary tree where null pointers are replaced with threads to other nodes, enabling faster traversal without recursion or a stack. It is used to optimize inorder traversal.

**Code:**

```cpp
#include <iostream>

struct Node {
    int key;
    Node* left;
    Node* right;
    bool isThreaded; // True if right pointer is a thread
    Node(int k) : key(k), left(nullptr), right(nullptr), isThreaded(false) {}
};

Node* leftMost(Node* node) {
    while (node && node->left)
        node = node->left;
    return node;
}

void inorderThreaded(Node* root) {
    Node* curr = leftMost(root);
    while (curr) {
        std::cout << curr->key << " ";

        if (curr->isThreaded)
            curr = curr->right;
        else
            curr = leftMost(curr->right);
    }
}
```

---

These implementations in C++ cover the requested topics. Let me know if you need further clarification!