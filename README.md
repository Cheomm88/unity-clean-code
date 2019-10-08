
# Unity Clean Code

This repository is dedicated to teach Unity developers of different backgrounds and programming skill levels to create cleaner code, which in most cases is also more maintainable and sometimes even faster! If you have any suggestions or found any errors below, be sure to submit an Issue or a Pull Request.

- [The Basics](#the-basics)
- [Identation](#identation)
- [Variables](#variables)
- [Methods](#methods)
- [Statements (if, for, etc.)](#statements)
- [Namespaces](#namespaces)
- [Advanced Tips and Patterns](#advanced-tips-and-patterns)
- [References](#references)

## The Basics
If you're reading this guide, you probably have a general understanding of what C# is, how to write (at least) some simple scripts in Unity and what not. However, I often see programmers struggle to truly understand the first principles they ever see when creating a new script. Let's take a look at it:

```csharp
using UnityEngine;
using System.Collections;

public class MyCustomScript : MonoBehaviour {

	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
	
	}
}
```

The code above is the basic template of a new C# script in Unity. We can divide it in a few portions.

```csharp
using UnityEngine;
using System.Collections;
```

The first lines of a script are dedicated to "importing" the necessary code libraries to make our code work. Think of it as dependencies: you can't use a feature of the UnityEngine library if you don't explicitly declare you are *using* them. Make sure you leave them at the very first lines of your script.

```csharp
public class MyCustomScript 
```

The script file we created defines a class (an abstraction of an object), that is public (accessible by other pieces of code), and with the name of the file you typed in. In this case, the class is called *MyCustomScript*. This is a **horrible** name! There is no way we can understand what this class does just by looking at its name (which should be your goal). Use meaninful names, like "PlayerHealth" or "Projectile", **always** in Pascal Case.

```csharp
                            : MonoBehaviour {

	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
	
	}
}
```

Starting at the very top (after public class MyCustomScript), we define what class (if any) our own class inherits from. In this case, it is Unity's MonoBehaviour. This is arguably the most important class in game development for Unity now, as it's what "marks" a class to be a component that can be attached to a GameObject. Inheriting from another class is not just marking it, it is what "enables you to create new classes that reuse, extend, and modify the behaviour that is defined in other classes". The methods Start and Update, for example, is something that Unity will only call during their predefined events if your class *is* a MonoBehaviour, which you will then extend the methods you need with the proper funcionality.

One common mistake to begginers is thinking that everything has to be a MonoBehaviour or inherit from something else. That is very far from the true, and actually a bad practice. For example, if you have a concept or abstraction that only handles data, consider making it a class that doesn't inherit from another class.

## Identation

At first glance, identation is a simple topic: managing *spaces* inside your code. It is what makes your code more readable by separating different words or symbols with a space, tab, or new line. Understanding the importance of a code with the correct identation is often hard to developers who are new to programming, and the best way I ever found to change that was to show a piece of code with really bad identations. Let's take our script template to a new level (of uglyness):

```csharp
// The code below is an example of *bad* identation.

using UnityEngine;   using System.Collections;
public class MyCustomScript    :MonoBehaviour{
//Use this for initialization
void Start() {
}
// Update is called once per frame
    void Update ( ) { } 
}
```

How hard it is for you to read the code above? If you're used to programming in Unity maybe not very much, but it is already more time-consuming to understand what this script does and where each part of the could should be. If we took our approach to a real script, with 50 or 100 lines or code, readability would be absolutely awful.

Many programming languages (C# included) have suggested guidelines or rules on how to properly ident your code, but truly learning it is not an easy task for most people. Luckily for us, there is a tool that will makes this process much easier: auto-formatting. If you're not sure you can handle identation by yourself, let the computer do the job for you and learn it by example. As you're probably using Visual Studio to code, my suggestion is to install the '*Productivity Power Tools*' plugin for Visual Studio and have the "Format document on save" option enabled. If you don't want to (or can't) install it, you can always use the "*Ctrl+K, Ctrl+D*" shortcut to format the currently opened script.

## Variables

As you probably know, variables (or members, technically speaking) are what stores the data of your application during runtime. Because your game probably has lots of data, having meaningful and readable names for them is vital. We will also talk about serialization of variables here, a topic of special importance in Unity.

### Naming

This is a tricky topic within the community. While certain languages have very strict naming conventions, C# is a little bit "relaxed" within a few cases, leaving the convention open for the organization to decide. With this in mind, what you will see below is a mix of what is the C# guidelines for naming variables (including fields and properties) and what is very common and/or widely adopted.

- Use meaningful names
```csharp
// Do
public int healthAmount;
public string teamName;
// Do NOT
public int hp;
public string tName;
```

- Use readable names
```csharp
// Do
public int movementSpeed;
// Do NOT
public int mvmtSpeed;
```

- Use nouns as names for variables
```csharp
// Do
public int movementSpeed;
// Do NOT
public int getMovementSpeed;
```

- Use the correct "casing" for the kind of variable
```csharp
public int movementSpeed; // Public variable, Camel Case
private int _movementSpeed; // Public variable, Camel Case with optional '_' at the start
public int Movement Speed { get; set; } // Property, Pascal Case
private const int MovementSpeed = 10; // Constant, Pascal Case
```

-  Avoid using abbreviations or single characters (unless it's math-related)
```csharp
// Do
public string groupName;
// Do NOT
public int grpName;

// Recommended for math-related scripts, like Vector2
public int x;
public int y;
```

- Explicitly use the 'private' keyword
```csharp
// Do
private bool _isJumping;
// Do NOT
bool _isJumping;
```

- Use the 'var' keyword when the second part of the variable attribution clearly reveals the type. Only for variables declared inside a method or scope (local variable)
```csharp
// Do
var players = new List<Players>();
// Do NOT
var players = PlayerManager.GetPlayers();
```

### Serialization

When you create a C# script using Unity and let the class inherit from MonoBehaviour, values that are "public" to the engine can be edited using the Inspector window. After doing it and saving the scene, these values are **serialized** (or saved) into the scene file.  As the goal of this is not to say when to use or not use serialized variables, let's focus on *how* you can declare a variable to be serialized:

```csharp
// Serialized
public int movementSpeed;
public int movementSpeed = 10;
[SerializeField] private int _movementSpeed;
[SerializeField] private int _movementSpeed = 10;

// NOT Serialized
private int _movementSpeed;
private int _movementSpeed = 10;
public int MovementSpeed { get; set; }
```

As a side note, attributes like the [SerializeField] can also be placed to the line above the declaration to improve readability, like this:

```csharp
[SerializeField]
private int _movementSpeed;
[SerializeField, AnotherCoolAttribute]
private bool _isEnemy;
```

## Methods

If you think about it, methods are the core part of a software: they **do** something. Because of their nature, when you create a method you should always name them using a verb. Don't forget what you learned above about naming variables: use descriptive, meaningful and readable names. Also remember to name methods using Pascal Case.

```csharp
// Do
public void SetInitialScore()
{

}

// Do NOT
public void InitialScore()
{

}

public void setInitialScore()
{

}

public void SetInitScr()
{

}
```

A key feature of methods are parameters. When creating them, use Camel Case and avoid prefixes.

``` csharp
// Do
public bool IsNewHighScore(int currentScore) 
{

}

// Do NOT
public bool IsNewHighScore(int CurrentScore) 
{

}

public bool IsNewHighScore(int _currentScore) 
{

}
```

Keep the structure of your methods clear with a low amount of information inside it. If your method contains more than 10 lines of code, it is probably a good candidate to be split into two or more methods. And finally, returning to the identation topic, follow the structure below for methods:

```csharp
public void DoSomething()
{ // Braces on a new line, starting at the same position of the method declaration.
	// Method content is a 'Tab' to the right after the braces.
	string somethingCool = "Cool";
	DoSomethingElse(somethingCool);
}
```

## Statements

## Namespaces

## Advanced Tips and Patterns

## References
- "*Clean Code: A Handbook of Agile Software Craftsmanship*" by Robert C. Martin
- "*Game Programming Patterns*" by Robert Nystrom
- C# Documentation by Microsoft
