#Swift Learning Notes

## Option
- **An Optional is just an enum**  
Conceptually it is like this(the <T> is a generic like as in Array<T>)...

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
	case Some()
	case None
}

```
