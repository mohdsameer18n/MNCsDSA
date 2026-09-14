# 🔐 Locking The Tree of Space

A Java implementation of a locking system on an **M-Ary Tree**.

The problem requires implementing three operations:

```text
1. lock(X, uid)
2. unlock(X, uid)
3. upgradeLock(X, uid)
```

The solution uses:

* `HashMap` for fast node lookup
* `parent` pointer for ancestor checking
* `HashSet` for maintaining locked descendants
* `Queue` for constructing the M-Ary tree from level-order input

The original problem requires efficient operations because `N` and `Q` can be as large as `5 × 10^5`.

---

# 📌 Problem Statement

We have a world map represented as an **M-Ary Tree**.

An M-Ary tree is a tree where each node can have at most `m` children.

A binary tree is a special case where:

```text
m = 2
```

A ternary tree is:

```text
m = 3
```

The tree is connected and acyclic, and there is a unique path between the root and every other node.

---

# 🎯 Operations

We need to implement three operations:

```text
lock(X, uid)
unlock(X, uid)
upgradeLock(X, uid)
```

Where:

* `X` = unique node name
* `uid` = user performing the operation

The operation definitions are described below.

---

# 1️⃣ Lock

```java
lock(X, uid)
```

The lock operation provides exclusive access to the subtree rooted at `X`.

A lock succeeds only when:

1. `X` itself is not locked.
2. No descendant of `X` is locked.
3. No ancestor of `X` is locked.

Once `X` is locked:

```text
lock(descendant, anyUser) → false
lock(ancestor, anyUser)   → false
lock(X, anyUser)          → false
```

This follows the exclusive-access requirement of the problem.

---

# 2️⃣ Unlock

```java
unlock(X, uid)
```

Unlocks node `X`.

Only the same user who originally locked the node can unlock it.

For example:

```text
lock(China, 9)       → true
unlock(China, 9)     → true
unlock(China, 10)    → false
```

The unlock operation fails when:

* The node is not locked.
* The supplied `uid` does not match the user who owns the lock.

---

# 3️⃣ Upgrade Lock

```java
upgradeLock(X, uid)
```

Upgrade allows a user to move their locks from descendants to an ancestor node.

The operation succeeds only if:

1. `X` is not already locked.
2. `X` has at least one locked descendant.
3. No ancestor of `X` is locked.
4. All locked descendants of `X` are locked by the same `uid`.

After a successful upgrade:

```text
locked descendants
        ↓
    unlocked
        ↓
X becomes locked
```

The upgrade operation must not violate the consistency rules of `lock` and `unlock`.

---

# 🌳 Example Tree

For:

```text
N = 7
m = 2
```

and the node names:

```text
World
Asia
Africa
China
India
South Africa
Egypt
```

the tree is:

```text
                 World
              /         \
           Asia         Africa
          /    \        /     \
      China   India  South    Egypt
                     Africa
```

---

# 📥 Input Format

```text
N
m
Q
NodeName1
NodeName2
...
NodeNameN
OperationType NodeName UserId
...
```

Where:

| Input      | Meaning                       |
| ---------- | ----------------------------- |
| `N`        | Number of nodes               |
| `m`        | Number of children per node   |
| `Q`        | Number of queries             |
| `NodeName` | Unique node name              |
| `UserId`   | User performing the operation |

Operation types:

```text
1 → lock
2 → unlock
3 → upgradeLock
```

The problem defines the query format as `Operation Type NodeName UserId`.

---

# 🧪 Example Input

```text
7
2
5
World
Asia
Africa
China
India
SouthAfrica
Egypt
1 China 9
1 India 9
3 Asia 9
2 India 9
2 Asia 9
```

---

# 📤 Example Output

```text
true
true
true
false
true
```

---

# 🔎 Example Dry Run

## Query 1

```text
1 China 9
```

`China` is initially unlocked.

There are:

* No locked descendants.
* No locked ancestors.

Therefore:

```text
true
```

`China` becomes locked by user `9`.

---

## Query 2

```text
1 India 9
```

`India` is also unlocked.

There are no locked ancestors or descendants.

Therefore:

```text
true
```

Now:

```text
                 World
              /         \
           Asia         Africa
          /    \
      [China] [India]
```

Both nodes are locked by user `9`.

---

## Query 3

```text
3 Asia 9
```

This is:

```text
upgradeLock(Asia, 9)
```

`Asia`:

* Is not locked.
* Has locked descendants.
* Has no locked ancestor.
* All locked descendants belong to user `9`.

Therefore the upgrade succeeds.

The operation:

```text
China → unlocked
India → unlocked
Asia  → locked by user 9
```

Result:

```text
true
```

---

## Query 4

```text
2 India 9
```

`India` was unlocked during the upgrade.

Therefore:

```text
unlock(India, 9) → false
```

---

## Query 5

```text
2 Asia 9
```

`Asia` is currently locked by user `9`.

Therefore:

```text
unlock(Asia, 9) → true
```

Final output:

```text
true
true
true
false
true
```

This is the example behavior specified in the problem.

---

# 🧠 Data Structures Used

## 1. `HashMap`

```java
Map<String, Node> nodes = new HashMap<>();
```

Used to find a node by its name.

```java
Node node = nodes.get(name);
```

Average lookup:

```text
O(1)
```

---

## 2. Parent Pointer

Every node stores:

```java
Node parent;
```

This lets us move upward:

```text
Node
 ↓
Parent
 ↓
Grandparent
 ↓
...
 ↓
Root
```

It is used to check whether an ancestor is locked.

---

## 3. Children List

Every node stores:

```java
List<Node> children = new ArrayList<>();
```

This represents the M-Ary tree structure.

---

## 4. Locked State

Each node contains:

```java
boolean locked = false;
int lockedBy = -1;
```

For example:

```text
locked = true
lockedBy = 9
```

means user `9` currently owns the lock.

---

## 5. Locked Descendants

The important optimization is:

```java
Set<Node> lockedDescendants = new HashSet<>();
```

This stores all currently locked nodes below the current node.

Therefore, instead of traversing the entire subtree:

```java
if (!node.lockedDescendants.isEmpty())
```

can immediately tell us whether there is a locked descendant.

The `lockedDescendants` approach is the key optimization used by this implementation.

---

# ⚡ Why `lockedDescendants`?

Without this set, suppose we have:

```text
                  A
              /   |   \
             B    C    D
            /|\   ...
```

To determine whether `A` has a locked descendant, we might have to traverse a large part of the subtree.

With:

```java
A.lockedDescendants
```

we can simply check:

```java
A.lockedDescendants.isEmpty()
```

When a node is locked, it is added to all of its ancestors:

```text
lock(X)
   ↓
add X to parent's lockedDescendants
   ↓
add X to grandparent's lockedDescendants
   ↓
...
```

When it is unlocked:

```text
unlock(X)
   ↓
remove X from parent's lockedDescendants
   ↓
remove X from grandparent's lockedDescendants
   ↓
...
```

---

# ⏱️ Complexity

Let:

```text
N = number of nodes
m = number of children
K = number of locked descendants
```

Since the tree is balanced:

```text
Height = O(log_m N)
```

Therefore:

| Operation         |       Complexity |
| ----------------- | ---------------: |
| `lock()`          |     `O(log_m N)` |
| `unlock()`        |     `O(log_m N)` |
| `upgradeLock()`   | `O(K × log_m N)` |
| Tree construction |           `O(N)` |
| Node lookup       |   `O(1)` average |

These are the target complexities given in the problem.

---

# 🔄 How `lock()` Works

```text
                 lock(X, uid)
                      |
                      ↓
              Is X already locked?
                 /          \
              YES            NO
               |              |
            return false      ↓
                      Has locked descendant?
                         /          \
                       YES           NO
                        |             |
                     false            ↓
                              Check ancestors
                               /          \
                            Locked       None locked
                              |              |
                           false             ↓
                                      Lock X
                                          |
                                          ↓
                              Update ancestor sets
                                          |
                                          ↓
                                       true
```

---

# 🔓 How `unlock()` Works

```text
              unlock(X, uid)
                    |
                    ↓
              Is X locked?
               /       \
             NO         YES
             |           |
          false          ↓
                  Is uid the owner?
                    /       \
                  NO         YES
                  |           |
               false          ↓
                         Unlock X
                            |
                            ↓
                  Update ancestors
                            |
                            ↓
                          true
```

---

# ⬆️ How `upgradeLock()` Works

```text
             upgradeLock(X, uid)
                       |
                       ↓
                Is X already locked?
                  /           \
                YES             NO
                 |               |
              false              ↓
                        Has locked descendants?
                          /             \
                        NO               YES
                        |                 |
                     false                ↓
                                Check ancestors
                                      |
                                      ↓
                           Check locked descendants
                                      |
                              Are all owned by uid?
                                  /        \
                                NO          YES
                                |            |
                              false          ↓
                                   Unlock descendants
                                          |
                                          ↓
                                      Lock X
                                          |
                                          ↓
                                        true
```

---

# 🏗️ Tree Construction

The input provides node names in **level order**.

For example:

```text
World
Asia
Africa
China
India
SouthAfrica
Egypt
```

For `m = 2`:

```text
                 World
                /     \
             Asia     Africa
            /   \     /    \
         China India SouthAfrica Egypt
```

The implementation uses:

```java
Queue<Node> queue = new LinkedList<>();
```

The root is inserted first.

Then each parent is removed from the queue and up to `m` children are assigned.

---

# 🔐 Important Invariant

The most important rule is:

> **A locked node cannot have a locked ancestor or a locked descendant.**

Example:

```text
        A
       / \
      B   C
     /
    D
```

If:

```text
lock(B, 1)
```

succeeds:

```text
lock(A, 2) → false
lock(D, 2) → false
lock(B, 2) → false
```

But:

```text
lock(C, 2) → true
```

because `C` is neither an ancestor nor a descendant of `B`.

---

# 📌 Important Upgrade Detail

Suppose:

```text
        A
       / \
      B   C
     / \
    D   E
```

and:

```text
lock(D, 10)
lock(E, 10)
```

Then:

```text
upgradeLock(B, 10)
```

is successful.

After upgrade:

```text
        A
       / \
     [B]  C
     / \
    D   E
```

where:

```text
B = locked by 10
D = unlocked
E = unlocked
```

If instead:

```text
lock(D, 10)
lock(E, 20)
```

then:

```text
upgradeLock(B, 10)
```

fails because the locked descendants belong to different users.

---

# 📋 Constraints

The problem specifies:

```text
1 < N < 5 × 10^5
1 < m < 30
1 < Q < 5 × 10^5
1 < length(NodeName) < 20
```

Because `N` and `Q` are large, the solution must avoid repeatedly traversing complete subtrees.

---

# 💡 Interview Points

### Why use `HashMap`?

For average `O(1)` node lookup.

### Why use `parent`?

To check ancestors in `O(height)`.

### Why use `HashSet`?

To efficiently maintain locked descendants.

### Why is the tree balanced important?

It gives:

```text
height = O(log_m N)
```

instead of potentially `O(N)`.

### Why copy `lockedDescendants` during upgrade?

Because `unlock()` modifies the set while we are processing it.

Therefore:

```java
List<Node> descendants =
        new ArrayList<>(node.lockedDescendants);
```

creates a snapshot.

---

# ⚠️ Thread Safety

The basic implementation below is designed for the sequential version.

It is **not thread-safe**.

If multiple threads can execute `lock()`, `unlock()`, or `upgradeLock()` simultaneously, the check and update operations must be made atomic.

For example, this sequence:

```text
Check ancestors
      ↓
Check descendants
      ↓
Lock node
      ↓
Update ancestors
```

must not be interrupted by another conflicting operation.

Possible Java mechanisms include:

```java
synchronized
```

or:

```java
ReentrantLock
```

The problem's review also identifies synchronization/concurrent-operation handling as an important consideration.

---

# 🌍 Real-World Use Cases

The locking-tree concept can be applied to hierarchical resources.

## 1. Transaction Processing

Different nodes can represent transaction stages.

Locking a node prevents conflicting operations on related stages.

---

## 2. User Session Management

Nodes can represent resources or sessions.

A lock prevents conflicting users from modifying the same resource simultaneously.

---

## 3. Resource Allocation

Nodes can represent:

```text
CPU
Memory
Storage
Servers
```

Locking a resource prevents multiple tasks from allocating it simultaneously.

---

## 4. Database Operations

A hierarchy could represent:

```text
Database
   ↓
Table
   ↓
Row
   ↓
Record
```

Locks can prevent conflicting modifications.

---

## 5. Payment Gateway Integration

For a payment system, a hierarchy could represent:

```text
Payment
   |
   +--- Gateway
   |      |
   |      +--- Visa
   |      +--- Mastercard
   |
   +--- Authorization
   |
   +--- Settlement
```

A locking mechanism can help prevent conflicting access to shared resources.

---

# 💳 Payment Gateway Example

### Selection and Locking

Customer A selects a payment gateway.

The corresponding gateway node can be locked.

```text
Gateway
   |
  [Visa]
```

This prevents conflicting operations on the same gateway.

### Concurrent Access Prevention

While the transaction is being processed, the relevant node remains locked.

### Controlled Resource Allocation

The lock can represent exclusive access to resources associated with the payment gateway.

### Unlock

After processing finishes, the node can be unlocked and made available again.

---

# 📂 Project Structure

A simple project structure:

```text
LockingTree/
│
├── LockingTree.java
└── README.md
```

---

# 💻 Complete Java Implementation

```java
import java.util.*;

public class LockingTree {

    static class Node {
        String name;
        Node parent;
        List<Node> children = new ArrayList<>();

        boolean locked = false;
        int lockedBy = -1;

        // All currently locked nodes in this node's subtree,
        // excluding the node itself.
        Set<Node> lockedDescendants = new HashSet<>();

        Node(String name) {
            this.name = name;
        }
    }

    private final Map<String, Node> nodes = new HashMap<>();

    // -------------------------------------------------------
    // LOCK
    // -------------------------------------------------------
    public boolean lock(String name, int uid) {

        Node node = nodes.get(name);

        // 1. Node itself already locked
        if (node.locked) {
            return false;
        }

        // 2. Has locked descendants
        if (!node.lockedDescendants.isEmpty()) {
            return false;
        }

        // 3. Check ancestors
        Node curr = node.parent;

        while (curr != null) {

            if (curr.locked) {
                return false;
            }

            curr = curr.parent;
        }

        // Lock the node
        node.locked = true;
        node.lockedBy = uid;

        // Add this node to every ancestor's lockedDescendants
        curr = node.parent;

        while (curr != null) {
            curr.lockedDescendants.add(node);
            curr = curr.parent;
        }

        return true;
    }

    // -------------------------------------------------------
    // UNLOCK
    // -------------------------------------------------------
    public boolean unlock(String name, int uid) {

        Node node = nodes.get(name);

        // Node is not locked
        if (!node.locked) {
            return false;
        }

        // Different user cannot unlock
        if (node.lockedBy != uid) {
            return false;
        }

        // Unlock
        node.locked = false;
        node.lockedBy = -1;

        // Remove from ancestors
        Node curr = node.parent;

        while (curr != null) {
            curr.lockedDescendants.remove(node);
            curr = curr.parent;
        }

        return true;
    }

    // -------------------------------------------------------
    // UPGRADE LOCK
    // -------------------------------------------------------
    public boolean upgradeLock(String name, int uid) {

        Node node = nodes.get(name);

        // 1. Node itself must not already be locked
        if (node.locked) {
            return false;
        }

        // 2. Must have at least one locked descendant
        if (node.lockedDescendants.isEmpty()) {
            return false;
        }

        // 3. Check ancestors
        Node curr = node.parent;

        while (curr != null) {

            if (curr.locked) {
                return false;
            }

            curr = curr.parent;
        }

        // 4. Every locked descendant must belong to same user
        for (Node lockedNode : node.lockedDescendants) {

            if (lockedNode.lockedBy != uid) {
                return false;
            }
        }

        // Copy because we will modify the set
        List<Node> descendants =
                new ArrayList<>(node.lockedDescendants);

        // 5. Unlock all locked descendants
        for (Node lockedNode : descendants) {
            unlock(lockedNode.name, uid);
        }

        // 6. Lock current node
        return lock(name, uid);
    }

    // -------------------------------------------------------
    // BUILD TREE
    // -------------------------------------------------------
    public void buildTree(String[] names, int m) {

        // Create all nodes
        for (String name : names) {
            nodes.put(name, new Node(name));
        }

        /*
         * Since input gives nodes in level order,
         * construct the M-ary tree using a queue.
         */
        Queue<Node> queue = new LinkedList<>();

        Node root = nodes.get(names[0]);
        queue.offer(root);

        int index = 1;

        while (index < names.length) {

            Node parent = queue.poll();

            for (int i = 0; i < m && index < names.length; i++) {

                Node child = nodes.get(names[index++]);

                child.parent = parent;
                parent.children.add(child);

                queue.offer(child);
            }
        }
    }

    // -------------------------------------------------------
    // MAIN
    // -------------------------------------------------------
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int m = sc.nextInt();
        int Q = sc.nextInt();

        String[] names = new String[N];

        for (int i = 0; i < N; i++) {
            names[i] = sc.next();
        }

        LockingTree tree = new LockingTree();

        tree.buildTree(names, m);

        for (int i = 0; i < Q; i++) {

            int operation = sc.nextInt();
            String nodeName = sc.next();
            int uid = sc.nextInt();

            boolean result;

            if (operation == 1) {
                result = tree.lock(nodeName, uid);
            }
            else if (operation == 2) {
                result = tree.unlock(nodeName, uid);
            }
            else {
                result = tree.upgradeLock(nodeName, uid);
            }

            System.out.println(result);
        }

        sc.close();
    }
}
```

---

# 🚀 Quick Revision

Before an interview, remember these five things:

```text
1. parent
   ↓
   Check ancestors

2. lockedDescendants
   ↓
   Check descendants efficiently

3. HashMap
   ↓
   O(1) average node lookup

4. HashSet
   ↓
   Maintain locked descendants

5. Upgrade
   ↓
   Unlock descendants
   ↓
   Lock current node
```

### Complexity

```text
lock        → O(log_m N)
unlock      → O(log_m N)
upgradeLock → O(K log_m N)
```

### Core Rule

```text
NO locked ancestor
+
NO locked descendant
+
Node itself unlocked
=
LOCK SUCCESS
```

This implementation directly follows the locking, unlocking, upgrade, balanced-tree, and complexity requirements described in the provided problem.
