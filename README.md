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

Si lo piensas, los métodos/funciones son el núcleo de un programa: ellos **hacen** algo. Por su naturaleza, cuando creas un método/función deberías utilizar un nombre con un verbo. No olvides lo aprendido con los nombres de las variables variables: debe ser descriptivo, con significado y fácil de leer. Así como utilizar la nomenclatura "Pascal Case" (Cada palabra con la primera letra en mayúscula)

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

El código de encima no está diseñado para hacer algo útil en particular, pero si tuvieras la tarea de entender que hace este algoritmo, sería una tarea difícil. La cantidad de complejidad añadida debido a que el código está **anidado** hace que la legibilidad y mantenimiento del código sea realmente malo. Créeme, he visto ejemplos peores que han llegado a salir en juegos reales. Como regla general, manten tus métodos con un máximo de una sentencia multi-línea anidado dentro de otra sentencia multi-línea. Cuando sea posible, evita anidar código o incluso evita las sentencias multi-líneas.

```csharp
// Objetivo
var someValue = 100;
var myValues = new int[10, 5]
for (int i = 0; i < 10; i++)
{
	for (int j = 0; j < 5; j++)
	{
		DoSomething(someValue, i, j);
	}
}

// Objetivo final
int[,] myValues = CalculateValueMatrix(100, 10, 5);
```

Existe otra curiosa característica de las sentencias multi-línea que puede generar tener un código con una apariencia rara. Cuándo una sentencia multi-línea solo tiene una línea dentro, puedes eliminar las llaves y el compilador no dará ningún error. Esto podría ayudar a hacer tu código más limpio, pero en ocasiones hace justo lo contrario. A continuación verás ejemplos de como llevar a cabo esto, hagas lo que hagas, debes mantener una consistencia en tu código. (Yo particularmente prefiero siempre mantener las llaves)

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

Puede que una de las características más útiles de C#, los namespaces una mejor forma de organizar tus scripts. Desfortunadamente también son una de las características menos usadas de C# por los desarrolladores de Unity. No solo son útiles, son potentes pero solo cuando son usados adecuadamente. Y una parte vital es nombrarlos bien.

Antes de ver que debes o no debes hacer con los namespaces, mira la siguiente parte de código:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
```

Cada nuevo MonoBehaviour está *usando* estos namespaces: UnityEngine, System.Collections y System.Collections.Generic. El UnityEngine namespace es de Unity y contiene muchos scripts, como la clase MonoBehaviour. Los son de Microsoft otros contienen clases y abstracciones de varias colecciones de objetos, como listas y diccionarios. Una parte importante de los namespaces es que puedes crear namespaces dentro de otro namespace. Por ejemplo, Collecions está dentro del namespace System.

Ahora tras estos conceptos, veamos como crear y nombrar un namespace.

```csharp
using System;

namespace Company.Product.Feature
{
	public class Example
	{
	
	}
}
```
Tras declarar que otros namespaces se usan en este fichero, vamos a crear un namespace y encapsular todo el código dentro de este ámbto del namespace. El nombre debería empezar con el nombre de la organización/empresa y después el del producto (por ejemplo: Github.EjemploProducto). Todos los nombres irán en Pascal Case, con los puntos separando la jerarquía anidada. Tu podrías decidir si seguir anidando o no, utilizando el nombre de una característica específica de tu producto (por ejemplo: Github.EjemploProducto.DataBase). Todos los nombres deben estar en singular, pero considera los plurales cuando permitan explicar mejor que contienen (como por ejemplo Collections, como vemos en System.Collections). Por último no utilices prefijos u otros símbolos como guiones.

## Comments

Con el honorable objetivo de ayudar a los humanos a entender el código, los comentarios son una increíble forma de documentar tus scripts y mejorar el mantenimiento de los mismos. Son fáciles de usar, pero también fáciles de usar en exceso. Para incrementar la calidad de tu código, aquí van unas pequeñas reglas que deberías aplicar a la hora de crear tus comentarios:
1) Úsalos para documtar una clase, función, enum o strcut.
2) En la cabcera de tu fichero (principalmnete para información de copyright)
3) Para explicar una línea que es complicada o difícil de entender.

Vamos a revisar estas tres situaciones, empezando por la primera. Si escribes  */* (la barra) tres veces encima de una línea que contenga una de las escructuras nombradas en el primer paratado, un comentario especial surgirá.

```csharp
/// <summary>
/// 
/// </summary>
public class Example
{

}
```

Esto no es una conveción para hacer tus comentarios más bonitos o fáciles de leer. Es un standar para ayudar a las herramientas automatizadas (como IntelliSense) a parsear tus comentarios y generar contenido útil. Por este motivo, asegurate de documentar el código utilizando este método.

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

Para el segundo caso, no hay muchos standards, siéntete libre de hacerlo de la mejor manera que consideres siempre y cuando empiece en la primera línea del documento. Por ejemplo:

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

La última situación es donde los programadores con poca experiencia "abusan" de comentarios, utilizandolos demasiado. Esto es nnecesario, y actualmente hace más daño que bien. Crear comentarios debería ser para ocasiones especiales, y no la norma. Esto es difícil de decir cuándo puedes usarlos, veamos el siguiente ejemplo (donde los comentarios son totalmente innecesarios) así que no repitas esto!

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
