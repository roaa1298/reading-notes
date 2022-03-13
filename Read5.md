# Linked List

- The Linked List: it's a sequence of nodes connected to each other, each node is a reference to the next node in the link.

  the most commonly used linked list is the singly linked list.

    - the linked list is considered as a data structure, each node links to the next node in the list.
    - in a singly linked list each node has only one reference points to the next node in the list.
    - in doubly linked list each node has two references, one for the next node and the other for the previous node.
    - next is property for node contains the reference to the next node.
    - head is a reference for the first node.  
### A Vocabulary/Definition List

| Vocabulary | Definition |
| --- | ----------- |
| Linked List | a sequence of nodes that are connected/linked to each other |
| Singly | linked list with one reference, point to the next node |
| Doubly | linked list with two referencesn next and previous node |
| Node | linked list element that contains the data for each link |
| Next | property contains the reference to the next node |
| Head | property contains the reference to the first node |
| Current | property contains the reference to the current node |
| static data structures | data structures that need a given size and amount of memory |
| dynamic data structures | data structures that need a given size and amount of memory |


- The Next property is exceptionally important because it will lead us where the next node is and allow us to extract the data appropriately. The best way to approach a traversal is through the use of a while() loop. This allows us to continually check that the Next node in the list is not null. If we accidentally end up trying to traverse on a node that is null, a NullReferenceException gets thrown and our program will crash.

### Big O

- Big O Notation is a method of evaluating an algorithm's performance. It's a means of expressing how long a function, operation, or algorithm takes to perform based on the number of elements we provide to it. An O(1) function runs in constant time, which means that no matter how huge our input is, our algorithm will always require the same amount of time and memory to run.

