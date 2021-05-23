---
title: LRU Cache implementation
author: Silesh Chandran
date: 2021-05-22 14:10:00 +0800
categories: [coding, problem]
tags: [coding, problem, interview]
toc: false
comments: true
---

LRU or least recently used cache is a very frequently used cache implementation. Here I'll list out its properties and then implement it in a very simple manner.

An LRU cache is a type of fixed size data structure that keeps the most recently used item at the front. When the cache is full and a new item needs to be added, the oldest item (least recently used) is removed and the new one is added to the front.

So if we assume that ordering is maintained in the order of access, insertion will happen in front and deletion will happen from the rear when the size of the cache exceeds max capacity. Insertion and deletion also happens frequently so using an array would add complexity. This suggests that a linked list queue should be used for maintaining order. 

Since it is a cache, it is also important to provide easy access (in o(1) preferably) so we will need to maintain a hashmap of elements currently in the cache for fast random access. Both of these can be combined by using a data structure like orderedDict (python) or linkedListMap (Java), but i’m keeping them separate for the sake of simplicity

## Algorithm:

### Initialize:
1. Create head and tail references for the linked list.
2. Initialize an empty hashmap.

### Get element from cache:
1. If element is in map, return element, otherwise return -1

### Add element to cache:
1. Create a node, add it to map and make it the head of the linked list
2. If size of cache exceeds max size, then remove the tail element and remove it from the map

## Python Code

```python
class Node:
  #each node in the queue. Key is the identifier and val is the data.
  def __init__(self, k, v):
      self.key = k
      self.val = v
      self.prev = None
      self.next = None

class lruCache:
  def __init(self, s):
    self.size = s
    self.cache = {}
    self.head = None
    self.tail = None

  def getFromCache(self, key):
    #-1 if not present in cache
    if key not in self.cache:
      return -1
    #get from cache and move to head of linkedlist
    node = self.cache(key)
    _remove()
    _addtofront(node)
  
  def addToCache(self,key, val):
    if key in self.cache:
      return -1
    #create node and add to cache
    node = Node(key, val)
    self.cache[key] = node
    _addtofront(node)
    if len(self.cache)>self.size:
      #tail element needs to be removed from cache
      del self.cache[self.tail.key]
      _removefromrear()

  
  def _remove(self, node):
    #removes node from the linkedlist
    #p->node->n
    if node == self.tail:
      _removefromrear()
    else:
      p = node.prev
      n = node.next
      p.next = n
      if n:
        n.prev = p

  #Utility methods:
  def _addtofront(self, node):
    #adds node to the linkedlist
    #node => head->n
    node.next = head
    head.prev = node
    self.head = node
    if not self.tail:
      self.tail = self.head
  
  def _removefromrear(self):
    self.tail = self.tail.prev
    self.tail.next = None
	
```

