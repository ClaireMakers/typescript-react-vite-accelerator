## Generics and OOP 

### Generics: 

Generics allow you to write functions, classes and interfaces for which specific types will be declared later in the programme's lifecyle. They increase the re-usability and readability of your code - for instance, you can write a function that will take more than one argument type without having to list every single one in its declaration, while still enforcing proper typing throughout.  

For instance: 

```
const identity = <Type>( arg: Type ) : Type {
    return arg; 
}
// => This is function can take and return any type of argument. It also captures the type provided when calling the function, as opposed, to, for example, the "any" type, which loses us access to that information. 

// => The argument and the return value of the function must both be of "Type" - this would be incorrect if our "Type" wasn't of number, and this wouldn't be great code as it's likely to break that rule: 

const identity = <Type>( arg: Type ) : Type {
    return 0; 
}
```

You can use generics in combination with other TypeScript features - for instance, if we want to write a function that works on a set of type, and we have some idea in advance of these will be, we can add a constraint on a particular generic type. For instance: 

```
const logInfo = <Type>(arg: Type): void => { 
	console.log(arg.length)
}
// This will throw an error, because there isn't a "length" property to refer to - we can narrow down the potential types for Type using an interface. 

interface LengthProp { 
	length: number
}

const logInfo = <Type extends LengthProp>(arg: Type): void => { 
	console.log(arg.length)
}
// Now, the expected input for this function will need to have a .length property, so strings and arrays will be acceptable. 
```

Generics can also be used widely when writing object-oriented programmes. 

### OOP in TypeScript:

The fundamental building blocks of OOP in TypeScript are very similar to JavaScript, with the added complexity of typing your programme accurately and consistently. You can use all the tools mentionned prior to this section to create classes that are meaningfully typed. Encapsulation is even easier to manage in TypeScript OOP than in JavaScript - for instance, you can set your instance variables and methods to public, protected or private, depending on your needs. 

The default visibility for any class members is **public** - this means that they can be used and access from any other place in your code. 

```
class Cat {
	public name: string; 

	constructor(name: string) {
		this.name = name;
	}
}
```

You don't necessarily need to add the "public" keyword in that scenario, but may choose to do so for documentation/clarity purpose. 

Protected class members will be accessible and readable within their class as well as within any subclass: 

```
class Pet {
	animalTypeStr: string; 

	constructor( animalType: string ) {
		this.animalTypeStr = animalType;
	}

	protected getAnimalType() : string {
		return this.animalTypeStr;
	}
}

class Cat extends Pet {
	catName: string; 

	constructor(name: string, animalType: string) {
		super(animalType);
		this.catName = name;
	}

	sayAnimalType(): void {
		console.log("I am a " + this.getAnimalType());
	}
}

```

In this example, the ```getAnimalType()``` method can be used either as part of the Pet class, or the cat class, but not outside either of them - for instance, this would throw an error: 

```
const cat = new Cat("Sigrid", "Cat");
cat.getAnimalType() 
//the method is only accessible within Pet or Cat themselves, not outside. 
```

Finally, you can set class members to "private", meaning that they can't be accessed outside of the class at all, including any subclasses extending it: 

```
class Cat {
	private name: string; 

	constructor(name: string) {
		this.name = name;
	}
}
```

You can use generics in combination with class declarations to create generic classes, which will allow you a certain control over typing in a class, without being over-prescriptive:  

```
class Cat<Type> {
  name: Type;
  constructor(catName: Type) {
    this.name = catName;
  }
}
```

Classes can also be used at types in TypeScript. For instance:

```
class Cat {
	public name: string; 

	constructor(name: string) {
		this.name = name;
	}
}

const sigrid : Cat = new Cat("Sigrid");
```

TypeScript will check that the object created matches the structure of the class ```Cat```. This means that it will check the object created shares its properties with the Cat class. In that scenario, they behave a little bit like an interface. There are some limits though as to what the compiler will "recognise" from that class - it will make sure all the properties match, but will not enforce method implementations. This is because it doesn't check the value is necessarily an instance of that particular class, only that the object **matches** the properties of this class. In practice:

```
class Cat {
	public name: string; 

	constructor(name: string) {
		this.name = name;
	}

	meow() : void => {
		console.log("meow");
	}
}

const sigrid : Cat = { name: "Sigrid", meow: () => { console.log("I can talk!!") }};

//This will not throw an error, because this object has a property of name, which is a string, and a property "meow" that is a void function. I can re-implement the content of the function so long as I respect whatever type I set as its return value/arguments/etc. 
```

[Here's an article that goes into depth about OOP and TypeScript if you want to explore those concepts in more depth.](https://archive.ph/U9XwR)

### Practice OOP & Generics: Playlist shuffler:

Create a playlist shuffler programme! 

- You should implement it using at least two classes, Playlist and Song, and the Song class should be used as a type at least in one place in your implementation.

- Your Playlist class should have a way to store, add and delete songs as and when needed. 

- Your Playlist class should have a "shuffle" method that when called with a shuffling order, will return the songs in the playlist in the correct order. You should aim to implement the three following shuffling orders: random, alphabetical song title order and longest song first. 

- The shuffle method should do two things: return a new array, that you will store as you see fit, and print the shuffled playlist to the console.



