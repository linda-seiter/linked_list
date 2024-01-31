---
theme: default
paginate: true
marp: true
---

# Linked Lists

Linda Seiter

---

## Why are linked lists challenging to learn?

- Composition (LinkedList + Node) **and** Self-Referential Class (Node)
- Object traversal, null reference, circular reference
- Reference reassignment
- Abstract software object, not a "real world" object

_Have students had sufficient practice with these concepts?_

---

## Composition Warmup Lesson

_Where to store the relationship?_

One-to-many:

- Book -< Review
- Order -< Item

Many-to-many:

- Publisher -< Book -< Review
- Customer -< Order -< Item

Object Traversal (code/debug activities):

- Total cost of items in order 123.
- All book reviews for publisher Z?
- Which customers purchased item J?

---

## Self-Referential Warmup Lesson

Self-Referential Class:

- A person has a best friend
- A person has an oldest sibling
- A course has prerequisite(s)

Object Traversal, Reference Reassignment (code/debug activities):

- Best friend’s best friend? Is X a prereq of Y? Oldest sibling’s best friend?
- Circular references (ok with friends, not ok with prereqs)
- Oldest sibling has new best friend, Dump best friend

---

## Real World Composition + Self-Referential Warmup

Train engine pulling railway cars

```java
public class RailwayCar {
    private String id;
    private RailwayCar next;

    ...
}

public class Engine {

    private RailwayCar head;

    public void addToFront(RailwayCar car) {
        ...
    }

    public void addToEnd(RailwayCar car) {
        ...
    }

    public void detach(String id) {
        ...
    }
}
```

## ![bg right 95%](img/train1.png)

---

## Debugging helps students understand complex object concepts

- Map static algorithms/code to dynamic object state and control flow
- Learn how to find and fix errors
  - Visual debugger
  - Builtin IDE debugger

[pythontutor.com visualization](https://pythontutor.com/render.html#code=%20%0Aclass%20RailwayCar%20%7B%0A%20%20%20%20private%20String%20id%3B%0A%20%20%20%20private%20RailwayCar%20next%3B%0A%0A%20%20%20%20public%20RailwayCar%28String%20id%29%20%7B%20this.id%20%3D%20id%3B%20%7D%0A%20%20%20%20public%20String%20getId%28%29%20%7B%20return%20this.id%3B%20%7D%0A%20%20%20%20public%20RailwayCar%20getNext%28%29%20%7B%20return%20next%3B%20%7D%0A%20%20%20%20public%20void%20setNext%28RailwayCar%20next%29%20%7B%20this.next%20%3D%20next%3B%20%7D%0A%7D%0A%0Aclass%20Engine%20%7B%0A%0A%20%20%20%20private%20RailwayCar%20head%3B%0A%0A%20%20%20%20public%20void%20addToFront%28RailwayCar%20car%29%20%7B%0A%20%20%20%20%20%20%20%20car.setNext%28head%29%3B%0A%20%20%20%20%20%20%20%20head%20%3D%20car%3B%0A%20%20%20%20%7D%0A%0A%20%20%20%20public%20void%20addToEnd%28RailwayCar%20car%29%20%7B%0A%20%20%20%20%20%20%20%20if%20%28head%20%3D%3D%20null%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20head%20%3D%20car%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20else%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20//start%20at%20the%20head%20car%0A%20%20%20%20%20%20%20%20%20%20%20%20RailwayCar%20lastCar%20%3D%20head%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20//follow%20next%20reference%20to%20get%20to%20the%20last%20car%0A%20%20%20%20%20%20%20%20%20%20%20%20while%20%28lastCar.getNext%28%29%20!%3D%20null%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20lastCar%20%3D%20lastCar.getNext%28%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20//add%20car%20after%20the%20last%20car%0A%20%20%20%20%20%20%20%20%20%20%20%20lastCar.setNext%28car%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%0A%20%20%20%20public%20void%20detach%28String%20id%29%20%7B%0A%20%20%20%20%20%20%20%20if%20%28head%20!%3D%20null%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20if%20%28head.getId%28%29.equals%28id%29%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20//special%20case%20when%20deleting%20head%20car%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20head%20%3D%20null%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20else%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20RailwayCar%20currentCar%20%3D%20head%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20RailwayCar%20previousCar%20%3D%20null%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20//Loop%20to%20find%20car%20matching%20id,%20keeping%20track%20of%20previous%20car%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20while%20%28currentCar%20!%3D%20null%20%26%26%20!currentCar.getId%28%29.equals%28id%29%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20previousCar%20%3D%20currentCar%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20currentCar%20%3D%20currentCar.getNext%28%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20if%20%28currentCar%20!%3D%20null%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20//found%20car%20matching%20id,%20detach%20it%20from%20previous%20car%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20previousCar.setNext%28null%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%0A%7D%0A%0Apublic%20class%20TrainMain1%20%7B%0A%0A%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%0A%20%20%20%20%20%20%20%20Engine%20blueEngine%20%3D%20new%20Engine%28%29%3B%0A%20%20%20%20%20%20%20%20Engine%20redEngine%20%3D%20new%20Engine%28%29%3B%0A%20%20%20%20%20%20%20%20Engine%20yellowEngine%20%3D%20new%20Engine%28%29%3B%0A%0A%20%20%20%20%20%20%20%20blueEngine.addToFront%28%20new%20RailwayCar%28%22a1%22%29%20%29%3B%20//a1%0A%20%20%20%20%20%20%20%20blueEngine.addToFront%28%20new%20RailwayCar%28%22b1%22%29%20%29%3B%20//b1,a1%0A%20%20%20%20%20%20%20%20blueEngine.addToFront%28%20new%20RailwayCar%28%22c1%22%29%20%29%3B%20//c1,b1,a1%0A%0A%20%20%20%20%20%20%20%20redEngine.addToEnd%28%20new%20RailwayCar%28%22a2%22%29%20%29%3B%20//a2%0A%20%20%20%20%20%20%20%20redEngine.addToEnd%28%20new%20RailwayCar%28%22b2%22%29%20%29%3B%20//a2,b2%0A%20%20%20%20%20%20%20%20redEngine.addToEnd%28%20new%20RailwayCar%28%22c2%22%29%20%29%3B%20//a2,b2,c2%0A%0A%20%20%20%20%20%20%20%20blueEngine.detach%28%22b1%22%29%3B%20%20//c1,b1,a1%20%3D%3D%3E%20c1%0A%20%20%20%20%20%20%20%20blueEngine.detach%28%22zz%22%29%3B%20%20//c1%20%3D%3D%3E%20c1%0A%20%20%20%20%20%20%20%20redEngine.detach%28%22a2%22%29%3B%20%20%20//a2,b2,c2%20%3D%3D%3E%20no%20cars%0A%20%20%20%20%20%20%20%20yellowEngine.detach%28%22a1%22%29%3B%20%20//no%20cars%0A%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=71&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false)

---

## Warmup Assessment

- Worksheet: Object state <-> Code
- Coding Task: Update RailwayCar to store passenger class. Update Engine
  addTo..() methods to maintain class order:1st class cars, 2nd class cars, etc.
- Coding Task: Add a splice method that takes 2 car ids and removes that section
  of the train
- Coding Task: Update RailwayCar with fields for passenger capacity and
  occupancy. Update Engine with new method to load n passengers, starting with
  the front car. Method returns number of passengers unable to board.

---

## Teaching Linked Lists

I've used Zybooks for some of my intro programming courses.

- Participation Activities (formative throughout lesson)
- Challenge Activities (summative per lesson)
- Exercises & Labs (summative per chapter)

![bg right 90%](img/zybooks1.png)

---

## How I would tweak the lessons:

- **Non-Generics Singly Linked List (int or String)**
- Generics SLL
- Circular SLL
- **Circular SLL without explicit size field**
- **DLL without sentinels**
- DLL with sentinels
- Cloning, shallow vs deep copying

Use debugger to step through various implementations.

---

## Assignment with 3-4 tasks

Some possibilities:

- Find the middle node of DLL
- Merge two linked lists
- Swap two nodes x and y (and not just their contents) in SLL given their
  references. Repeat for a DLL. Analyze the efficiency of each. Which algorithm
  takes more time?
- Given a circularly linked list containing an even number of nodes, implement a
  method two split into two circularly linked lists of half the size.
- Implement the DLL with a single sentinel that guards both ends.
- Implement a circular version of a doubly linked list, without any sentinels,
  that supports all the public behaviors of the original as well as two new
  update methods, rotate() and rotateBackward().
