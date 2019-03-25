---
description: >-
  How many programmers does it take to change a lightbulb? None, that's a
  hardware issue
---

# Day 02

What we covered today:

* JS Gotchas
* Data Structures
* Singly Linked Lists

Warmup

* Text Soup 3

## Singly Linked List <a id="singly-linked-list"></a>

Singly Linked Lists are a type of data structure. In a singly linked list, each node in the list stores the contents and a pointer or reference to the next node in the list. It does not store any pointer or reference to the previous node. ... The last node in a single linked list points to nothing.

```bash
mkdir singly_linked_list
cd singly_linked_list/
touch singly_linked_list.rb
```

```ruby
class SinglyLinkedList
​
end
​
require 'pry'
binding.pry
```

```ruby
# add a class into the SinglyLinkedList Class
​
class SinglyLinkedList
​
  class Node
​
  end
​
end
```

```ruby
class SinglyLinkedList
​
  class Node
​
  end
​
  attr_accessor :head
​
  def initialize
    @head = nil
  end
​
end
```

```ruby
class SinglyLinkedList
​
  class Node
​
  end
​
  attr_accessor :head
​
  def initialize
    @head = nil
  end
​
  def prepend(value)
    node = Node.new(value)
    node.next = @head
    @head = node
  end
​
end
```

```ruby
class Node
  attr_accessor :value, :next
​
  def initialize(value)
    @value = value
    @next = nil
  end
end
```

Now we can run the program to see how it works.

```bash
ruby singly_linked_list.rb
```

Our `binging.pry`will keep us in the program as well will have access to the class.

```bash
pry(main)> SinglyLinkedList=> SinglyLinkedList
```

We are now able to instantiate a new instance of the `SinglyLinkedList`.

```bash
pry(main)> bros = SinglyLinkedList.new
=> #<SinglyLinkedList:0x00007fc048a83148 @head=nil>
```

We can then call the `prepend` method we just created to add a new node.

```bash
pry(main)> bros.prepend 'Groucho'
=> #<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">
```

This now have set the value of the head node to 'Groucho'.

```bash
pry(main)> bros
=> #<SinglyLinkedList:0x00007fc048a83148
 @head=#<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">>
​
pry(main)> bros.head
=> #<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">
​
pry(main)> bros.head.value
=> "Groucho"
```

Now if we were to add another Marx brother we will see that the head will change.

```bash
pry(main)> bros.prepend 'Harpo'
=> #<SinglyLinkedList::Node:0x00007fc0490d9880
 @next=#<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">,
 @value="Harpo">
​
pry(main)> bros
=> #<SinglyLinkedList:0x00007fc048a83148
 @head=
  #<SinglyLinkedList::Node:0x00007fc0490d9880
   @next=#<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">,
   @value="Harpo">>
​
pry(main)> bros.head.value
=> "Harpo"
```

We can see what the next head value or the previous node was.

```bash
pry(main)> bros.head.next.value
=> "Groucho"
```

As expected if you add another you can chain through to find the original node.

```bash
pry(main)> bros.prepend 'Zeppo'
=> #<SinglyLinkedList::Node:0x00007fc04806c218
 @next=
  #<SinglyLinkedList::Node:0x00007fc0490d9880
   @next=#<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">,
   @value="Harpo">,
 @value="Zeppo">
​
pry(main)> bros.head.value
=> "Zeppo"
​
pry(main)> bros.head.next.value
=> "Harpo"
​
pry(main)> bros.head.next.next.value
=> "Groucho"
```

```bash
def initialize(value=nil)
  @head = Node.new(value) if value
end
```

Make a method that finds the last node.

```bash
def last
  node = @head
  while node && node.next
    node = node.next # Traverse by one to the next node in the list.
  end
  node
end

```

We can now set a new instance of the class as the next node of the last.

```bash
def append(value) # AKA .push
  last.next = Node.new(value)
end
```

Run the program again.

```bash
ruby singly_linked_list.rb
​
​
    36: end
    37:
    38:
    39:
    40: require 'pry'
 => 41: binding.pry
```

Create a new instance of the class.

```bash
pry(main)> bros = SinglyLinkedList.new 'Groucho'
=> #<SinglyLinkedList:0x00007ffd690683f8
 @head=#<SinglyLinkedList::Node:0x00007ffd690683d0 @next=nil, @value="Groucho">>
​
pry(main)> bros.append 'harpo'
=> #<SinglyLinkedList::Node:0x00007ffd68ad62f0 @next=nil, @value="harpo">
​
pry(main)> bros.append 'chico'
=> #<SinglyLinkedList::Node:0x00007ffd68a85940 @next=nil, @value="chico">
​
pry(main)> bros.head.value
=> "Groucho"
​
pry(main)> bros.head.next
=> #<SinglyLinkedList::Node:0x00007ffd68ad62f0
 @next=#<SinglyLinkedList::Node:0x00007ffd68a85940 @next=nil, @value="chico">,
 @value="harpo">
```

##  <a id="node-js"></a>

