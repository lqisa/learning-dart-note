## Instance variables
- **all** instance variables implicit `getter` method
- non-final & `late final` without initializers generate an implicit `setter` method
- non-late can't access `this`, beacause it initialized:
  - **before** instance is created
  - **before** constructor
  - **before** initializer list execute

## Static Field
- Static variables aren't initialized until they're used.
```dart
// does not call until Queue.initialCapacity is called
String getValue() {
  print('getValue');
  return 'a';
}

class Queue {
  static var initialCapacity = getValue();
  // ···
}

void main() {
  var q = Queue();
  print('queue init'); // ====> print 'queue init'
}
```
add `Queue.initialCapacity`:
```dart
//... same as above

void main() {
  var q = Queue();
  print('queue init'); // ====> print 'queue init'
  print('get queue static field: ${Queue.initialCapacity}'); // ====> print 'getValue' && 'get queue static field: a'
}
```

## Constructors
Declare a constructor by creating **a function with the same name** as its class (plus, optionally, an additional identifier as described in Named constructors).

### Generative constructor (生成构造函数)
Generative constructor（生成构造函数）是指在面向对象编程中用于创建新对象的一种特殊类型的构造函数。
如果没有定义任何构造函数，则会**自动生成**一个默认的 generative constructor。如果定义了**一个或多个构造函数**，则**必须显式地调用**其中一个构造函数，否则默认的构造函数将不可用。

#### default constructor
> ⚠⚠⚠ The default constructor has **no arguments** and invokes the **no-argument constructor in the superclass**
```dart
class Person {
  Person() {
    print('superclass constructor is invoked'); // print here
  }
}

class Queue extends Person {}

void main() {
  Queue(); // OK
}

```

#### explictly call constructor
```dart
class Queue {
  Queue.constor();
}

void main() {
  var q1 = Queue(); => ERROR: The class 'Queue' doesn't have an unnamed constructor. Try using one of the named constructors defined in 'Queue'.
  var q2 = Queue.constor(); // => OK
}

```

### Initializing formal parameters
> Simplify the common pattern of assigning a constructor argument to an instance variable.

Use `this.propertyName` directly in the constructor declaration, and omit the body.
```dart
class Point {
  final double x;
  final double y;

  // Sets the x and y instance variables
  // **before** the constructor body runs.
  Point(this.x, this.y);
}
```

### Constructors aren't inherited
A superclass's **named constructor** is not inherited by a subclass  =>  you must implement in the subclass




