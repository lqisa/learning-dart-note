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
  
