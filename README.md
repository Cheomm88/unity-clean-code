# Unity Clean Code

Este repositorio está indicado para los desarrolladores de diferentes niveles que utilizan Unity, para que puedan crear código más limpio y en la mayoría de los casos más fácilemtene mantenible y en ocsasiones incluso más rápido. Si tienes alguna sugerencia o has encontrado algún error puedes ponerla como una incidencia o un pull request.

Este guía no cubre todo sobre el Clean Code para Unity, pero si las partes más imporantes y de manera consisa. Si quieres aprender más te sugerimos leer [C# programming guide](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/) en cuanto a dudas específicas sobre las características de C# el libro "*Clean Code: A Handbook of Agile Software Craftsmanship*" de Robert C. Martin te dará una visión mayor sobre Clean Code.

En esta guía trataremos sobre todo conveciones para el lenguaje C#. Recuerda, que de todos modos, estas convencions con mucha frecuencia son moidifcadas **al gusto** de las organizaciones/empresas por diversos motivos (Unity y Microsoft incluidas). Si tienes una buena razón para no hacer uso de ellas, se libre! Pero recuerda que si usas una convención diferente debe hacerse en todo el proyecto.

- [The Basics](#the-basics)
- [Identation](#identation)
- [Variables](#variables)
- [Methods](#methods)
- [Statements](#statements)
- [Namespaces](#namespaces)
- [Comments](#comments)
- [Automated Tests](#automated-tests)
- [Advanced Tips and Patterns](#advanced-tips-and-patterns)
- [References](#references)

Esta guía está basada en [Unity Clean Code](https://github.com/sampaiodias/unity-clean-code)

## The Basics
Si estás leyendo esta guía, probablemente ya tengas conocimiento general sobre C#, cómo escribir algún script simple y qué cosas no hacer. Sin embargo en ocasiones los programadores se pueden encontrar con problemas para realmente entender los primeros principios que vemos cuando se crea un script. Vamos a verlo: 

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MyCustomScript : MonoBehaviour {

	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
	
	}
}
```

El código de aquí arriba es la plantilla de un nuevo script en C# de Unity. Podemos dividirlo en varias secciones.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
```

En las primeras líneas son las que "importan" las librerías de código necesarias para que nuestro código funcione. Piensa en ellas como dependencias: no puedes usar una característica de la librería UnityEngine (Unity) si no expecificas en una declaración que las estás *usando*. Asegurate de dejarlas siempre en las primeras líneas de código

```csharp
public class MyCustomScript 
```

El fichero que hemos creado define una clase (una abstracción de un objeto), que es público "public" (accessible por otros scripts/códigos), y su nombre es el mismo que el del fichero. En este caso, se llama *MyCustomScript*. Este es un nombre **horrible** ! No podemos entender que hace la clase simplemente al leer el nombre (ese es tu objetivo). Siempre usa nombres con significado como "PlayerHealth" (SaludJugador) o "Projectile" (Proyectil), **siempre** usando "Pascal Case", es decir, cada palabra tendrá su primera letra en mayúscula.

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

En la parte superiora (justo tras public class MyCustomScript), definimos que clases (si es que hay alguna) vamos a heredar. EN este caso, es la clase MonoBehaviour de Unity. Esta es sin duda la clase más importante en el desarrollo de juegos para Unity ahora mismo, podríamos decir que "marca" esta clase como un componente que podremos adjuntar/asignar a un GameObject. Heredar de otra clase no es solo "marcar", es lo que nos "permite crear nuevas clases que se reutilizan, expanden y modifican el comportamiento que está definido en otras clases". Los métodos Start y Update, por ejemplo, son algo que Unity solo llamará durante sus [eventos predefinidos](https://docs.unity3d.com/Manual/ExecutionOrder.html) si tu clase *es* un MonoBehaviour, en la que ampliaras los métodos que necesitas con las funcionalidades adecuadas. (Por ejemplo: en el Update podrás modificar la posición de un objeto).

Un error común entre los que se inician, es pensar que todo debe ser un MonoBehaviour o heredar de otra clase. Esto está muy lejos de la realidad, y se trata de una mala prácticas. Por ejemplo, si tienes un código que solo manipula datos, podrías considerarlo como una clase que no herede de MonoBehaviour.

## Identation

A primera vista, la identación es un aspecto simple: poner *espacios* dentro de tu código. Lo que hará que sea más legible al separar palabras o símbolos con un espacio, tabulador o una nueva linea. Entender la importancia de la identación es en ocasiones dificil para los que son nuevos en la programación, y la mejor forma que he encontrado de explicarlo es cambiar un código y ponerle muy mala identación. Vamos a llevar la plantilla de Unity a un nuevo nivel (de fealdad):

```csharp
// The code below is an example of *bad* identation.

using System.Collections; using System.Collections.Generic;
using UnityEngine;
public class MyCustomScript    :MonoBehaviour{
//Use this for initialization
void Start() {
}
// Update is called once per frame
    void Update ( ) { } 
}
```

¿Es difícil leer el código de arriba? Si estás acostumbrado a programar en Unity tal vez no mucho, pero lleva más tiempo entender que hace este script y donde está cada parte. Si llevamos este ejemplo a un script real, con 50 o 100 líneas de código,la legibilidad sería terrible.

Muchos lenguajes de programación (incluido C#) han sugerido líneas o regalas sobre como identar correctamente tu código, pero aprender esto no es taría fácil para todo el mundo. Por suerte para nosotros, existe una herramienta que hará este proceso mucho más fácil: el auto-formato. Si no estás seguro que puedes llevara cabo la identación tuy mismo, deja que el ordeandor haga el trabajo por ti. Si estás utilizando Visual Studio para programar, puedes instalar el plugin '*Productivity Power Tools* y tener la opción "Format document on save" activada. Sino quieres o no puedes hacer esto, siempre puedes utilizar el atajo "*Ctrl+K, Ctrl+D*" para formatear tu script correctametne.


## Variables

Como probablemente sepas, las variables (o miembros, hablando técnicamente) son donde se almacena los datos de tu aplicación durante la ejecución. Porque tu juego probablemente tenga un monton de datos, tener nombres con significado propio y fácilmente legibles es algo vital. También hablaremos de la serialización de variables aquí, un punto realmente importante en Unity,

### Naming

Este es un aspecto difícil entre la comunidad. Mientras que ciertos lenguajes tienen una convención muy estricta en cuantos los nombres de variables, C# es algo más "relajado", dejando de lado que la conveción estará en manos de la emprsa. Con esto en mente, vamos a ver ejemplos de como llamar variables y que es común.


- Usar nombres con significado propio
```csharp
// Do
public int healthAmount;
public string teamName;
// Do NOT
public int hp;
public string tName;
```

- Usa nombres legibles
```csharp
// Do
public int movementSpeed;
// Do NOT
public int mvmtSpeed;
```

- Usa sustantivos como nombres para las variables
```csharp
// Do
public int movementSpeed;
// Do NOT
public int getMovementSpeed;
```

- Usa las mayúsculas de roma correta para el tipo de variable.
```csharp
public int movementSpeed; // Variable pública, Camel Case (primera palabra minúscula las siguientes con una mayúscula)
private int _movementSpeed; //Variable privada, Camel Case con un '_' OPCIONAL al inicio
public int MovementSpeed { get; set; } // Propiedad, Pascal Case (Cada palabra con una mayuscula)
private const int MovementSpeed = 10; // Constante, Pascal Case
```

-  Evita usar abreviaciones o un solo carácter (a no ser que sea algo matemáico)
```csharp
// Do
public string groupName;
// Do NOT
public int grpName;

// Recommended for math-related scripts, like Vector2
public int x;
public int y;
```

- Explicita el término 'private'
```csharp
// Do
private bool _isJumping;
// Do NOT
bool _isJumping;
```

- Usa la palabra clave 'var' solo cuando en la asignación se pued ver claramente el tipo de dato que almacena. Usar solo en variables locales (declaradas dentro de un método/función/ámbito)
```csharp
// Do
var players = new List<Players>();
// Do NOT
var players = PlayerManager.GetPlayers();
```

### Serialization

Cuando tienes un script en C# utilizando Unity y heredas de la clase MonoBehaviour, los valores que son públicos serán accesibles para su edición desde la ventana del Inspector. Tras hacer y guardar la escena, estos valores son **serializados** (o guardardados) en el fichero de la escena. El objetivo de este punto no es decir cuándo o cuándo no utilizar variables serializables, sino en *cómo* declarar una variables para que sea serializada:


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

Como nota, los atributos como [SerializeField] pueden también ponerse por encima de la declaración para mejorar la lectura del código, de la siguiente manera:

```csharp
[SerializeField]
private int _movementSpeed;
[SerializeField, AnotherCoolAttribute]
private bool _isEnemy;
```

## Methods

Si lo piensas, los métodos/funciones son el núcleo de un programa: ellos **hacen** algo. Por su naturaleza, cuando creas un método/función deberías utilizar un nombre con un verbo. No olvides lo aprendido con los nombres de las variables variables: debe ser descriptivo, con signiificado y fácil de leer. Así como utilizar la nomenclatura "Pascal Case" (Cada palabra con la primera letra en mayúscula)

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

Una de las características principales de los métodos/funciones, son los parámetros. Al crearlos usa Camel Case y evita los prefijos.

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

Mantén la estructura de tus funciones/métodos limpia con una baja cantidad de información en ella. Si tu función contiene más de 10 líneas de código, es prodablmente una buena candidada en ser partida en dos o más funciones. Finalmente, volviendo al tema de la identación, sigue la estructura de la siguiente para tus métodos.

Keep the structure of your methods clear with a low amount of information inside it. If your method contains more than 10 lines of code, it is probably a good candidate to be split into two or more methods. And finally, returning to the identation topic, follow the structure below for methods:

```csharp
public void DoSomething()
{ // Las llaves en una nueva línea, a la altura de la declaración de la función
	// El contenido de la función tiene un "Tabulador" a la derecha después de la llave.
	string somethingCool = "Cool";
	DoSomethingElse(somethingCool);
}
//La llave de cierre del método a la misma altura que la de apertura.
```

## Statements

Statements (sentencias), son definidas por Microsoft, como "acciones que hace el programa", como declarar variables, llamar a métodos, bucles dentro de collecciones. Podemos definir dos categorías sentencias de una línea o de varias líneas.
```csharp
private void DoSomething()
{
	// Single-line statement. In this case, declaring and initializing a variable;
	var randomNumbers = new int[3];

	// Multi-line statement. In this case, looping through 'randomNumbers'
	foreach (int number in randomNumbers)
	{
		DoSomethingElse(number);
	}
}
```

Para las sentencias de una sola línea no hay mucho que hablar más allá de mantener cada sentencia en una línea, cuando las líneas no sean muy largas. Aunque existe una excepción: los saltos de línea. Puedes usarlos para dividir sentencias muy largas.

```csharp
// Do

Debug.Log("This is just an example log that will print a long text with the values of the script: "
	+ playerName + " " + playerHealth);

string logMessageForPlayersInformation = 
	PlayerManager.GetPlayerInformation(GetPlayerIndex("Player1")) +
	PlayerManager.GetPlayerInformation(GetPlayerIndex("Player2"));
Debug.Log(logMessageForPlayersInformation);

// Do NOT

// Too long
Debug.Log("This is just an example log that will print a long text with the values of the script: " + playerName + " " + playerHealth); 

string logMessageForPlayersInformation = 
	PlayerManager.GetPlayerInformation(
		GetPlayerIndex("Player1")) +
	PlayerManager.GetPlayerInformation(
		GetPlayerIndex("Player2"));
Debug.Log(logMessageForPlayersInformation);
```

La complejidad llega cuando trabajamos en sentencias multi-línea. Cuando decides crear bloques de código dentro de otro bloque de código, tu script se encontrará con un problema similar al de arriba. Veamos otro ejemplo:

```csharp
var someValue = 100;
var myValues = new int[10, 5]
for (int i = 0; i < 10; i++)
{
	for (int j = 0; j < 5; j++)
	{
		if (i >= 1)
		{
			someValue--;
		}
		else
		{
			while (someValue > 50)
			{
				someValue -= 10;
			}
		}
		myValues[i, j] = someValue;
	}
}
```

The code above is not meant to do something useful in particular, but if you were given the task to understand what this algorithm does, that would be a hard job. The amount of complexity added to the code because of these **nested** statements make their readability and maintainablity really low. And trust me, I've seen worse examples of pieces of code that actually shipped to a real game. As a general rule of thumb, keep your methods with a maximum of one multi-line statement nested inside another multi-line statement. When possible, no nesting or even no multi-line statements at all is desirable.

```csharp
// Goal
var someValue = 100;
var myValues = new int[10, 5]
for (int i = 0; i < 10; i++)
{
	for (int j = 0; j < 5; j++)
	{
		DoSomething(someValue, i, j);
	}
}

// Ultimate Goal
int[,] myValues = CalculateValueMatrix(100, 10, 5);
```

There is another curious feature of multi-line statements that can produce some weird-looking code. When a multi-line statement only contains one statement inside its scope, you can remove the braces and the compiler will not throw any errors. There are situations where doing this makes your code much cleaner, but sometimes it is the opposite. Below are some examples of how to approach this, but whatever you do, make this choice consistent across your code.

```csharp
// Do

for (int i = 0; i < 10; i++)
	DoSomething();
	
if (_isJumping == false) return;

// Avoid

if (_isJumping == false) DoSomething();

// Do NOT

// Nesting multi-line statements without braces
for (int i = 0; i < 10; i++)
	for (int j = 0; j < 10; j++)
		DoSomething();
```

## Namespaces

Maybe one of the most useful features of C#, namespaces are (in short) a way to better organize your scripts. Unfortunately, namespaces were also one the most underused C# features by Unity developers for quite some time (maybe because of Unity's very own scripting documentation not showing it properly), but that's something I'm personally seeing some changes within the community. Namespaces are not only useful, they are powerful, but only when you properly use them. And the most important part about it is... naming them!

Before we go to what you should or shouldn't do with namespaces, take a look at this familiar piece of code:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
```

Every new MonoBehaviour is *using* theses namespaces: UnityEngine, System.Collections and System.Collections.Generic. The UnityEngine namespace is provided by Unity and contains a lot of scripts, like the MonoBehaviour class. The other ones are provided by Microsoft and and also contain many classes and abstractions that define various collections of objects, such as lists and dictionaries. One important part of namespaces is that you can create namespaces inside another namespace. For example, Collections is actually nested inside of the System namespace.

Now that these concepts are behind us, let's take a look at how to create them and how to name them!

```csharp
using System;

namespace Company.Product.Feature
{
	public class Example
	{
	
	}
}
```

After declaring what other namespaces this file is using, we create a namespace and encapsulate all other code inside the scope of this namespace. The naming for it should start with the organization name and then the name of the product (for example: Github.ExampleProduct). All names are in Pascal Case, with dots separating the nested hierarchy. You may or may not continue with the nesting, using now the name of a specific feature of your product (for example: Github.ExampleProduct.Database). All names should be in singular, but consider using plural when it makes the name of the namespace better explain what it contains (like Collections, of System.Collections). Finally, don't use prefixes or other symbols, like underscores.

## Comments

With the honorable objective of helping humans understand the code, comments are an incredible way of documentating your scripts and improving maintainability. They are easy to use, but also easy to use in excess. To increase the quality of your code, here are the simple guidelines to when you should create a comment:

1) To document a class, method, enum, or struct.
2) To serve as a header of the file (mainly for a copyright notice).
3) To explain a statement that is inherently complex or not obvious to understand.

Let's explore each of these three situations, starting with first one. If you type */* (dash symbol) three times on the line above of one of the structures mentioned above, a special comment section will show up.

```csharp
/// <summary>
/// 
/// </summary>
public class Example
{

}
```

This is not a convention to just make your comments pretty or easier to read. This is actually a standard to help automated tools (like your best friend IntelliSense) to parse your comments and generate useful content. Because of this, be sure to use this pattern to document your code.

```csharp
// Do

/// <summary>
/// Calculates the total area of a circle.
/// </summary>
/// <param name="radius">The radius of the circle</param>
private float CalculateCircleArea(float radius)
{
    return 3.14f * radius * radius;
}

//Do NOT

// Calculates the total area of a circle, given a certain radius.
private float CalculateCircleArea(float radius)
{
    return 3.14f * radius * radius;
}
```

For the second situation there are not many standards, you are free to type in almost any way you need, as long as it starts in the very first line of the document. Here's an example:

```csharp
// --------------------------------------------------------------------------------------------------------------------
// <copyright file="Script.cs" company="{Company}">
//
// Copyright (C) 2019 {Company}
//
// This program is free software: you can redistribute it and/or modify
// it under the +terms of the GNU General Public License as published by
// the Free Software Foundation, either version 3 of the License, or
// (at your option) any later version.
//
// This program is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.
//
// You should have received a copy of the GNU General Public License
// along with this program.  If not, see http://www.gnu.org/licenses/. 
// </copyright>
// <summary>
// {one line to give the program's name and a brief idea of what it does.}
// 
// Email: {Email}
// </summary>
// --------------------------------------------------------------------------------------------------------------------

// Rest of the code goes here
```

The last situation is where inexperienced programmers "abuse" comments, putting them on a huge number of statements. This is unnecessary, and it actually does more harm than good! Creating comments should be for special occasions, and not the norm. It is hard to say exactly when you should use them, so follow the example below (where the comments are completely useless) so you don't make the same mistakes.

```csharp
// Do NOT! Once again, this is an example of what you should NOT do!

private void CreateEnemy(GameObject prefab)
{
	// The GameObject of the enemy to be instantiated
	GameObject enemy;
	// Creates the enemy in the scene
	enemy = Instantiate(prefab)
	
	enemy.GetComponent<Enemy>().InitiateMovementBehaviour(); // Makes the enemy walk around the world
}
```

## Automated Tests

## Advanced Tips and Patterns

## References
- "*Clean Code: A Handbook of Agile Software Craftsmanship*" by Robert C. Martin
- "*Game Programming Patterns*" by Robert Nystrom
- C# Documentation by Microsoft
