# Swift Style Guide

In general, code should match Apple's style in [The Swift Programming Language](https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html).

## The Basics

### Constants and Variables

Prefer `let` over `var`. The use of `let` signals that the value will not change, and makes the code safer, more clear, and generally less scary for other programmers.

When possible, rely on Swift's built-in type inference rather than explicity specifying the type.

✅
```swift
let computer = "MacBook Pro"
let count = 5
```
❌
```swift
let computer: String = "MacBook Pro"
let count: Int = 5
```

### Optionals

Prefer optional binding or optional chaining over forced unwrapping.

✅
```swift
if let photo = photo {
    crop(photo)
}
```
❌
```swift
if photo != nil {
    crop(photo!)
}
```

Avoid using implicitly unwrapped optionals.

```swift
let speed: Float  // ✅
let speed: Float! // ❌
```

## Spacing

- Indent using 4 spaces. This is the default setting in Xcode.
- For blank lines, keep the leading indentation. This is the default setting in Xcode.
- Colons should have no space on the left and one space on the right.

✅
```swift
class Ham: Sandwich {}
let pi: Float = 3.14
let diameters = ["Earth": 7918, "Moon": 2159]
```
❌
```swift
class Ham : Sandwich {}
let pi :Float = 3.14
let diameters = ["Earth":7918, "Moon":2159]
```

- Parentheses used in function declarations and function calls should not have any spaces on either side.

✅
```swift
func draw(usingColor: UIColor) {}
draw(usingColor: .purple)
```
❌
```swift
func draw (usingColor: UIColor ) {}
draw( usingColor: .purple )
```

- All operators (with the exception of range operators and unary operators) should have one space to the left, and one space to the right.

```swift
let x = 1 + 2 // ✅

let x = 1+2   // ❌
```

- Comments should have exactly one space after the "//".

```swift
// Comment ✅
//Comment  ❌
```

- Classes should begin with a blank line.
- Simple structures that only have stored properties do not need a blank line at the beginning. Structures with added functionality such as computed properties or functions, should begin with a blank line.
- Simple enumerations that only have a list of cases do not need a blank line at the beginning. Enumerations with added functionality such as computed properties or functions, should begin with a blank line.
- Longer functions should begin with one blank line. For shorter functions (roughly 1 to 3 lines long), the blank line is optional.
- Use blank lines within methods to separate functionality.
- There should be one blank line between function declarations.
- There should be one blank line at the end of each file.
- Long lines should not be wrapped. Xcode automatically wraps long lines, so there is no need to manually wrap them to some arbitrary length. This applies to comments, statements, function declarations, etc.

## Operators

### Ternary Conditional Operator

Use the ternary conditional operator (a ? b : c) instead of an if/else statement in situations where it enables a more consice, efficient syntax.

 ✅
```swift
let name = isFrench ? "Pierre" : "Rupert"
```
❌
```swift
if isFrench {
    name = "Pierre"
} else {
    name = "Rupert"
}
```

Do not combine multiple uses of the ternary conditional operator into a single compound statement. This results in code that is difficult to understand.

 ✅
```swift
let maleName = isFrench ? "Pierre" : "Rupert"
let femaleName = isFrench ? "Emma" : "Gillian"
let name = isMale ? maleName : femaleName
```
❌
```swift
let name = isMale ? (isFrench ? "Pierre" : "Rupert") : (isFrench ? "Emma" : "Gillian")
```

When the ternary conditional operator is used in conjunction with a comparison operator, always use parentheses to add to the clarity and readability of the code.

 ✅
```swift
let text = (count == 0 ? "None" : "Some")
```
❌
```swift
let text = count == 0 ? "None" : "Some"
```

### Nil-Coalescing Operator

Use the nil-coalescing operator (a ?? b) to encapsulate conditional checking and unwrapping.

✅
```swift
let greeting: String? = nil
label.text = greeting ?? "Hello"
```
❌
```swift
let greeting: String? = nil
label.text = (greeting != nil) ? greeting : "Hello"
```

### Explicit Parentheses

>It is sometimes useful to include parentheses when they are not strictly needed, to make the intention of a complex expression easier to read ... use parentheses where they help to make your intentions clear." – Apple

✅
```swift
if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
```

## Control Flow

### if/else braces

For the placement if/else statement braces, follow Apple's lead:

✅
```swift
if true {
    print("true")
} else {
    print("false")
}
```
❌
```swift
if true
{
    print("true")
}
else
{
    print("false")
}
```
❌
```swift
if true {
    print("true")
}
else {
    print("false")
}
```

### switch statements

Switch statements are generally preferred over if/else statements for handling the cases of an enumeration.

✅
```swift
switch breakfast {
case .egg:
    fry()
case .biscuit:
    bake()
}
```
❌
```swift
if breakfast == .egg {
    fry()
} else if breakfast == .biscuit {
    bake()
}
```

In situations where only a single case of the enum is handled, it is OK to use an if-statement.

🆗
```swift
switch breakfast {
case .egg: scramble()
default: break
}
```
🆗
```swift
if breakfast == .egg {
    scramble()
}
```

When each case handler within the switch statement has a single line of code, and the nature of the single line of code is consistent across all cases, prefer a more compact version of the switch statement where the case and the statement are on the same line. This keeps the code concise and improves readability. In addition, the statements can be manually aligned horizontally to further improve the readablity of the code.

✅
```swift
switch breakfast {
case .egg:     fry()
case .biscuit: bake()
case .fruit:   slice()
}
```
🆗
```swift
switch breakfast {
case .egg:     
    fry()
case .biscuit:
    bake()
case .fruit:   
    slice()
}
```

### Early Exit

>Using a guard statement for requirements improves the readability of your code, compared to doing the same check with an if statement. It lets you write the code that’s typically executed without wrapping it in an else block, and it lets you keep the code that handles a violated requirement next to the requirement. – Apple

✅
```swift
func divideOneHundredBy(input: Int) -> Int? {
    guard input != 0 else {
        return nil
    }
    return 100 / input
}
```
❌
```swift
func divideOneHundredBy(input: Int) -> Int? {
    if input != 0 {
        return 100 / input
    } else {
        return nil
    }
}
```

## Classes and Structures

In general, don't use a class in cases where a structure could be used instead. Be sure to consider value semantics versus reference semantics when making this decision.

### Using `self`

Avoid the use of `self`. If the code will compile without the use of `self`, then don't use it.

### Read-Only Computed Properties

Do not use the `get` keyword with read-only computed properties.

✅
```swift
var fullName: String {
    return "\(first) \(last)"
}
```
❌
```swift
var fullName: String {
    get {
        return "\(first) \(last)"
    }
}
```

## Code Organization

Within a class, code should be ordered, labeled, and grouped in a logical and consistent manner from top-to-bottom. Use these guiding principles:

- Properties should be positioned near the top, followed by initialization/deinitialization code, followed by function declarations.
- Public properties and fuctions should be positioned above private ones.

Based on these principles, the order of code from top-to-bottom will be roughly as follows:

- Public properties
- Property overrides
- IBOutlets
- Private properties
- Init / Deinit
- Public fuctions
- Fuction overrides
- IBActions
- Private functions

Developer/Blogger Jared Sinclair outlined this organization philosophy in the following blog post:

[How I Organize a Swift File](http://blog.jaredsinclair.com/post/152672541355/how-i-organize-a-swift-file)

When it makes sense, group extra functionality into extensions at the bottom of the file.

Protocol conformance should typically be implemented in an extension at the bottom of the file.

Major groups of code within a class or structure should be organized using `// MARK: -`. This groups code within the Xcode jump bar, which is very helpful when navigating code. It also makes it easier to visually navigate the codebase. As shown below, visual emphasis can be added by including a line of `===` above and below the `MARK` line.

```swift
//===============================================
// MARK: - <#Title#>
//===============================================
```

## Access Control

>Access control restricts access to parts of your code from code in other source files and modules. This feature enables you to hide the implementation details of your code, and to specify a preferred interface through which that code can be accessed and used.
>
>You can assign specific access levels to individual types (classes, structures, and enumerations), as well as to properties, methods, initializers, and subscripts belonging to those types. Protocols can be restricted to a certain context, as can global constants, variables, and functions. – Apple

In general, use the most restrictive access level possible. This makes it more clear which properties and functions are intended to be exposed publicly, and which are intended to remain private.

Do not specify an explicit access level if the default access level already provides the desired access level.

## Documentation

Use documentation sparingly. Although "Documentation" sounds like a good thing to do, it often just clutters up the code with chunks of text that will never be read.

Code should be self-documenting as much as possible. This means spending time to think of clear and concise variable names, function names, and function argument labels. A function that is difficult to name may be a sign that the function is doing too much; consider breaking it up into multiple functions if it makes sense in context.

When making decisions about documentation, think about other programmers, or your future self. Ask "how difficult will it be for others to understand the intent and/or effects of this code?". Add documentation where it would improve the other programmer's ability to quickly understand the code. Documentation should explain *why* the code does something, rather than explaining *what* the code does – To figure out what the code does, programmers can simply read the code itself.

Weird bug fixes and iOS-workarounds should often be documented. If there is a Stack Overflow article explaining the weird bug fix, consider including a link to the article above the applicable line of code. You don't want your future self asking "why in the world did I do this?".

[Wikipedia: Self-documenting code](https://en.wikipedia.org/wiki/Self-documenting_code)

## Naming

- Use "tapped" rather than "pressed" or "clicked" when referring to the user tapping something on the screen, such as a button.
- Follow the Swift API Design Guidelines for naming. [Swift.org - API Design Guidelines](https://swift.org/documentation/api-design-guidelines)
- To be continued ...

## General Cocoa Touch Guidelines

The guidelines below are not technically related to Swift style. Perhaps we will break this out into a separate document later.

- When possible, use `UITableViewController` instead of creating a `UIViewController` with a `UITableView` added to it. This reduces complexity and boilerplate code.
- For very simple view layouts, avoid the use of .storyboard or .xib files. For example, if a view controller has a single edge-to-edge subview, this subview could be added to the `view` with a single line of code in `viewDidLoad`. One less file means the project is easier to navigate, easier to maintain, and less prone to file-merge conflicts.
- For more complex view layouts, use .storyboard or .xib files. The alternative would be large blocks of layout code; layout code is error-prone, and often difficult to debug and difficult to change. Weigh the benefits of a leaner project with fewer files against the complexity of the layout when making this decision.
- For view controllers that need a layout file, use a .storyboard instead of a .xib file. The main benefit of the .storyboard file is that it includes the top and bottom layout guides, which are very helpful when laying out the subviews of the view controller. When using a .xib file, the navigation bar will often cover-up part of the view. The band-aid for this issue is to add extra space at the top of the .xib layout, using a magic number of 64 points, which is the height of the 20 point status bar plus the 44 point navigation bar.
