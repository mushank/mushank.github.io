#Swift Learning Notes

## Option
- **An Optional is just an enum**  
Conceptually it is like this (the <T> is a generic like as in Array<T>)...

```
enum Optional<T> {
	case None
	case Some(T)
}
let x:String? = nil
... is ...
let x = Optional<String>.None

let x:String? = "Hello"
... is ...
let x = Optional<String>.Some("Hello")

var y = x!
... is ...
switch x{
	case Some(let value): y = value
	case None:	// raise an exception
}

```
## Array
```
var a = Array<String>()
... is the same as ...
var a = [String]()

let animals = ["Giraffe", "Cow", "Doggie", "Bird"]
animals.append("Ostrich")	// won't compile, animals is immutable (because of let)
let animal = animals[5]	// crash (array index out of bounds)

// enumerating an Array
for animal in animals{
	print("\(animal)")
}
```

## Dictionary
```
var pac10teamRankings = Dictionary<String, Int>
... is the same as ...
var pac10teamRankings = [String:Int]()

pac10teamRankings = ["Stanford":1, "Cal":10]
let ranking = pac10teamRankings["Ohio State"]	// ranking is an Int? (would be nil)

// use a tuple with for-in to enumerate a Dictionary
for (key, value) in pac10teamRankings{
	print("\(key) = \(value)")
}
```

## Range
- **A Range in Swift is just two end points of a sensible type**  
Range is generic (e.g. Range<T>)  
This is sort of a pseudo-representation of Range:  

```
struct Range<T>{
	var startIndex:T
	var endIndex:T
}
```
An Array's range would be a Range<Int> (since Arrays are indexed by Int)  
Warning: A String subrange is not Range<Int> (it is Range<String.Index>)  

There is special syntax for specifying a Range: either ...(inclusive) or ..< (open-ended)  

```
let array = ["a", "b", "c", "d"]
let subArray1 = array[2...3]	// subArray1 will be ["c", "d"]
let subArray2 = array[2..<3]	// subArray2 will be ["c"]
for i in [27...104]{ }	// Range is enumeratable, like Array, String, Dictionary
```

## NSObject
- Base class for all Objective-C classes  
- Some advanced features will require you to subclass from NSObject (and it can't hurt to do so)  
***The best practice here is to always have your Swift classes inherit from NSObject.***  

## NSNumber
- Generic number-holding class

```
let n = NSNumber(35.5)
let intversion = n.intValue	// also doubleValue, boolValue, etc.
```

## NSDate
- Used to find out the date and time right now or to store past or future dates.  
- See alse NSCalendar, NSDateFormatter, NSDateComponents  
- If you are displaying a date in your UI, there are localization ramifications, so check these out!  

## NSData
- A "bag of bits".  Used to save/restore/transmit raw data throughout the iOS SDK  

## Data Structures in Swift
- **Classes, Structures and Enumerations**  
These are the 3 fundamental building blocks of data structures in Swift
- **Similarities**  
1.Declaration syntax ...

```
class CalculatorBrain{
	// class
}

struct Vertex{
	// struct
}

enum Op{
	// enum
}
```

2.Properties and Functions ...

```
func doit(argument:Type) -> ReturnValue{

}

var storedProperty = <initial value> (not enum)

var computedProperty:Type{
	get {}
	set {}
}
```
3.Initializers (again, not enum) ...

```
init(argument1:Type, argument2:Type, ...){
	
}
```


- **Differences**  
1.Inheritance (class only)	// 继承  
2.Introspection and casting (class only)	// 内省 和 转型  
3.Value type (struct, enum) vs. Reference type (class)	// 值类型(拷贝变量) 与 引用类型（传递指针）  
***堆内存中的对象，系统会自动为我们管理内存（ARC）***

















