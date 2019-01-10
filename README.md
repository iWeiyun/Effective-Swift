# Effective-Swift



#### 优先使用Struct，而非Class

Good - Struct

```swift
struct MyStruct {
    var v: Int
}
let m = MyStruct(v: 5)
var n = m
n.v = 6
print(m.v)	// 结果为5
```

Bad - Class

```swift
class MyClass {
    var v: Int?
}
let a = MyClass()
a.v = 5
let b = a
b.v = 6
print(a.v)	// 结果为6
```

​	

#### 尽量写纯函数

Good

```swift
class MyClass {
    var v1: String?

    // 输入、输出比较明确
    func myFunc(v: String?) -> String {
        return "Hello" + (v ?? "")
    }
}
```

Bad

```swift
class MyClass {
    var v1: String?

    // 输出结果依赖于成员变量v1。如果有修改成员变量，在多线程情况下也会不安全
    func myFunc() -> String {
        return "Hello" + (v1 ?? "")
    }
}
```

​	

#### 过滤、转换等，优先使用filter、map等高阶函数

Good

```swift
let arr = [1, 2, 3, 4]
let total = arr.reduce(0, +)
```

Bad

```swift
let arr = [1, 2, 3, 4]
var total = 0
for i in arr {
    total += i
}
```

​	

#### 使用集合的lazy来提升执行效率

Good

```swift
let arr = [1, 2, 3, 4, 5, 6]
let r = arr.lazy.filter { $0 % 3 == 0 }.first    // 集合遍历到3就结束了
```

Bad

```swift
let arr = [1, 2, 3, 4, 5, 6]
let r = arr.filter { $0 % 3 == 0 }.first    // 集合会被完整的遍历
```

​	

#### 判断集合的类型时，要先判断集合是否为空

Good

```swift
let arr = [Int]()
if arr.count > 0, arr is [String] {
    print("is String")
}
```

Bad

```swift
let arr = [Int]()
if arr is [String] {
    print("is String")    // 判断会成功，输出: is String
}
```

​	



#### Swift与OC混编时，OC中属性、参数等nullable、nonnull修饰要正确的使用。

Good

```objc
// OC
@interface MyOCClass : NSObject
@property (strong, nullable) NSString *str;
@end
```

```swift
// Swift
class SwiftClass {
    func test() {
        let oc = MyOCClass()
        print(oc.str?.count)
    }
}
```

Bad

```objc
// OC
@interface MyOCClass : NSObject
@property (strong) NSString *str;
@end
```

```swift
// Swift
class SwiftClass {
    func test() {
        let oc = MyOCClass()
        print(oc.str.count) // str为nil时，编译可以通过，会强制解包导致crash
    }
}
```