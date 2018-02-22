# This meets arrow

## Getting Started

1. Create a new project
2. Create a script.js file
3. Make sure you have node installed -- You'll be using this to run the JavaScript file and read the logs in the terminal

```
scripts.js
README.md
```

### Objectives

In this lab assignment we are going to walk through some key concepts to understand what happens when we use the *this* keyword in your programs and how *this* and the arrow function work together. 

*This* is a complicated concept to understand so take your time completing the two phases and allow youself to think about what is really happpening and read over the descriptions. Feel free to move things around or add to the lab. The most important thing to take away is to have a better understanding of *this*. 

#### PHASE ONE 

Your first task is to create an ES6 Person class with a constructor method and a printPlacesLived method. 

The constructor should assign a value to this.name and array of cities to this.cities. 

The printPlacesLived method should loop through each city stored in this.cities and print the name and places lived to the terminal. 

I'll be using Covalence as the "person" for the example but feel free to make yours more personal. 

```javascript
class Person {
    constructor() {
        this.name = 'Covalence';
        this.cities = ['Nashville', 'Bermingham'];

    };

    printPlacesLived() {
        this.cities.forEach(function (city) {
            console.log(`${this.name} has lived at ${city}`);
        });
    }
};

let person = new Person();
person.printPlacesLived();
```

Yep. That's it. In the terminal type: **node scripts.js**

You should now see the following output:

```
C:\Users\Nicot\Documents\Covalence\Labs\this-arrow\scripts.js:10
            console.log(`${this.name} has lived at ${city}`);                                ^

TypeError: Cannot read property 'name' of undefined
```
This about what happend and what may have caused 'name' to be undefined....... 


Ok now that you had some time to think or play around with the code lets begin to understand what caused the error by adding two console.logs in the printPlacesLived method. 

Your printPlacesLived method should now like this:

```javascript
    printPlacesLived() {
        console.log('this is:', this); //Person object
        this.cities.forEach(function(city) {
            console.log('this is:', this); //undefined  
            console.log(`${this.name} has lived at ${city}`);
        });
    }
```
In the terminal type: **node scripts.js**

Notice that the first time we log *this* the Person class is printed but when we log *this* inside of the forEach function, *this* is undefined and so is 'name'.

Lets fix this in Phase 2.

### PHASE TWO

Change your ES5 forEach function to an ES6 arrow function. 

Your scripts.js file should now look like this: 

```javascript
    printPlacesLived() {
        console.log('this is:', this); //Person object
        this.cities.forEach((city) => {
            console.log('this is:', this); //undefined  
            console.log(`${this.name} has lived at ${city}`);
        });
    }
```

In the terminal type: **node scripts.js**

You will now notice that everytime *this* is logged we see the Person class printed and most importantly that *this* is no longer undefined inside of the forEach function and 'name' is also no longer undefined. 

``` 
this is: Person { name: 'Covalence', cities: [ 'Nashville', 'Bermingham' ] }this is: Person { name: 'Covalence', cities: [ 'Nashville', 'Bermingham' ] }
Covalence has lived at Nashville
this is: Person { name: 'Covalence', cities: [ 'Nashville', 'Bermingham' ] }
Covalence has lived at Bermingham
```
Cool stuff but why does this work right?

## HERES WHY

#### Let’s recap what we did: ****
We first created an instance of the Person class using the keyword new. 
The new keyword bound *this* to reference the newly constructed object, person. 
Then we called the printPlacesLived method on the person object, still maintaining the value of *this*. 
Inside the printPlacesLived method we invoked the forEach function by using the object this.cities, resulting in losing the reference for *this*.
So when we tried to access this.name it returned an error because the forEach function was invoked by this.cities. 

#### How did the arrow function fix this? ****
The arrow function circumvents the rules of *this* specified for ES5 and the reference for *this* is no longer based on how the function is called and is instead based on the functions surrounding context. 
So when we accessed this.name this time, the reference for *this* was not lost because the enclosed scope of the function included the Person class and the this.name object. 

## GUIDELINE TO FOLLOW WHEN WORKING WITH THIS

### Questions to ask when determining this:
*In order of precedence.*

>#### For Regular Functions:
>```
>1. If the new keyword is used then *this* will reference the newly created object.
>let person = new Person();
>2. If call or apply is used then *this* will reference the explicitly specified object.
>let person = Person.call(obj);
>3. If a function is called on a context object then *this* is that context object.
>obj.printPlacesLived();
>4. Otherwise, *this* will reference the global object or if in strict mode, will reference undefined.
>let person = Person();
>```

>#### For Arrow Functions: 
>```
>1. The reference for *this* will be based on the context of the enclosing lexical scope.
>```
