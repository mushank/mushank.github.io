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

## Properties

### Property Observers

- **You can observe changes to any property with `willSet` and `didSet`**

```
var someStoredProperty: Int = 42 {
	willSet {newValue is the new Value}
	didSet {oldValue is the old value}
}

override var inheritedProperty {
	willSet {newValue is the new value}
	didSet {oldValue is the old value}
}
```
- **One very common thing to do in an observer in a Controller is to update the user-interface**

### Lazy Initialization
1. A lazy property does not get initialized until someone accesses it**  
2. You can allocate an object, execute a closure, or call a method if you want

```
lazy var brain = CalculatorBrain()	// nice if CalclatorBrain used lots of esources

lazy var someProperty: Type = {
	// construct the value of someProperty here
	return <the constructed value>
}()

lazy var myProperty = self.initializeMyProperty()
```
- **This still satisfies the "you must initialize all of your properties" rule**
- **Unfortunately, things initialized this way can't be constants (i.e., `var` ok, `let` not okay)**  
*Only vars can be lazily initialized*  
*If you had constants in your class, those would have to be initializing in your initializer*
- **This can be used to get around some initialization dependency conundrums**

## Initialization

- **When is an init method needed?**  
	- init methods are not so common because properties can have their defaults set using `=`
	- Or properties might be Optionals, in which case they start out `nil`
	- You can also initialize a property by executing a closeure
	-  Or use lazy instantiation
	- So you only need init when a value can't be set in any of these ways
	
- **You also get some "free" init methods**
	- If all properties in a base class (no superclass) have defaults, you get `init()` for free
	- If a `struct` has no initializers, it will get a default one with all properties as arguments
	
	```
	struct MyStruct {
		var x: Int = 42
		var y: String = "moltuae"
		
		init(x: Int, y: String)	// comes for free
	}
	```
	
- **What can you do inside an init?**
	- You can set any prperty's value, even those with default values
	- Constant prperties (i.e. properties declared with `let`)can be set
	- You can call other init methods in your own class using `self.init(<args>)`
	- In a class, you can of course also call `super.init(<args>)`
	- But there are some rules for calling inits from inits in a `class` ...

- **What are you required to do inside init?**
	- By the time any init is done, all properties must have values (optionals can have the value nil)
	- There are two types of inits in a `class`, `convenience` and `designated` (i.e. not convenience)

	- ***Rules for designated init***
		1. `A designated init must (and can only) call a designated init that is in its immediate superclass`
		2. `You must initialize all properties introduced by your class before calling a superclass's init`
		3. `You must call a superclass's init before you assign a value to an inherited property`
		
	- ***Rules for convenience init***
		1. `A convenience init must (and can only) call a designated init in its own class`
		2. `A convenience init may call a designated init indirectly (through another convenience init)`
		3. `A convenience init must call a designated init before it can set any property values`
	
		**The calling of other inits must be complete before you can access properties or invoke methods**










