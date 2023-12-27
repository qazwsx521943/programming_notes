
## Struct vs Class vs Actor

### struct

```swift
struct MyStruct {
  var title: String
}

var structObject = MyStruct(title: "myStruct")

structObject.title = "updated myStruct" // This one is a whole new struct object
```

### Class

### Actor

```swift
actor MyDataManager {
 static let instance = MyDataManager()

 private init() {}

 var data: [String] = []

 func getRandomData() -> String? {
  self.data.append(UUID().uuidString)
  print(Thread.current)
  return self.data.randomElement()
 }
}
```

> Classes that are thread safe.

## Class vs Struct ?

There is much less need to worry about memory leaks or multiple threads racing to access / modeify a single instance of a variable.

## Value vs Reference types

types | Struct | Class | Actor | Function
---------|:----------:|:---------:|:--------:|:------:|
 type | Value | Reference | Reference | Reference
 Stored in | Stack | Heap | Heap | Heap
 Speed | Faster | Slower | Slower
 Thread safe | V | X | V | X
 Inheritance | X | V | X | X

## Stack vs Heap

Value based data types are stored in stack

Reference based data types are stored in heap

### Stack

- Each thread has it's own stack!
- Variables allocated on the stack are stored directly to the memory, and access to this memory is very fast

### Heap

- Shared across threads!

![](/assets/images/project%20brainstorming.flavor%20flash%20journal.thread_struct_heap.png)

## Automatic Reference Counting (ARC)

value types such as structures and enumerations are just copying the data to data. Therefore it's not affected by ARC.

ARC is used to track and manage the app's memory usage. When class instances are no longer needed, ARC automatically frees up the memory used by that class.

這也是為什麼esacaping closure通常會需要標註`weak self`的原因，以免當某個class不需要被使用時，卻因為某個closure中有引用到strong self而無法正常釋放記憶體。

## Situations when choosing different data types

1. Structs:
   1. Data Models
   2. SwiftUI Views

2. Classes:
   1. ViewModels

3. Actors:
   1. Shared 'Manager' / 'Data Store'

References:
- https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/