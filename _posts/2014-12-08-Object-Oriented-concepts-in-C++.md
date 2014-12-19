---
layout: post_page
title: "Object Oriented concepts in C++"
date: 2014-12-08 17:30:24
categories: Programming, C++
---

This post explains a few things I wish I'd found someone to tell me before mostly naming conventions of OOP stuff in C++ which is surprisingly different than it is in other languages. This post also assumes you're familiar with what functions, classes, inheritance, objects and so forth are. If you're taking INF 332 this would be good to read while studying for your exam. You can get a huge and more detailed version on [Wikipedia].


So we'll define a few things we need:

**Data member**: This is basically an attribute. A variable accessible from within the class or global within the class.  
**Member function**: A function that is within a class. Known as methods in other langauges.  
**Getter**: Also called accessor. A function used to access or get the value held by a private data member. These variable names often start with get  
**Setter**: Also called Modifier. A function used to modify or set the value held by a private data member.  These variable names often start with set.  
**Struct vs Class**: There are two ways to define a class in C++. Using class and using struct. When using class all that is within the braces is private by default unless otherwise specified. When using struct all that is within the braces is public unless otherwise specified.

It is good practice to have everything be private first and set to public what needs to be as one moves on instead of vice versa. This is why on many projects one will find class used instead of struct.

**Base class**: A base class is a class that is created with the intention of deriving other classes from it.
**Child class**: A child class is a class that was derived from another, that will now be the parent class to it.
**Parent class**: A parent class is the closest class that we derived from to create the one we are referencing as the child class. The one right above in the inheritance heirachy.

### Access Labels

These are private, public and protected. They are used within classes to set access permissions for the members in that section of the class. They really aren't very interesting so I won't spend too much time on them. You can also specify them when carrying out inheritance.

**public**: This label indicates any members within the 'public' section can be accessed freely anywhere a declared object is in scope.

**private**: Members defined as private are only accessible within the class defining them, or friend classes. Usually the domain of member variables and helper functions. It's often useful to begin putting functions here and then moving them to the higher access levels as needed so to reduce complexity.

**protected**: The protected label has a special meaning to inheritance, protected members are accessible in the class that defines them and in classes that inherit from that base class, or friends of it.

There's a [table] on wikipedia explaining how these access labels may change in the derived class when inheritance is done.  Also explains inheritance quite a bit.

### Constructors

A constructor is a special member function that is called *whenever* a new instance of a class is created. The compiler calls the  constructor after the new object has been allocated in memory, and converts that "raw" memory into a proper, typed object.

A constructor is used to assign values to the data members that the creator of the class choses. If you don't declare a constructor the compiler will impicitly make one for you.
y
The constructor is declared much like a normal member function but it will share the name of the class and it has no return value.

The constructor may or may not have arguments. A constructor without arguments is called a `default constructor`, a constructor that takes arguments are `non-default constructors`.

**Overloading**: The ability to create multiple methods of the same name with different implementations. Calls to an overloaded function will run a specific implementation of that function appropriate to the context of the call, allowing one function call to perform different tasks depending on context. [wikipedia]

So let's rewrite this like people with brains combining constructors and overloading. This way we have the choice of different interfaces to our class.

    class employee {
        public:
            employee () {
                // employee constructor code should be put here.
                // objects for this constructor have the structure
                // employee objectName;
            }

            employee (string name, int employeeNUmber) {
                // employee constructor code should be put here.
                // objects for this constructor have the structure
                // employee objectName (string, int);
           }

           employee (int employeeNumber, string name){
                // employee constructor code should be put here.
                // objects for this constructor have the structure
                // employee objectName (int, string)
           }
    };

Notice the constructor has the same name as the class. It returns nothing.



#### Constructor initalization lists

We save by not having to do an assignment, the compiler knows to construct the object with that value in memory

Also called *member initialization lists*. They are used to initialize data members and base classes with non-default constructor.

The difference between initialization list construction and custom constructor thru assignment is run-time speed. If you have a class with a few large member variables, assignment construction can create a lot of extra overhead


Constructors are used to assign values to data members however the initialization isn't done within the body of constructors; such kind of initialization would actually be *assignment*. Keep in mind that data members are initialized in the order they are declared, not the order they appear in the initializer list therefore as good practice add the members to the initializer list in the same order they're declared.

To quote the wikibook on C++ classes under constructor initialization lists: *The C++ standard defines that all initialization of data members are done before entering the body of constructors. This is the reason why certain types (const types and references) cannot be assigned to and must be initialized in the constructor initialization list.*

Note: *we didn't assign; instead the values seem like paramters being passed to data members.*

    class myClass {
    //Much thanks to crazypyro on ##programming freenode irc
    public:
        // A default crazypro argument.
        myClass() : my_name( "crazypyro" ) { }
        myClass(std::string name) : my_name( name ) {}
        std::string get_name() {return my_name;};
 
    private:
        std::string my_name;
    };

We've overloaded the constructor but used constructor initialization lists instead of assigning values.



### Destructors

Destructors like the Constructors are declared as any normal member functions but will share the same name as the Class, what distinguishes them is that the Destructor's name is preceded with a "~", it can not have arguments and can't be overloaded. Destructors are called whenever an Object of the Class is destroyed. Destructors are crucial in avoiding resource leaks (by deallocating memory) and in implementing the RAII idiom. The Destructor is invoked when Objects are destroyed.

It therefore called AFTER an object has been destroyed to restore the system to a desired state.

from [Wikipedia]

    class toBeDestroyed {
        // Much class code here

        //Can't have arguments and can't be overloaded.
        //Overloading meaning we can't create a similarly named member function with different implementation.
        ~toBeDestroyed () {
            // All your destruction stuff here like deallocating memory.
        }
    };




### Static

Member functions or variables declared **static** are shared between all instances of an object type. Meaning that only one copy of the member function or variable does exists for any object type.

    class staticExample {

    static int id_number;

    public:
        staticExample (int id) id_number(id) {
            // Constructor code goes here.
        }

        int get_id() {
            return id_number;
        }
    };

    int main () {
        staticExample me (1232);
        staticExample you (9854);
        cout << me.get_id << endl;
        //Output will be 9854. Therefore me and you share same id_number variable.
        // We are pointing to the same memory location.
        return 0;
    }




### Composition

A composition is a class which contains as it's data memebers another class

    class exampleClass {
        // Class stuff goes here.
    };

    class composition {
        exampleClass exampleObject;
    };

Those two are different classes.  

### Inheritance

So inheritance is a rather huge topic for me to explain here. It's best you read it elsewhere. I shall just explain how to do inheritance in C++. Multiple inheritance to be specific.

In the below case we shall have 3 classes two base classes baseClassA and baseClassB; one child class that will inherit from both classes childClass. We shall see the use of constructor initialization lists with multiple inheritance and regarding constructors of parent classes.

    class baseClassA {
        int aInt;
        string aString;
    public:
        baseClass (int aIn, string aStr): aInt(aIn), aString(aStr) {
            //Put constructor code here
        }

    // more class code e.g member functions.

    };

    class baseClassB {
        int bInt;
        string bString

    public:
        baseClassB (int bin, bStr): bInt(bin), bString(bStr) {
            //Put constructor code here
        }
    };

    class childClass: public baseClassA, public baseClassB {
        int cInt;
        string cString
    
    public:
    childClass(int cIn, string cStr, int aIn, string aStr, int bIn, string bStr ):
    cInt(cIn), cString(cStr), baseClassA(aIn, aStr), baseClassB (bIn, bStr) {
            // Constructor code
        }
    };

    

### Dynamic Polymorphism

It would do you better to read this [Dynamic polymorphism] but since I already read it I'll just give you the part that I feel is most relevant.

Suppose that we have two classes, A and B. B derives from A and redefines the implementation of a method c() that resides in class A. Now suppose that we have an object b of class B. How should the instruction b.c() be interpreted?

If b is declared in the stack (not declared as a pointer or a reference) the compiler applies **static binding**, this means it interprets (at compile time) that we refer to the implementation of c() that resides in B.
y
However, if we declare b as a pointer or a reference of class A, the compiler could not know which method to call at compile time, because b can be of type A or B. If this is resolved at run time, the method that resides in B will be called. This is called **dynamic binding**. If this is resolved at compile time, the method that resides in A will be called. This is again, **static binding**.

    
    
    
### Virtual Member Funtions  

Virtual member functions are member functions, that can be overridden in any class derived from the one where they were declared. Sort of like you can "overwrite" the funtion in the derived class. This is done by placing the keyword virtual before the method declaration. Like `virtual memberFunc () { /* Member function code*/}`
The point is that when the compiler has to decide between applying static binding or dynamic binding it will apply dynamic binding. Otherwise, static binding will be applied.
If the base class function is virtual all subclass overrides of it will also be virtual. However it is still good practice to add the virtual keyword before function definitions in subclasses, clarity and all.

In this case assume we were simulating marketing companies in the world so. The new one will redefine marketing. (There's a mutifaceted joke here btw :'-D )

    class oldCompany {
    public:
        virtual void marketing () {
            //How old company does marketing
        }
    };

    class newCompany: public oldCompany {
    public:
        virtual void marketing () {
            //How new company does marketing. This is different than the one in the parent class.
            //It is redefininig marketing. :D
        }
    };

    
    
    
#### Pure Virtual Member Functions  

Sometimes we don't want to provide an implementation of our function at all, but want to **force** people sub-classing our class to provide an implementation on their own.

To write a pure virtual function do not try to write the function code however just add '= 0' after function declaration.

    class allCarsInGame {
    public:
        virtual void colour = 0;
        //More class stuff
    };


    class policeCars : public allCarsInGame {
    public:
        virtual string getColour () {
            return "blue";
        }
    };


This way anyone deriving from virtualStuff will have to implement/write stuffDerivedDoes or their code won't compile. In this case all cars have colour but we can't set that in the parent car class but we want to make sure whoever implements the derived class does this.

[Wikipedia]: https://en.m.wikipedia.org/wiki/Function_overloading
[Dynamic polymorphism]: https://en.wikibooks.org/wiki/C%2B%2B_Programming/Classes#Dynamic_polymorphism_.28Overrides.29
[table]: https://en.wikibooks.org/wiki/C%2B%2B_Programming/Classes#Inheritance_.28Derivation.29
