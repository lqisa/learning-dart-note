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
class Base {
  Base() {
    print('superclass constructor Base is invoked');  // called first
  }
}

class Person extends Base {
  Person() {
    print('superclass constructor Person is invoked'); // called after Base constructor
  }
}

class Queue extends Person {}

void main() {
  Queue();
}

```

Need explictly call Super named constructor beacause no default constructor
```
class Base {
  Base() {
    print('superclass constructor Base is invoked');
  }
}

class Person extends Base {
  Person.constor();
}

class Queue extends Person {
  Queue(); // ERROR: The class 'Person' doesn't have an unnamed constructor. Try defining an unnamed constructor in 'Person', or invoking a different 

  // manually call one of the constructors in the superclass
  Queue() : super.constor(); // OK
  // constructor body
  Queue() : super.constor() { // OK
    print('call super named constructor and then self constructor')
  }
}

void main() {
  Queue();
}
```

#### explicitly call constructor
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

### Invoking a non-default superclass constructor
Execution order:
1. initializer list
2. superclass's no-arg constructor
3. main class's no-arg constructor

```dart
class Base {
  Base() {
    print('superclass constructor Base is invoked'); // invoke order: 2
  }
}

class Person extends Base {
  Person() {
    print(
        'superclass constructor Person is invoked'); // invoke order: 3
  }
}

String getName () {
  print('initializer list'); // invoke order: 1
  return 'Queue-name';
}

class Queue extends Person {
  String name;
  Queue() : name = getName() {
    print('constructor Queue is invoked'); // invoke order: 4
  }
}

void main() {
  Queue();
}
```


### Expression evaluated before constructor
> Because the arguments to the superclass constructor are evaluated before invoking the constructor, an argument can be an expression such as a function call

```dart
class Employee extends Person {
  Employee() : super.fromJson(fetchDefaultData());
  // ···
}
```


### Initializer list

Besides invoking a superclass constructor, you can also initialize instance variables **before** the constructor body runs.

### Constant constructors
If a constant constructor is outside of a constant context and is invoked without const, it creates a non-constant object:

```dart
class ImmutablePoint {
  static const ImmutablePoint origin = ImmutablePoint(0, 0);

  final double x, y;

  const ImmutablePoint(this.x, this.y);
}

var a = const ImmutablePoint(1, 1); // Creates a constant
var b = ImmutablePoint(1, 1); // Does NOT create a constant

assert(!identical(a, b)); // NOT the same instance!
```
