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

## Value vs. Reference
- **Value (struct and enum)**

	1. Copied when passed as an argument to a function
	2. Copied when assigned to a different variable
	3. Immutable if assigned to a variable with let
	4. Remember that function parameters are, by default, constants
	5. You can put the keyword `var` on an parameter, and it will be mutable, but it's still a copy
	6. You must note any func that can mutate a struct/enum with the keyword `mutating`

- **Reference (class)**
	1. Stored in the heap and reference counted (automatically)
	2. Constant pointers to a class(let) still can mutate by calling methods and changing properties
	3. When passed as an argument, does not make a copy (just passing a pointer to same instance)

- **Choosing which to use**
	1. Usually you will choose class over struct. struct tends to be more for fundamental types.
	2. Use of enum is situational (any time you have a type of data with discrete values)

## Method
- **Obviously you can override methods/properties in your superclass**

	1. Precede your func or var with the keywork `override`
	2. A method can be marked `final` which will prevent subclasses from being able to override
	3. Classes can alse be marked final

- **Both types and instances can have methods/properties**

	1. For this example, lets consider the struct Double (yes, Double is a struct)

```
var d:Double = ...
if d.isSignMinus{
	d = Double.abs(d)
}
```

*`isSignMiuns` is an instance property of a Double (you send it to a particular Double)*  
*`abs` is a type method of Double (you send it to the type itself, not to a particular Double)*  

- ***Declare a type method or a property by using `static` in front of it：***  

```
static func ads(d:Double) -> Double
```
- ***In a class you use the word `class` not `static`, called a class function***

### Parameters Names
- **All parameters to all functions have an `internal name` and `an external name`**
- **The `internal name` is the name of the local variable you use inside the method**
- **The `external name` is what callers will use to call the method**

``` 
// Both with external name and internal name
func foo(external internal: Int){
	let local = internal
}

func bar(){
	let result = foo(external:123)
}
```
- **You can put `_` if you don't want callers to use an external name at all for a given parameter	// That is the default for the first argument to a function**

```
// With under bar thing for enternal name
func foo(_ internal: Int){
	let local = internal
}

func bar(){
	let result = foo(123)
}
```
```
// Under bar thing is the default for the first argument
func foo(internal: Int){
	let local = internal
}

func bar(){
	let result = foo(123)
}
```

- **You can force the first parameter's external name to be the internal name with `#`**

```
// Force the first parameter's external name to be the internal name with `#`
func foo(#internal: Int){
	let local = internal
}

func bar(){
	let result = foo(internal: 123)
}
```

- **For other (not the first) parameters, the internal name is, by default, the external name

```
func foo(first: Int, second: Double){
	let local = internal
}

func bar(){
	let result = foo(123, second: 5.5)
}
```
- **Any parameter's external name can be changed**

```
func foo(first: Int, externalSecond second: Double){
	let local = internal
}

func bar(){
	let result = foo(123, externalSecond: 5.5)
}
```

- **Or even omitted (though this would be sort of "anti-Swift")**

```
func foo(first: Int, _ second: Double){
	let local = internal
}

func bar(){
	let result = foo(123, 5.5)
}
```





