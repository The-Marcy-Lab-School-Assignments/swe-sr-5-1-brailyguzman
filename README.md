# Technical Writing Assignment

For guidance on setting up and submitting this assignment, refer to the Marcy lab School Docs How-To guide for [Working with Short Response and Coding Assignments](https://marcylabschool.gitbook.io/marcy-lab-school-docs/fullstack-curriculum/how-tos/working-with-assignments#how-to-work-on-assignments).

## Prompt 1 - Braily

Imagine you are teaching a friend about OOP. They mainly want to understand what is Encapsulation. Write a brief lesson on Encapsulation that includes the following:

-   What is encapsulation?
-   What major goal does this help to achieve in software engineering?
-   Give an example (in code) of encapsulation.
-   An explanation of how the code example demonstrates encapsulation

### Response 1

In OOP, **encapsulation** is a fundamental concept that consists of bundling data and methods into a single unit. In _software engineering_, the main goal this helps achieve is to prevent data from being directly accessed, while still allowing access to the data through the object's functions.

```js
class Person {
    #friends = [];

    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    getFriends() {
        return [...this.#friends];
    }

    addFriend(newFriend) {
        if (!newFriend || typeof newFriend !== 'string') return;
        this.#friends.push(newFriend);
    }
}
```

#### How This Demonstrates Encapsulation

1. Private Field (`#friends`):
   The friends array is private, meaning it cannot be accessed directly from outside the class. This ensures that only controlled methods (`getFriends` and `addFriend1) can modify or retrieve the data.

2. Getter Method (`getFriends`):
   This method returns a copy of the `friends` array rather than the original, preventing external code from modifying it directly.

3. Setter Method (`addFriend`):
   This method ensures that only valid string values are added to the `friends` array, preventing unintended or incorrect data from being stored.

By encapsulating the `friends` array, we protect internal data, enforce controller access and prevent unintended side effects in the program.

## Prompt 2 - Suru

The following `friendsManager` object is an example of an interface that is **NOT** consistent and predictable:

```js
const friendsManager = {
    friends: [],
    addFriend(newFriend) {
        if (typeof newFriend !== 'string') return;
        this.friends.push(newFriend);
    },
};

friendsManager.addFriend('daniel');
friendsManager.addFriend(true);
friendsManager.friends.push('emmaneul');
friendsManager.friends.push(42);
```

Explain how the code is not consistent or predictable, then provide an example in code that uses closure to make it more consistent and predictable.

### Response 2

The friendsManager object is not consistent or predictable because the friends array is publicly accessible, allowing external code to push invalid values; this bypasses the validation logic in the addFriend method. The addFriend method only prevents non-string values from being added when using the method, but the friends array can still be modified directly. Anyone can modify friends directly, leading to potential unintended behavior.  
To fix these issues, we can use a closure to encapsulate the friends array, making it private and only allowing modifications through controlled methods:

```js
const createFriendsManager = () => {
    let friends = [];

    return {
        addFriend(newFriend) {
            if (typeof newFriend !== 'string') {
                console.log('Invalid input: Friend name must be a string.');
                return;
            }
            friends.push(newFriend);
        },
        getFriends() {
            return [...friends];
        },
    };
};

const friendsManager = createFriendsManager();

friendsManager.addFriend('Daniel');
friendsManager.addFriend(true);
console.log(friendsManager.getFriends());

friendsManager.friends = ['Hacker'];
console.log(friendsManager.getFriends());
```

## Prompt 3 - Braily

With OOP in JavaScript, it's possible to use factory functions to achieve encapsulation and re-use them to make objects that look alike. However, factory functions have drawbacks and we often use classes instead.

How would you explain to a budding developer what the drawbacks of using factory functions are and why it is better to use classes instead?

### Response 3

The drawbacks of factory functions are:

#### 1. Memory Inefficiency

-   Every time that you create an object with a factory function, methods get duplicated in memory for each object.
-   Example:

```js
const createPerson = (name, city) => {
    // Returns an anonymous object with the properties and methods
    return {
        name,
        greet() {
            console.log(`Hey there, I'm ${name} based in ${city}.`);
        },
    };
};

// Every time we invoke the function, we are storing the methods in memory every time, leading to duplicates.
const person1 = createPerson('Max', 'Los Angeles');
const person2 = createPerson('Thomas', 'New York');
```

#### 2. Lack of Prototypes (Slower Performance)

-   Factory functions don't use the prototype system efficiently while classes allow methods to be shared via prototypes which improves performance.

#### 3. No `instanceof` Check

-   Objects created with factory functions aren't instance of anything
-   For example:

```js
console.log(person1 instanceof createPerson); // false
```

-   With classes, you can check the instance:

```js
class Person {
    constructor(person, city) {
        this.person = person;
        this.city = city;
    }

    greet() {
        console.log(`Hey there, I'm ${this.name} based in ${this.city}.`);
    }
}

const person1 = createPerson('Max', 'Los Angeles');

console.log(person1 instanceof Person); // true
```

#### 4. Less readable and Standardized

-   The `class` syntax is more common and easier to read.
-   Example:

```js
class Car {
    constructor(make, model, year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }

    sayMessage() {
        console.log(
            `Vroom Vroom! I'm a ${this.year} ${this.make} ${this.model}`
        );
    }
}
const camry = new Car('Toyota', 'Camry', 2025);
camry.sayMessage(); // Vroom Vroom! I'm a 2025 Toyota Camry
```

## Prompt 4 - Suru

Do some research on the history of when / how classes were introduced into JavaScript and share your findings. Your response should include:

-   What version of JavaScript were classes introduced in and when did it come out?
-   Why were classes introduced into JavaScript?

### Response 4

JavaScript classes were introduced in ECMAScript 6 (ES6), which was released in 2015, to provide a cleaner and more structured approach to object-oriented programming. Before ES6, JavaScript relied on constructor functions and prototypes, which could be confusing and less intuitive. The introduction of classes improved readability and maintainability by offering a more organized syntax similar to languages like Java and Python. It also simplified inheritance with the extends keyword, making it easier to create subclasses without manually modifying prototype chains. Additionally, classes helped contain logic by automatically adding methods to the prototype, optimizing memory usage. While JavaScript classes did not change the prototype-based nature of the language, they made object-oriented programming more accessible and easier to understand, benefiting developers transitioning from other programming languages.

## Prompt 5

OOP can still be achieved in JavaScript without using the `class` keyword and instead using the "Constructor Functions" and the "Prototype Chain" (look them up!)

```js
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function () {
    return `Hi, I'm ${this.name}, and I'm ${this.age} years old.`;
};

const alice = new Person('Alice', 30);
console.log(alice.greet());
```

Provide one point that advocates for the use of this syntax and then provide a counter-argument for the use of classes instead.

### Response 5
