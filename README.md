# Data Structures in Python
## Table of contents
- [Array Data Structures](#array-data-structures)
    - [list](#list)
    - [tuple](#tuple)
    - [array.array](#arrayarray)
    - [str](#str)
    - [bytes](#bytes)
- [Dictionaries, Maps, and Hash Tables](#dictionaries-maps-and-hash-tables)
    - [dict](#dict)
    - [collections.OrderedDict](#collectionsordereddict)
    -[collections.defaultdict](#collectionsdefaultdict)
    - [collections.ChainMap](#collectionschainmap)
- [Records, Structs, and Data Transfer Objects](#records-structs-and-data-transfer-objects)
    - [dict](#dict-1)
    - [tuple](#tuple-1)
    - [dataclasses.dataclass](#dataclassesdataclass)
    - [collections.namedtuple](#collectionsnamedtuple)
    - [typing.NamedTuple](#typingnamedtuple)
    - [struct.Struct](#structstruct)
- [Sets and Multisets](#sets-and-multisets)
    - [set](#set)
    - [frozenset](#frozenset)
    - [collections.Counter](#collectionscounter)
- [Stacks](#stacks)
    - [list](#list-1)
    - [collections.deque](#collectionsdeque)
    - [queue.LifoQueue](#queuelifoqueue)
- [Queues](#queues)
    - [list](#list-2)
    - [collections.deque](#collectionsdeque-1)
    - [queue.Queue](#queuequeue)
    - [multiprocessing.Queue](#multiprocessingqueue)
- [Priority Queues](#priorityqueues)
    - [list](#list-3)
    - [heapq](#heapq)
    - [queue.PriorityQueue](#queuepriorityqueue)


## Array Data Structures
- An array is a fundamental data structure available in most programming languages.
- Arrays consist of fixed-size data records that allow each element to be efficiently located based on its index
- Arrays store information in adjoining blocks of memory, they’re considered contiguous data structures 

### list
Mutable dynamic arrays
- A list allows elements to be added or removed, and the list will automatically adjust the backing store that holds these elements by allocating or releasing memory.
    ```
    >>> a = ["apple","banana","carrot"]
    >>> a[0]
    'apple'

    >>> a
    ['apple','banana','carrot']

    >>> # Lists are mutable:
    >>> a[1] = "ball"
    >>> a
    ['apple','ball','carrot']

    >>> del a[1]
    >>> a
    ['apple','carrot']

    >>> # Lists can hold arbitrary data types:
    >>> a.append(58)
    >>> a
    ['apple','carrot',58]
    ```
### tuple
- Immutable Containers
- Unlike lists, Python’s tuple objects are immutable. This means elements can’t be added or removed dynamically—all elements in a tuple must be defined at creation time.
    ```
    >>>a = ("apple","banana","carrot")
    >>> a[0]
    'apple'
    
    >>> a
    ('apple','banana','carrot')

    >>> # Tuples are immutable:
    >>> a[1] = "ball"
    
    TypeError: 'tuple' object does not support item assignment

    >>> del a[1]

    TypeError: 'tuple' object doesn't support item deletion

    >>> # Tuples can hold arbitrary data types:
    >>> # (Adding elements creates a copy of the tuple)
    >>> a + (58,)
    ('apple','banana','carrot', 58)
    ```

### array.array
- Basic Typed Arrays
- Arrays created with the `array.array` class are mutable and behave similarly to lists except for one important difference: they’re typed arrays constrained to a single data type.
- Because of this constraint, `array.array` objects with many elements are more space efficient than lists and tuples. The elements stored in them are tightly packed, and this can be useful if you need to store many elements of the same type.

    ```
    >>> import array
    >>> a = array.array("r", (1.0, 1.5, 2.0, 2.5))
    >>> a[1]
    1.5

    >>> a
    array('r', [1.0, 1.5, 2.0, 2.5])

    >>> # Arrays are mutable:
    >>> a[1] = 58.0
    >>> a
    array('r', [1.0, 58.0, 2.0, 2.5])

    >>> del a[1]
    >>> a
    array('r', [1.0, 2.0, 2.5])

    >>> a.append(4.0)
    >>> a
    array('f', [1.0, 2.0, 2.5, 4.0])

    >>> # Arrays are "typed":
    >>> a[1] = "hello"
    
    TypeError: must be real number, not str
    ```

### str
- Immutable Arrays of Unicode Characters
- A `str` is an immutable array of characters. 
- It’s also a recursive data structure—each character in a string is itself a `str` object of length 1.
- Because strings are immutable in Python, modifying a string requires creating a modified copy. 
    ```
    >>> a = "abcd"
    >>> a[1]
    'b'

    >>> a
    'abcd'

    >>> # Strings are immutable:
    >>> a[1] = "e"
    
    TypeError: 'str' object does not support item assignment

    >>> del a[1]
    
    TypeError: 'str' object doesn't support item deletion

    >>> # Strings can be unpacked into a list to
    >>> # get a mutable representation:
    >>> list("abcd")
    ['a', 'b', 'c', 'd']
    >>> "".join(list("abcd"))
    'abcd'

    >>> # Strings are recursive data structures:
    >>> type("abc")
    "<class 'str'>"
    >>> type("abc"[0])
    "<class 'str'>"
    ```
### bytes
- Immutable Arrays of Single Bytes
- `bytes` objects are immutable sequences of single bytes, or integers in the range 0 ≤ x ≤ 255. 
-   `bytes` objects are immutable, but unlike strings, there’s a dedicated mutable byte array data type called `bytearray`.
    ```
    >>> a = bytes((0, 1, 2, 3))
    >>> a[1]
    1

    >>> # Bytes literals have their own syntax:
    >>> a
    b'\x00\x01\x02\x03'
    >>> a = b"\x00\x01\x02\x03"

    >>> # Only valid `bytes` are allowed:
    >>> bytes((0, 300))
    
    ValueError: bytes must be in range(0, 256)

    >>> # Bytes are immutable:
    >>> a[1] = 58
   
    TypeError: 'bytes' object does not support item assignment

    >>> del a[1]
    
    TypeError: 'bytes' object doesn't support item deletion
    ```
### bytearray
- Mutable Arrays of Single Bytes
- The `bytearray` type is a mutable sequence of integers in the range 0 ≤ x ≤ 255. 
- The `bytearray` object is closely related to the `bytes` object, with the main difference being that a bytearray can be modified freely—you can overwrite elements, remove existing elements, or add new ones. The `bytearray` object will grow and shrink accordingly.
    ```
    >>> a = bytearray((0, 1, 2, 3))
    >>> a[1]
    1

    >>> # The bytearray repr:
    >>> a
    bytearray(b'\x00\x01\x02\x03')

    >>> # Bytearrays are mutable:
    >>> a[1] = 58
    >>> a
    bytearray(b'\x00\x17\x02\x03')

    >>> a[1]
    58

    >>> # Bytearrays can grow and shrink in size:
    >>> del a[1]
    >>> a
    bytearray(b'\x00\x02\x03')

    >>> a.append(4)
    >>> a
    bytearray(b'\x00\x02\x03*')

    >>> # Bytearrays can only hold `bytes`
    >>> # (integers in the range 0 <= x <= 255)
    >>> a[1] = "hello"
    
    TypeError: 'str' object cannot be interpreted as an integer

    >>> a[1] = 300
    
    ValueError: byte must be in range(0, 256)

    >>> # Bytearrays can be converted back into bytes objects:
    >>> # (This will copy the data)
    >>> bytes(arr)
    b'\x00\x02\x03*'
    ```
## Dictionaries, Maps, and Hash Tables
- In Python, dictionaries (or dicts for short) are a central data structure. `dicts` store an arbitrary number of objects, each identified by a unique dictionary key.
- Dictionaries are also often called **maps**, **hashmaps**, **lookup tables**, or **associative arrays**.

### dict
- Python features a robust dictionary implementation that’s built directly into the core language: the `dict` data type.
    ```
    >>> a = {
    ...     "aaru": 123,
    ...     "akshu": 124,
    ...     "arun": 125,
    ... }

    >>> s = {x: x * x for x in range(6)}

    >>> a["akshu"]
    124

    >>> s
    {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
    ```
- Python’s dictionaries are indexed by keys that can be of any hashable type. A **hashable** object has a hash value that never changes during its lifetime.

### collections.OrderedDict
- Python includes a specialized `dict` subclass that remembers the insertion order of keys added to it: `collections.OrderedDict`.
    ```
    >>> import collections
    >>> d = collections.OrderedDict(one=1, two=2, three=3)

    >>> d
    OrderedDict([('one', 1), ('two', 2), ('three', 3)])

    >>> d["four"] = 4
    >>> d
    OrderedDict([('one', 1), ('two', 2),
                ('three', 3), ('four', 4)])

    >>> d.keys()
    odict_keys(['one', 'two', 'three', 'four'])
    ```

### collections.defaultdict
- Return Default Values for Missing Keys
- The `defaultdict` class is another dictionary subclass that accepts a callable in its constructor whose return value will be used if a requested key cannot be found.

    ```
    >>> from collections import defaultdict
    >>> d = defaultdict(list)

    >>> # Accessing a missing key creates it and
    >>> # initializes it using the default factory,
    >>> # i.e. list() in this example:
    >>> d["dogs"].append("Rufus")
    >>> d["dogs"].append("Kathrin")
    >>> d["dogs"].append("Mr Sniffles")

    >>> d["dogs"]
    ['Rufus', 'Kathrin', 'Mr Sniffles']
    ```
### collections.ChainMap
- Search Multiple Dictionaries as a Single Mapping
- The collections.ChainMap data structure groups multiple dictionaries into a single mapping.

    ```
    >>> from collections import ChainMap
    >>> dict1 = {"one": 1, "two": 2}
    >>> dict2 = {"three": 3, "four": 4}
    >>> chain = ChainMap(dict1, dict2)

    >>> chain
    ChainMap({'one': 1, 'two': 2}, {'three': 3, 'four': 4})

    >>> # ChainMap searches each collection in the chain
    >>> # from left to right until it finds the key (or fails):
    >>> chain["three"]
    3
    >>> chain["one"]
    1
    ```
### types.MappingProxyType
-  A Wrapper for Making Read-Only Dictionaries
- `MappingProxyType` is a wrapper around a standard dictionary that provides a read-only view into the wrapped dictionary’s data.

    ```
    >>> from types import MappingProxyType
    >>> writable = {"one": 1, "two": 2}
    >>> read_only = MappingProxyType(writable)

    >>> # The proxy is read-only:
    >>> read_only["one"]
    1
    >>> read_only["one"] = 58
    
    TypeError: 'mappingproxy' object does not support item assignment

    >>> # Updates to the original are reflected in the proxy:
    >>> writable["one"] = 4
    >>> read_only
    mappingproxy({'one': 4, 'two': 2})
    ```
## Records, Structs, and Data Transfer Objects

Record data structures provide a fixed number of fields. Each field can have a name and may also have a different type.


### dict
- Simple Data Objects

- Python dictionaries store an arbitrary number of objects, each identified by a unique key. 
- Dictionaries are also often called **maps** or **associative arrays** and allow for efficient lookup, insertion, and deletion of any object associated with a given key.

    ```
    >>> car1 = {
    ...     "color": "red",
    ...     "mileage": 3812.4,
    ...     "automatic": True,
    ... }
    >>> car2 = {
    ...     "color": "blue",
    ...     "mileage": 40231,
    ...     "automatic": False,
    ... }

    >>> # Dicts have a nice repr:
    >>> car2
    {'color': 'blue', 'automatic': False, 'mileage': 40231}

    >>> # Get mileage:
    >>> car2["mileage"]
    40231

    >>> # Dicts are mutable:
    >>> car2["mileage"] = 12
    >>> car2["windshield"] = "broken"
    >>> car2
    {'windshield': 'broken', 'color': 'blue',
    'automatic': False, 'mileage': 12}

    >>> # No protection against wrong field names,
    >>> # or missing/extra fields:
    >>> car3 = {
    ...     "colr": "green",
    ...     "automatic": False,
    ...     "windshield": "broken",
    ... }
    ```
### tuple
- Immutable Groups of Objects
-  Tuples take up slightly less memory than lists in CPython, and they’re also faster to construct.
    ```
    >>> # Fields: color, mileage, automatic
    >>> car1 = ("red", 3812.4, True)
    >>> car2 = ("blue", 40231.0, False)

    >>> # Tuple instances have a nice repr:
    >>> car1
    ('red', 3812.4, True)
    >>> car2
    ('blue', 40231.0, False)

    >>> # Get mileage:
    >>> car2[1]
    40231.0

    >>> # Tuples are immutable:
    >>> car2[1] = 12
    
    TypeError: 'tuple' object does not support item assignment

    >>> # No protection against missing or extra fields
    >>> # or a wrong order:
    >>> car3 = (3431.5, "green", True, "silver")
    ```
### dataclasses.dataclass
- The syntax for defining instance variables is shorter, since you don’t need to implement the .__init__() method.
- Instances of your data class automatically get nice-looking string representation via an auto-generated .__repr__() method.
- Instance variables accept type annotations, making your data class self-documenting to a degree. Keep in mind that type annotations are just hints that are not enforced without a separate `type-checking` tool.

    ```
    >>> from dataclasses import dataclass
    >>> @dataclass
    ... class Car:
    ...     color: str
    ...     mileage: float
    ...     automatic: bool
    ...
    >>> car1 = Car("red", 3812.4, True)

    >>> # Instances have a nice repr:
    >>> car1
    Car(color='red', mileage=3812.4, automatic=True)

    >>> # Accessing fields:
    >>> car1.mileage
    3812.4

    >>> # Fields are mutable:
    >>> car1.mileage = 12
    >>> car1.windshield = "broken"

    >>> # Type annotations are not enforced without
    >>> # a separate type checking tool like mypy:
    >>> Car("red", "NOT_A_FLOAT", 99)
    Car(color='red', mileage='NOT_A_FLOAT', automatic=99)
    ```
### collections.namedtuple
- Convenient Data Objects
- The `namedtuple` class available in Python 2.6+ provides an extension of the built-in `tuple` data type.
- `namedtuple` objects are immutable, just like regular tuples.
    ```
    >>> from collections import namedtuple
    >>> from sys import getsizeof

    >>> p1 = namedtuple("Point", "x y z")(1, 2, 3)
    >>> p2 = (1, 2, 3)

    >>> getsizeof(p1)
    64
    >>> getsizeof(p2)
    64
    ```
### typing.NamedTuple
- Improved Namedtuples
- `typing.NamedTuple` is the younger sibling of the namedtuple class in the collections module.
- It’s very similar to namedtuple, with the main difference being an updated syntax for defining new record types and added support for type hints.

    ```
    >>> from typing import NamedTuple

    >>> class Car(NamedTuple):
    ...     color: str
    ...     mileage: float
    ...     automatic: bool

    >>> car1 = Car("red", 3812.4, True)

    >>> # Instances have a nice repr:
    >>> car1
    Car(color='red', mileage=3812.4, automatic=True)

    >>> # Accessing fields:
    >>> car1.mileage
    3812.4

    >>> # Fields are immutable:
    >>> car1.mileage = 12
    
    AttributeError: can't set attribute

    >>> car1.windshield = "broken"
   
    AttributeError: 'Car' object has no attribute 'windshield'

    >>> # Type annotations are not enforced without
    >>> # a separate type checking tool like mypy:
    >>> Car("red", "NOT_A_FLOAT", 99)
    Car(color='red', mileage='NOT_A_FLOAT', automatic=99)
    ```

### struct.Struct
- Serialized C Structs
- The `struct.Struct` class converts between Python values and C structs serialized into Python bytes objects.

    ```
    >>> from struct import Struct
    >>> MyStruct = Struct("i?f")
    >>> data = MyStruct.pack(58, False, 4.0)

    >>> # All you get is a blob of data:
    >>> data
    b'\x17\x00\x00\x00\x00\x00\x00\x00\x00\x00(B'

    >>> # Data blobs can be unpacked again:
    >>> MyStruct.unpack(data)
    (58, False, 4.0)
    ```
### types.SimpleNamespace
- Fancy Attribute Access
- This means SimpleNamespace instances expose all of their keys as class attributes. 
    ```
    >>> from types import SimpleNamespace
    >>> car1 = SimpleNamespace(color="red", mileage=3812.4, automatic=True)

    >>> # The default repr:
    >>> car1
    namespace(automatic=True, color='red', mileage=3812.4)

    >>> # Instances support attribute access and are mutable:
    >>> car1.mileage = 12
    >>> car1.windshield = "broken"
    >>> del car1.automatic
    >>> car1
    namespace(color='red', mileage=12, windshield='broken')
    ```

## Sets and Multisets
A set is an unordered collection of objects that doesn’t allow duplicate elements.
```
vowels = {"a", "e", "i", "o", "u"}
```
### set
- The `set` type is the built-in set implementation in Python. It’s mutable and allows for the dynamic insertion and deletion of elements.
    ```
    >>> vowels = {"a", "e", "i", "o", "u"}
    >>> "e" in vowels
    True

    >>> letters = set("alice")
    >>> letters.intersection(vowels)
    {'a', 'e', 'i'}

    >>> vowels.add("x")
    >>> vowels
    {'i', 'a', 'u', 'o', 'x', 'e'}

    >>> len(vowels)
    6
    ```
### frozenset
- Immutable Sets
- The `frozenset` class implements an immutable version of `set` that can’t be changed after it’s been constructed.
- `frozenset` objects are static and allow only query operations on their elements, not inserts or deletions. 
    ````
    >>> vowels = frozenset({"a", "e", "i", "o", "u"})
    >>> vowels.add("p")
    
    AttributeError: 'frozenset' object has no attribute 'add'

    >>> # Frozensets are hashable and can
    >>> # be used as dictionary keys:
    >>> d = { frozenset({1, 2, 3}): "apple" }
    >>> d[frozenset({1, 2, 3})]
    'apple'
    ```
### collections.Counter
- Multisets
- The `collections.Counter` class in the Python standard library implements a multiset, or bag, type that allows elements in the set to have more than one occurrence.

    ```
    >>> from collections import Counter
    >>> inventory = Counter()

    >>> loot = {"sword": 1, "bread": 3}
    >>> inventory.update(loot)
    >>> inventory
    Counter({'bread': 3, 'sword': 1})

    >>> more_loot = {"sword": 1, "apple": 1}
    >>> inventory.update(more_loot)
    >>> inventory
    Counter({'bread': 3, 'sword': 2, 'apple': 1})
    ```
- Calling `len()` returns the number of unique elements in the multiset, whereas the total number of elements can be retrieved using `sum()`

    ```
    >>> len(inventory)
    3  

    >>> sum(inventory.values())
    6  
    ```
## Stacks
- A stack is a collection of objects that supports fast Last-In/First-Out (LIFO) semantics for inserts and deletes. 
- The insert and delete operations are also often called **push** and **pop**

### list
- Simple, Built-in Stacks
- Python’s lists are implemented as dynamic arrays internally, which means they occasionally need to resize the storage space for elements stored in them when elements are added or removed.
    ```
    >>> s = []
    >>> s.append("eat")
    >>> s.append("sleep")
    >>> s.append("code")

    >>> s
    ['eat', 'sleep', 'code']

    >>> s.pop()
    'code'
    >>> s.pop()
    'sleep'
    >>> s.pop()
    'eat'

    >>> s.pop()
   
    IndexError: pop from empty list
    ```
### collections.deque
- The deque class implements a double-ended queue that supports adding and removing elements from either end.
- Python’s deque objects are implemented as doubly-linked lists.
    ```
    >>> from collections import deque
    >>> s = deque()
    >>> s.append("eat")
    >>> s.append("sleep")
    >>> s.append("code")

    >>> s
    deque(['eat', 'sleep', 'code'])

    >>> s.pop()
    'code'
    >>> s.pop()
    'sleep'
    >>> s.pop()
    'eat'

    >>> s.pop()
    
    IndexError: pop from an empty deque
    ```
### queue.LifoQueue
- Locking Semantics for Parallel Computing
- The `LifoQueue` stack implementation in the Python standard library is synchronized and provides locking semantics to support multiple concurrent producers and consumers.
    ```
    >>> from queue import LifoQueue
    >>> s = LifoQueue()
    >>> s.put("eat")
    >>> s.put("sleep")
    >>> s.put("code")

    >>> s
    <queue.LifoQueue object at 0x108298dd8>

    >>> s.get()
    'code'
    >>> s.get()
    'sleep'
    >>> s.get()
    'eat'

    >>> s.get_nowait()
    queue.Empty

    >>> s.get()  # Blocks/waits forever...
    ```
## Queues
- A queue is a collection of objects that supports fast First-In/First-Out (FIFO) semantics for inserts and deletes. 
-  The insert and delete operations are sometimes called enqueue and dequeue.

### list
- It’s possible to use a regular `list` as a queue, but this is not ideal from a performance perspective.
- Lists are quite slow for this purpose because inserting or deleting an element at the beginning requires shifting all the other elements by one.
    ```
    >>> q = []
    >>> q.append("eat")
    >>> q.append("sleep")
    >>> q.append("code")

    >>> q
    ['eat', 'sleep', 'code']

    >>> # Careful: This is slow!
    >>> q.pop(0)
    'eat'
    ```
### collections.deque
- The `deque` class implements a double-ended queue that supports adding and removing elements from either end.
    ```
    >>> from collections import deque
    >>> q = deque()
    >>> q.append("eat")
    >>> q.append("sleep")
    >>> q.append("code")

    >>> q
    deque(['eat', 'sleep', 'code'])

    >>> q.popleft()
    'eat'
    >>> q.popleft()
    'sleep'
    >>> q.popleft()
    'code'

    >>> q.popleft()
    
    IndexError: pop from an empty deque
    ```
### queue.Queue
- Locking Semantics for Parallel Computing
- The `queue.Queue` implementation in the Python standard library is synchronized and provides locking semantics to support multiple concurrent producers and consumers.
    ```
    >>> from queue import Queue
    >>> q = Queue()
    >>> q.put("eat")
    >>> q.put("sleep")
    >>> q.put("code")

    >>> q
    <queue.Queue object at 0x1070f5b38>

    >>> q.get()
    'eat'
    >>> q.get()
    'sleep'
    >>> q.get()
    'code'

    >>> q.get_nowait()
    queue.Empty

    >>> q.get()  # Blocks/waits forever...
    ```

### multiprocessing.Queue
- Shared Job Queues
- `multiprocessing.Queue` is a shared job queue implementation that allows queued items to be processed in parallel by multiple concurrent workers.
    ```
    >>> from multiprocessing import Queue
    >>> q = Queue()
    >>> q.put("eat")
    >>> q.put("sleep")
    >>> q.put("code")

    >>> q
    <multiprocessing.queues.Queue object at 0x1081c12b0>

    >>> q.get()
    'eat'
    >>> q.get()
    'sleep'
    >>> q.get()
    'code'

    >>> q.get()  # Blocks/waits forever...
    ```

## Priority Queues
- A priority queue is a container data structure that manages a set of records with totally-ordered keys to provide quick access to the record with the smallest or largest key in the set.

### list
- Manually Sorted Queues
- You can use a sorted list to quickly identify and delete the smallest or largest element.

    ```
    >>> q = []
    >>> q.append((2, "code"))
    >>> q.append((1, "eat"))
    >>> q.append((3, "sleep"))
    >>> # Remember to re-sort every time a new element is inserted,
    >>> # or use bisect.insort()
    >>> q.sort(reverse=True)

    >>> while q:
    ...     next_item = q.pop()
    ...     print(next_item)
    ...
    (1, 'eat')
    (2, 'code')
    (3, 'sleep')
    ```
### heapq
- List-Based Binary Heaps
- `heapq` is a binary heap implementation usually backed by a plain list, and it supports insertion and extraction of the smallest element.
    ```
    >>> import heapq
    >>> q = []
    >>> heapq.heappush(q, (2, "code"))
    >>> heapq.heappush(q, (1, "eat"))
    >>> heapq.heappush(q, (3, "sleep"))

    >>> while q:
    ...     next_item = heapq.heappop(q)
    ...     print(next_item)
    ...
    (1, 'eat')
    (2, 'code')
    (3, 'sleep')
    ```
### queue.PriorityQueue
- `queue.PriorityQueue` uses heapq internally and shares the same time and space complexities. 
- The difference is that `PriorityQueue` is synchronized and provides locking semantics to support multiple concurrent producers and consumers.
    ```
    >>> from queue import PriorityQueue
    >>> q = PriorityQueue()
    >>> q.put((2, "code"))
    >>> q.put((1, "eat"))
    >>> q.put((3, "sleep"))

    >>> while not q.empty():
    ...     next_item = q.get()
    ...     print(next_item)
    ...
    (1, 'eat')
    (2, 'code')
    (3, 'sleep')
    ```

