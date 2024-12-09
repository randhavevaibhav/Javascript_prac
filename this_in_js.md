# This keyword
 
### Definition - 
 
`this` keyword refers to the object it belongs to.

### Different secanrios for this keyword usage - 

1. In a method, this refers to the owner object.

    ```javascript
    var person = {
    firstName: "John",
    lastName : "Doe",
    id : 5566,
    fullName : function() {
        return this.firstName + " " + this.lastName;
    }
    };
    person.fullName(); // John Doe
    ```

2. Used alone, this refers to the global object.

    ```javascript
    var x = this;
    console.log(this); // [object Window]
    ```
3. In a function, this refers to the global object.

    ```javascript
    function myFunction() {
    cnosole.log(this);
    }
    myFunction(); //[object Window]
    ```
4. In a function, but in `strict mode`, `this` is `undefined`.

    ```javascript
    "use strict";
    function myFunction() {
    cnosole.log(this);
    }
    myFunction(); // undefined
    ```
5. In an `event`, `this` refers to the `element` that received the event.

    ```html
    <button onclick="this.style.display='none'">
    Click to Remove Me!
    </button>
    <!-- When click on the button, the button will disappear since its display property is set to "none"; -->
    ```
6. Methods like `call()`, and `apply()` can refer this to `any` `object`.
    
    * In the example below of `call()`, when calling person1.fullName with person2 as argument, this will refer to person2, even if it is a method of person1:

        ```javascript
        var person1 = {
        fullName: function() {
            return this.firstName + " " + this.lastName;
            }
        }
        var person2 = {
            firstName:"John",
            lastName: "Doe",
        }
        person1.fullName.call(person2);  // Will return "John Doe"
        ```
    * In the example below of `apply()`, the effect is the same.

        ```javascript
            var person1 = {
            fullName: function() {
                return this.firstName + " " + this.lastName;
                }
            }
            var person2 = {
                firstName:"John",
                lastName: "Doe",
            }
            console.log(person1.fullName.apply(person2));  // John Doe
        ```
    * `IMP Note -` You `can't` change `binding` of `this` in `arrow` function with `call` `bind` or `apply`.

        ```javascript
        var person1 = {
                firstName:"sam",
                lastName:"sun",
                fullName: ()=> {
                    console.log( this.firstName + " " + this.lastName);
                    }
        }
        //normally calling function inside object
        person1.fullName(); //TypeError: Cannot read properties of undefined (reading 'firstName')
        // calling function inside object with call,apply or bind
        const func = person1.fullName;
        func.call(person1);//TypeError: Cannot read properties of undefined (reading 'firstName')
        // you will get same error with apply and bind

        //Even if you called it on other object you will get the same error.
  
        var person2 = {
                firstName:"spider",
                lastName:"man"
            }
        const func = person1.displayname;
        func.call(person2) //TypeError: Cannot read properties of undefined (reading 'firstName')
        ```
    * If you want to you can return that arrow function from a regular      function then your code will work fine !

        ```javascript
        var person1 = {
            firstName:"sam",
            lastName:"sun",
            displayname(){
                return ()=> {
                console.log( this.firstName + " " + this.lastName);
                }
            }
         }

         person1.displayname()(); // sam sun  
        ```

### Value of `this` in a class

#### Different secanrios for this keyword usage in class
* #### value of this in a static methods in a class
    In static methods of class `this` always refers to the `class` `itself` whether it
    is an `arrow` function or a `regular` function.

    ```javascript
    class Person{

        constructor(firstName,lastName)
        {
            this.firstName = firstName;
            this.lastName = lastName;
        }
        
        //In static methods this refers to the class itself
        static  displayFirstName()
        {
            console.log(`${this}`) // gives class definition
        }
        static  displayLastName=()=>
        {
            console.log(`${this}`) // gives class definition
        }
        

        }
    Person.displayFirstName(); //class Person
    ```
    Hence if we try to acess `instance` properties such as `${this.firstName}` inside `static` methods (arrow or regular func) we will get `undefine`.
* #### value of `this` in a `regualr` and `arrow` methods in a class
    In case of regular and arrow methods in class value of `this` refers to the class
    `instance` i.e., class object.

    ```javascript
    class Person{

        constructor(firstName,lastName)
        {
            this.firstName = firstName;
            this.lastName = lastName;
        }
        
        //In regular methods this refers to the class intance
         displayFirstName()
        {
            console.log(`${this.firstName}`) 
        }
        //In regular methods this refers to the class intance
        displayLastName=()=>
        {
            console.log(`${this.lastName}`) 
        }
        

        }
    const person1 = new Person("sam","son");
    person1.displayFirstName() //sam
    person1.displayLastName() //son
    ```
### Binding in Js classes with this keyword
* #### Inexplicit binding

    Let’s explictly bind the display function to the object.

    ```javascript
        class Person {
            constructor() {
                this.firstName = "John";
                this.lastName = "Doe";
            }
            display() {
                return this.firstName + this.lastName;
            }
        }
        const person = new Person();
        person.display(); // "JoneDoe"
        const display = person.display;
        display(); // Error: this is undefined
    ```
    When directly invoking the `person.display` method, `this` keyword is `inexplicted bind` to the `person` object.
    But `changing` the `context` to the `global environment` by using `const display = person.display` , this keyword becomes undefined (Since the bodies of class declarations and class expressions are executed in strict mode, which makes this keyword in global regular function becomes undefined.)
* #### Explicit binding
    Let’s explictly bind the display function to the object.

    ```javascript
    class Person {
        constructor() {
            this.firstName = "John";
            this.lastName = "Doe";
        }
        display() {
            return this.firstName + this.lastName;
        }
    }
    const person = new Person();
    const display = person.display;
    display.bind(person);
    display(); // "JohnDoe"
    ```
    Now even though we assign the `person.display` to `globally` defined variable, `this` keyword is still `binded` to the `person` object.In the last part, we discuss the difference between `arrow function` and `regular function`. Let’s see what’s the difference between them in terms of binding.

### Execption with arrow function
    
* With arrow function, this keyword is binded to the context of class where it is defined, so there is no need to explictly bind this keyword.

    ```javascript
        class Person {
        constructor() {
            this.firstName = "John";
            this.lastName = "Doe";
        }
        display = () {
            return this.firstName + this.lastName;
        }
        }
        const person = new Person();
        const display = person.display
        display(); // "JohnDoe"
    ```
    Even though `person.display` is `assigned` to a variable defined in `global context`, its `context` `stays the same` as in person object.    


   



### Discrepancy of this keyword for Regular function and Arrow function -
* With arrow functions there are no binding of this. this keyword only represents the object that defined the arrow function.

    ```javascript
    var person = {
    firstName: "John",
    lastName: "Doe",
    myName: () => {
        return this.firstName + this.lastName; // undefined + undefined
    }
    }
    person.myName();
    ```
* In regular functions the this keyword represents the object that called the function, which could be the window, the document, a button etc.

    ```javascript
    var person = {
    firstName: "John",
    lastName: "Doe",
    myName: function() {
        return this.firstName + this.lastName; // "John" + "Doe"
    }
    }
    person.myName();
    ```
>In a word, in `regular function` the context of `this` keyword will change according to the context where it is `called`, while in `arrow function` the context is bound to the `origial context` of definition.

