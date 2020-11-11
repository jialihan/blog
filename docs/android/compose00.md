# Android - Compose Basics

## I. basic concept

### 1. android api and os 
1. latest android api level can be also used in an old android os device: like an old version of android phone
2. when you build your app, set the **android api level/version.**
* minVersion: eg: 21
* targetVersion: eg: 30
because these 3rd party api libs are installed and bounded on your app.


### 2. Android runtime
* android not run on Oracle JVM
* android has its own runtime (ART)

### 3. Android Studio
- IDE
- User Interface Designer
- Android SDK
- Android NDK
- Profiler and other tools
- AVD - Android virtal devices (like simulator)
- ADB - Android debug bridge (real device)
- Gradle

## II. MVC vs. MVVM

### 1. MVC
- **Controller** -> event handlers, fetch data, update model, interact with bus logic
- **View** -> Modeldirect way, view knows the data
- **Model** - Data: eg: Person class

Question: **how's the view updated?**
it depends on different frameworks, for exmaple: the model is updated, then the view automatically get updated.

### 2. MVVM

// TODO

### 3. React-ish / Declarative View

* AKA Functional Reactive
* AKA The "React" Pattern

This is react-pattern, we never updates the view directly, we only updates the model (data).
```
View = Function(Model)
```

Who use this pattern:
- Jetpack Compose - Kotlin
- Flutter - Dart
- React - JavaScript
- SwiftUI - Swift

## III. kotlin overview
* Concise and powerful lambdas
* Type safe builders
* Null handling ( safer than java)
* Extension functions
* rather than trying to generate a new ecosystem, they are latching on to existing
	- java Interop
	- javaScript Interop
	- Two-way Interop
	- Migration: low-cost, low-risk

Syntax:
- kotlin: destructure
- 
## IV. build your first android project

1. create a "no activity" project
2. data class: 
- Add a toString() methdhave a tostring method, then you can print the data class object.
```
data class Box(var width: Int, var height:Int)
fun f1(){  
    var b1 = Box(10,10)  
    println("b1= ${b1}")  
}
```
3. for variables:
- val: immutable
- var: mutable
 
4. shorthand in  IDE:
- "CTRL + ENTER":  generate for class constructors, getters and setters, and so on.
- "CMD + D": copy correct line when you put your pointer on that line
- "shift +Enter": enter the class first line from the class declaration
- "main + TAB": generate the main class code template
- "CMD + OPTION + L": auto-format code
- TODO:: how to do the test shorthand????
- "CTRL + SHIFT + P": show the type of the object

### V. OOD
Card
- suit: int
- value: int
Hand
- name:String
- cards: List<Card>
- points:Int

Game
- deck
- playHand
- delarHand
- deal
- hit
- stay
- msg

#### 1. kotlin syntax
1. function as a property
val: only the getter
var: getter, setter
```
val suitName: String 
    get() {  
	    return "hello world!"
    }  
}
```

2. named parameters
init the class with value, but also the parameter name.
```
 Card(suit = 1,value = 1);
```
3. assertions
require function -
check function -
assert function -

4. init{} function


VI. android folders
* Library/android
* users/android
* AVD manager: your virtual testable devices
* SDK manager: your SDKs


VII. Create a UI
1. activity - like a main
2. lifecycle 

3. Component UI:
- Text
- Column
- Row
- Button
wrapper component:
- MaterialTheme()
- Surface()

4.  remember ui componenets and local variables do not lost
5. functional programming: 
- everything is immutable
- pure functions: output is only depended on the input
	```
	func (y)
	{
		return y*y;
	}
	```
- MutableState
 val state:MutableState<Game> = mutableStateOf(Game(shuffle = true))
 mutable state of ui components
 For simple example:
	 ```
	val(active, setActive) = remember { mutableStateOf(true) }

	// in onClick: toggle the active state
	onClick ={
		setActive(!active) 
	}
	 ```
- 

6. function extension - need to import