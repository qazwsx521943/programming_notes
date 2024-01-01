
## Definition

- class, struct

> Model custom types that encapsulate data.

- enum

> Model custom types that define a list of possible values.

## Value Type , Reference Type

差異在於它們在記憶體中的儲存和管理方式

### value type:

- 通常較小，且輕量，因為儲存的是實際數據的`值`，可以直接存在`Stack`上
- pass by value

![value type](/assets/images/programming.language.swift.Types.value-type.png)

### reference type:

- 可能較大，因為儲存的是對實際數據的`引用`，可能會包含其他資訊(類型訊息、ARC相關訊息)
- pass by reference

![reference type](/assets/images/programming.language.swift.Types.reference-type.png)
