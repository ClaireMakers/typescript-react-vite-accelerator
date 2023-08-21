# Type Aliases and Interfaces

### Type Aliases & Union Types: 

Type aliases are convenient ways to **store a particular type to re-use it throughout our programme** - in practice, it's the same as re-writing the actual type again and again. For instance:

```typescript
type Cat = {
    name: string,
    colour: string
};

const sigrid : Cat = { name: "Sigrid", colour: "Tortie" };

//The code above is the same as the following: 

const izzy : { name: string, colour: string } = { name: "Izzy", colour: "Black and white" };

```

If we want to create more than one cat throughout our programme though, it makes a lot of sense to create a custom type and give it an alias name like "Cat" in the example above. Type aliases can be given to any type, including union types: 

```typescript
type StringAlias = string;
type NumberAlias = number;

type StringOrNum = StringAlias | NumberAlias;
```

Aliases are also just that - they are not a distinct version of the type they represent. For instance, building on the example above: 

```typescript
const isItString = ( input: StringOrNum ): StringOrNum =>  {
    return input;
}

let string : string = "string"

isItString(string); => Won't throw an error even though I didn't pass an argument of type StringAlias directly,
StringAlias represents a string, so that's not incorrect.
```

StringAlias is just a **representation** of the type "string", renamed for our convenience. As far as the compiler is concerned, it's the same as the type "string". Properties can be made optional using "?" - this means is that if the property is set, it should have a specific type: 

```typescript
type Cat = {
    name: string,
    colour: string,
    age?: number
};

const sigrid : Cat = { name: "Sigrid", colour: "Tortie", age: "9"}
// => This will throw an error, because "age" is set to the wrong type, even if it's technically optional. 

```

### Practicing with Type Aliases & Union Types: 

Implement a function to calculate the area of a rectangle or a circle. 

- It should take a parameter of custom "Shape" type, which itself should be a union type of either "Rectangle" or "Circle" type.
- It should return the area of the shape passed as an argument. 
- It should throw an error if the type passed to the function isn't "Shape".
- It should throw an error if the width, length and radius properties of the types Rectangle and Circle aren't numbers.

<br>
As a quick reminder, here are the formulas you'll need (we'll use 3.14 for Pi): 

```typescript
Area of a rectangle = length × width .

Area of a circle = 3.14 * square radius

```

Here is an example of what you should get if running the code: 

```typescript
const rectangle: Rectangle = { width: 5, length: 10 }

console.log(calculateArea(rectangle)); 
// Expected output: 50

const circle: Circle = { radius: 10 }

console.log(calculateArea(circle))
//Expected output: 314

const rectangle: Rectangle = { width: "5", length: "10" }
// should throw an error

const circle: Circle = { radius: "radius" }
// should throw an error

console.log(calculateArea({ random: "object" }))
//should throw an error
```

Your code should be test-driven using Jest. 

### Interfaces & Intersection Types: 

Interfaces are essentially named object types - for instance, these three ways of typing are very similar: 

```typescript
const sigrid : { name: string, age: number } = { name: "Sigrid", age: 9 };

interface Cat {
    name: string, 
    age: number
} 

type Cat {
    name: string, 
    age: number
}
```
Interfaces, unlike types, **are open ended**, which means that **properties can added after they are initialised**, as they function like objects. Types, on the other hand, once declared with an alias and initialised, cannot be changed. 

Interfaces also have some features that types on their own do not, such as, for instance, the ability to **extend from another interface or type** (a type cannot extend from an interface):

```typescript
type Animal {
    isWild: boolean 
}
// type Animal extends interface x would not compile, because types cannot be extended. 

interface Pet extends Animal {
    name: string, 
    age: number
}
// This works because Pet is an interface extending from the type Animal, not the other way around.

interface Cat extends Pet {
    colour: string,
    indoorsOnly: boolean
}
// Cat is an interface extending from another interface, and inherits everything from Pet, including what it took from Animal.

```

We touched upon union types before, and it is possible to use interfaces to build them up: 

```typescript
interface Pet {
    name: string, 
    age: number,
    colour: string,
}

interface Cat extends Pet {
    indoorsOnly: boolean
}

interface Dog extends Pet {
    likesBellyRubs: boolean
} 

type CatOrDog = Cat | Dog; 
// Any value typed as CatOrDog can now be of either type. 
```
There is another powerful tool you can use with Typescript, called **intersection types**. These are a bit similar to using the "extend" keyboard, because they will let you create a type that **combines several interfaces/types together**, as opposed to giving different alternatives like union types do. These allow you to **save new types under aliases**, whereas the **extend keyword preserves the interface's distict properties**, such as the ability to add to it later on in you programme. 

```typescript
interface Pet {
    name: string, 
    age: number,
    colour: string,
}

interface Cat {
    indoorsOnly: boolean
}

type PetCat = Pet & Cat; 
// PetCat is { name: string, age: number, colour: string, indoorsOnly: boolean }

type Dog {
    likesBellyRubs: boolean;
}

type PetDog = Pet & Dog;
// You can mix types and interfaces with intersection type to build up custom types. 
```

Intersection types can for instance be really useful in functions where you want the argument to have characteristics shared across multiple types/interfaces:

```typescript
interface Pet {
    name: string, 
    age: number,
    colour: string,
}

interface Cat { 
    indoorsOnly: boolean
}

const isItIndoorCat = ( arg: Pet & Cat ) : boolean => {
    if( arg.indoorsOnly === true ) {
        return true;
    }

    return false;
}
//This function expects an argument structured like so: { name: string, age: number, colour: string, indoorsOnly: bolean }
```

### Practicing:

Complete the code below to write a middleware function that returns a Session object made up of two object properties: one of "user", containing all the relevant information about the user, and one of "permissions", containing all the relevant permissions for the user connecetd. Notice how you can use an enum as a type - TypeScript allows for a lot of flexibility like this, and you'll get to explore it over the course of this week and the next.  

```typescript
enum PermissionLevel { 
	PERMISSION_LEVEL_ONE = 1,
	PERMISSION_LEVEL_TWO,
	PERMISSION_LEVEL_THREE
}

interface User {
	username: string,
	password: string,
	permissionLevel: PermissionLevel,
}

interface PermissionLevelOne {
	canDeletePost: boolean,
}

interface PermissionLevelTwo {
	canSuspendAccount: boolean,
}

interface PermissionLevelThree {
	canDeleteUser: boolean,
}

const permissionsGrantingMiddleWare = () => {}
```

Expected behaviour: 

```typescript
permissionsGrantingMiddleWare({ username: "Claire", password: "BanoffeePie", permissionLevel: PermissionLevel.PERMISSION_LEVEL_ONE })

// Should output: 
session {
    user: {
        username: "Claire", 
        password: "BanoffeePie", 
        permissionLevel: PermissionLevel.PERMISSION_LEVEL_ONE //( or 1, since it's the corresponding enum value )
    },
    permissions: {
        canDeletePost: true,
		canSuspendAccount: false,
		canDeleteUser: false,
    }
}
```

Draw on the features of TypeScript discussed above to come up with your solution, building on the interfaces and the enum provided. Use Jest to test-drive your solution.

<details>
<summary>Reveal potential solution</summary>

```typescript
enum PERMISSION_LEVEL { 
	PERMISSION_LEVEL_ONE = 1,
	PERMISSION_LEVEL_TWO,
	PERMISSION_LEVEL_THREE
}

interface User {
	username: string,
	password: string,
	permissionLevel: PERMISSION_LEVEL,
}

interface PermissionLevelOne {
	canDeletePost: boolean,
}

interface PermissionLevelTwo {
	canSuspendAccount: boolean,
}

interface PermissionLevelThree {
	canDeleteUser: boolean,
}

type PermissionsGranted = PermissionLevelOne & PermissionLevelTwo & PermissionLevelThree;

interface Session {
	user: User 
	permissionObj: PermissionsGranted;
}

const permissionsGrantingMiddleWare = ( user: User ) : Session => {

	const permissionsGranted: PermissionsGranted = {
		canDeletePost: false,
		canSuspendAccount: false,
		canDeleteUser: false,
	}
	
	const session: Session = {
		user: user,
		permissionObj: permissionsGranted,
	}
		
	if (user.permissionLevel === PERMISSION_LEVEL.PERMISSION_LEVEL_ONE ) {
		permissionsGranted.canDeletePost = true;
	}
	if ( user.permissionLevel === PERMISSION_LEVEL.PERMISSION_LEVEL_TWO ) { 
		permissionsGranted.canSuspendAccount = true;
	}
	if (user.permissionLevel === PERMISSION_LEVEL.PERMISSION_LEVEL_THREE) { 
		permissionsGranted.canDeleteUser = true;
	}

	return session;
}
```
</details>
<br>

[Next Section](https://github.com/ClaireMakers/typescript-react-vite-accelerator/blob/main/oop-in-and-generics-in-typescript.md)

