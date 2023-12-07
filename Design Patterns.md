  

Creational Design Patterns

==========================

  

In plain words

> Creational patterns are focused towards how to instantiate an object or group of related objects.

  

Wikipedia says

> In software engineering, creational design patterns are design patterns that deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. The basic form of object creation could result in design problems or added complexity to the design. Creational design patterns solve this problem by somehow controlling this object creation.

  

* [Simple Factory](https://github.com/sunilsoni/interview-notes/blob/main/design-pattern/creational-design-pattern.md#-simple-factory)

* [Factory Method](https://github.com/sunilsoni/interview-notes/blob/main/design-pattern/creational-design-pattern.md#-factory-method)

* [Abstract Factory](https://github.com/sunilsoni/interview-notes/blob/main/design-pattern/creational-design-pattern.md#-abstract-factory)

* [Builder](https://github.com/sunilsoni/interview-notes/blob/main/design-pattern/creational-design-pattern.md#-builder)

* [Prototype](https://github.com/sunilsoni/interview-notes/blob/main/design-pattern/creational-design-pattern.md#-prototype)

* [Singleton](https://github.com/sunilsoni/interview-notes/blob/main/design-pattern/creational-design-pattern.md#-singleton)

  
  

🏠 Simple Factory

--------------

Real world example

> Consider, you are building a house and you need doors. You can either put on your carpenter clothes, bring some wood, glue, nails and all the tools required to build the door and start building it in your house or you can simply call the factory and get the built door delivered to you so that you don't need to learn anything about the door making or to deal with the mess that comes with making it.

  

In plain words

> Simple factory simply generates an instance for client without exposing any instantiation logic to the client

  

Wikipedia says

> In object-oriented programming (OOP), a factory is an object for creating other objects – formally a factory is a function or method that returns objects of a varying prototype or class from some method call, which is assumed to be "new".

  

**Programmatic Example**

  

First of all we have a door interface and the implementation

```java

interface Door

{

  public float getWidth();

  public float getHeight();

}

  

class WoodenDoor implements Door

{

  protected float width;

  protected float height;

  

  public WoodenDoor(float width, float height)

  {

    this.width = width;

    this.height = height;

  }

  

  public float getWidth()

  {

    return width;

  }

  

  public float getHeight()

  {

    return height;

  }

}

```

Then we have our door factory that makes the door and returns it

```java

class DoorFactory

{

  public static Door makeDoor(float width, float height)

  {

    return new WoodenDoor(width, height);

  }

}

```

And then it can be used as

```java

public class SimpleFactoryPattern {

  public static void main(String[] args) {

    Door door = DoorFactory.makeDoor(100, 200);

    System.out.println("Width: " + door.getWidth());

    System.out.println("Height: " + door.getHeight());

  }

}

```

  

**When to Use?**

  

When creating an object is not just a few assignments and involves some logic, it makes sense to put it in a dedicated factory instead of repeating the same code everywhere.

  

🏭 Factory Method

--------------

  

Real world example

> Consider the case of a hiring manager. It is impossible for one person to interview for each of the positions. Based on the job opening, she has to decide and delegate the interview steps to different people.

  

In plain words

> It provides a way to delegate the instantiation logic to child classes.

  

Wikipedia says

> In class-based programming, the factory method pattern is a creational pattern that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created. This is done by creating objects by calling a factory method—either specified in an interface and implemented by child classes, or implemented in a base class and optionally overridden by derived classes—rather than by calling a constructor.

  

**Programmatic Example**

  

Taking our hiring manager example above. First of all we have an interviewer interface and some implementations for it

  

```java

interface Interviewer

{

  public void askQuestions();

}

  

class Developer implements Interviewer

{

  public void askQuestions()

  {

    System.out.println("Asking about design patterns!");

  }

}

  

class CommunityExecutive implements Interviewer

{

  public void askQuestions()

  {

    System.out.println("Asking about community building");

  }

}

```

  

Now let us create our `HiringManager`

  

```java

abstract class HiringManager

{

  

  // Factory method

  abstract protected Interviewer makeInterviewer();

  

  public void takeInterview()

  {

    Interviewer interviewer = this.makeInterviewer();

    interviewer.askQuestions();

  }

}

  

```

Now any child can extend it and provide the required interviewer

```java

class DevelopmentManager extends HiringManager

{

  protected Interviewer makeInterviewer()

  {

    return new Developer();

  }

}

  

class MarketingManager extends HiringManager

{

  protected Interviewer makeInterviewer()

  {

    return new CommunityExecutive();

  }

}

```

and then it can be used as

  

```java

public class AbstractFactoryPattern {

  public static void main(String[] args) {

    HiringManager devManager = new DevelopmentManager();

    devManager.takeInterview(); // Output: Asking about design patterns

  

    HiringManager marketingManager = new MarketingManager();

    marketingManager.takeInterview(); // Output: Asking about community building.

  }

}

```

  

**When to use?**

  

Useful when there is some generic processing in a class but the required sub-class is dynamically decided at runtime. Or putting it in other words, when the client doesn't know what exact sub-class it might need.

  

🔨 Abstract Factory

----------------

  

Real world example

> Extending our door example from Simple Factory. Based on your needs you might get a wooden door from a wooden door shop, iron door from an iron shop or a PVC door from the relevant shop. Plus you might need a guy with different kind of specialities to fit the door, for example a carpenter for wooden door, welder for iron door etc. As you can see there is a dependency between the doors now, wooden door needs carpenter, iron door needs a welder etc.

  

In plain words

> A factory of factories; a factory that groups the individual but related/dependent factories together without specifying their concrete classes.

  

Wikipedia says

> The abstract factory pattern provides a way to encapsulate a group of individual factories that have a common theme without specifying their concrete classes

  
  

**Programmatic Example**

  

Translating the door example above. First of all we have our `Door` interface and some implementation for it

  

```java

interface Door

{

  public void getDescription();

}

  

class WoodenDoor implements Door

{

  public void getDescription()

  {

    System.out.println("I am a wooden door");

  }

}

  

class IronDoor implements Door

{

  public void getDescription()

  {

    System.out.println("I am an iron door");

  }

}

```

Then we have some fitting experts for each door type

  

```java

interface DoorFittingExpert

{

  public void getDescription();

}

  

class Welder implements DoorFittingExpert

{

  public void getDescription()

  {

    System.out.println("I can only fit iron doors");

  }

}

  

class Carpenter implements DoorFittingExpert

{

  public void getDescription()

  {

    System.out.println("I can only fit wooden doors");

  }

}

```

  

Now we have our abstract factory that would let us make family of related objects i.e. wooden door factory would create a wooden door and wooden door fitting expert and iron door factory would create an iron door and iron door fitting expert

```java

interface DoorFactory

{

  public Door makeDoor();

  public DoorFittingExpert makeFittingExpert();

}

  

// Wooden factory to return carpenter and wooden door

class WoodenDoorFactory implements DoorFactory

{

  public Door makeDoor()

  {

    return new WoodenDoor();

  }

  

  public DoorFittingExpert makeFittingExpert()

  {

    return new Carpenter();

  }

}

  

// Iron door factory to get iron door and the relevant fitting expert

class IronDoorFactory implements DoorFactory

{

  public Door makeDoor()

  {

    return new IronDoor();

  }

  

  public DoorFittingExpert makeFittingExpert()

  {

    return new Welder();

  }

}

```

And then it can be used as

```java

public class AbstractFactoryPattern {

  public static void main(String[] args) {

    DoorFactory woodenFactory = new WoodenDoorFactory();

  

    Door door = woodenFactory.makeDoor();

    DoorFittingExpert expert = woodenFactory.makeFittingExpert();

  

    door.getDescription();  // Output: I am a wooden door

    expert.getDescription(); // Output: I can only fit wooden doors

  

    // Same for Iron Factory

    DoorFactory ironFactory = new IronDoorFactory();

  

    door = ironFactory.makeDoor();

    expert = ironFactory.makeFittingExpert();

  }

}

```

  

As you can see the wooden door factory has encapsulated the `carpenter` and the `wooden door` also iron door factory has encapsulated the `iron door` and `welder`. And thus it had helped us make sure that for each of the created door, we do not get a wrong fitting expert.

  

**When to use?**

  

When there are interrelated dependencies with not-that-simple creation logic involved

  

👷 Builder

--------------------------------------------

Real world example

> Imagine you are at Hardee's and you order a specific deal, lets say, "Big Hardee" and they hand it over to you without *any questions*; this is the example of simple factory. But there are cases when the creation logic might involve more steps. For example you want a customized Subway deal, you have several options in how your burger is made e.g what bread do you want? what types of sauces would you like? What cheese would you want? etc. In such cases builder pattern comes to the rescue.

  

In plain words

> Allows you to create different flavors of an object while avoiding constructor pollution. Useful when there could be several flavors of an object. Or when there are a lot of steps involved in creation of an object.

  

Wikipedia says

> The builder pattern is an object creation software design pattern with the intentions of finding a solution to the telescoping constructor anti-pattern.

  

Having said that let me add a bit about what telescoping constructor anti-pattern is. At one point or the other we have all seen a constructor like below:

  

```java

public Burger(size, cheese, pepperoni, tomato, lettuce)

        {

        }

```

  

As you can see; the number of constructor parameters can quickly get out of hand and it might become difficult to understand the arrangement of parameters. Plus this parameter list could keep on growing if you would want to add more options in future. This is called telescoping constructor anti-pattern.

  

**Programmatic Example**

  

The sane alternative is to use the builder pattern. First of all we have our burger that we want to make

  

```java

class Burger

{

  protected int size;

  

  protected boolean cheese = false;

  protected boolean pepperoni = false;

  protected boolean lettuce = false;

  protected boolean tomato = false;

  

  public Burger(BurgerBuilder builder)

  {

    this.size = builder.size;

    this.cheese = builder.cheese;

    this.pepperoni = builder.pepperoni;

    this.lettuce = builder.lettuce;

    this.tomato = builder.tomato;

  }

}

```

  

And then we have the builder

  

```java

class BurgerBuilder

{

  public int size;

  

  public boolean cheese = false;

  public boolean pepperoni = false;

  public boolean lettuce = false;

  public boolean tomato = false;

  

  public BurgerBuilder(int size)

  {

    this.size = size;

  }

  

  public BurgerBuilder addPepperoni()

  {

    this.pepperoni = true;

    return this;

  }

  

  public BurgerBuilder addLettuce()

  {

    this.lettuce = true;

    return this;

  }

  

  public BurgerBuilder addCheese()

  {

    this.cheese = true;

    return this;

  }

  

  public BurgerBuilder addTomato()

  {

    this.tomato = true;

    return this;

  }

  

  public Burger build()

  {

    return new Burger(this);

  }

}

```

And then it can be used as:

  

```java

public class BuilderPattern {

  public static void main(String[] args) {

    Burger burger = (new BurgerBuilder(14))

            .addPepperoni()

            .addLettuce()

            .addTomato()

            .build();

  }

}

```

  

**When to use?**

  

When there could be several flavors of an object and to avoid the constructor telescoping. The key difference from the factory pattern is that; factory pattern is to be used when the creation is a one step process while builder pattern is to be used when the creation is a multi step process.

  

🐑 Prototype

------------

Real world example

> Remember dolly? The sheep that was cloned! Lets not get into the details but the key point here is that it is all about cloning

  

In plain words

> Create object based on an existing object through cloning.

  

Wikipedia says

> The prototype pattern is a creational design pattern in software development. It is used when the type of objects to create is determined by a prototypical instance, which is cloned to produce new objects.

  

In short, it allows you to create a copy of an existing object and modify it to your needs, instead of going through the trouble of creating an object from scratch and setting it up.

  

**Programmatic Example**

  

In PHP, it can be easily done using `clone`

  

```java

class Sheep implements Cloneable

{

  protected String name;

  protected String category;

  

  public Sheep(String name, String category)

  {

    this.name = name;

    this.category = category;

  }

  

  public Sheep(String name)

  {

    this.name = name;

    this.category = "Mountain Sheep";

  }

  

  public void setName(String name)

  {

    this.name = name;

  }

  

  public String getName()

  {

    return this.name;

  }

  

  public void setCategory(String category)

  {

    this.category = category;

  }

  

  public String getCategory()

  {

    return this.category;

  }

  

  public Object clone() throws CloneNotSupportedException

  {

    return super.clone();

  }

}

```

Then it can be cloned like below

```java

public class PrototypePattern {

  public static void main(String[] args) {

    Sheep original = new Sheep("Jolly");

    System.out.println(original.getName()); // Jolly

    System.out.println(original.getCategory()); // Mountain Sheep

  

    // Clone and modify what is required

    Sheep cloned;

    try {

      cloned = (Sheep) original.clone();

      cloned.setName("Dolly");

      System.out.println(cloned.getName()); // Dolly

      System.out.println(cloned.getCategory()); // Mountain sheep

    } catch (CloneNotSupportedException e) {

      // TODO Auto-generated catch block

      e.printStackTrace();

    }

  }

}

```

  

Also you could use the magic method `__clone` to modify the cloning behavior.

  

**When to use?**

  

When an object is required that is similar to existing object or when the creation would be expensive as compared to cloning.

  

💍 Singleton

------------

  

A singleton is a class that can be instantiated only one time in a JVM per class loader. Repeated calls always

return the same instance. Ensures that a class has only one instance, and provide a global point of access. It can be an issue if singleton class gets loaded by multiple class loaders or JVMs.

  

```java

public class OnlyOne {

    private static OnlyOne one = new OnlyOne();

    // private constructor. This class cannot be instantiated from outside and // prevents subclassing.

    private OnlyOne(){}

    public static OnlyOne getInstance() {

        return one;

    }

}

```

To use it:

  

```java

//No matter how many times you call, you get the same instance of the object.

 OnlyOne myOne = OnlyOne.getInstance();

  

```

  

The constructor must be explicitly declared and should have the private access modifier, so that it cannot be instantiated from out side the class. The only way to instantiate an instance of class OnlyOne is through the getInstance() method with a public access modifier.

  

- **When to use:**

  Use it when only a single instance of an object is required in memory for a single point of access. For example the following situations require a single point of access, which gets invoked from various parts of the code.

  

- Accessing application specific properties through a singleton object, which reads them for the first time from a properties file and subsequent accesses are returned from in-memory objects. Also there could be another piece of code, which periodically synchronizes the in-memory properties when the values get modified in the underlying properties file. This piece of code accesses the in-memory objects through the singleton object (i.e. global point of access).

- Accessing in-memory object cache or object pool, or non-memory based resource pools like sockets, connections etc through a singleton object (i.e. global point of access).

  

Refer: https://dzone.com/articles/singleton-in-java

  

singleton class vs a static class

------------

  

Static class is one approach to make a class singleton by declaring all the methods as static so that you can’t create any instance of that class and can call the static methods directly.

  
  
  
  

Real world example

> There can only be one president of a country at a time. The same president has to be brought to action, whenever duty calls. President here is singleton.

  

In plain words

> Ensures that only one object of a particular class is ever created.

  

Wikipedia says

> In software engineering, the singleton pattern is a software design pattern that restricts the instantiation of a class to one object. This is useful when exactly one object is needed to coordinate actions across the system.

  

Singleton pattern is actually considered an anti-pattern and overuse of it should be avoided. It is not necessarily bad and could have some valid use-cases but should be used with caution because it introduces a global state in your application and change to it in one place could affect in the other areas and it could become pretty difficult to debug. The other bad thing about them is it makes your code tightly coupled plus mocking the singleton could be difficult.

  

**Programmatic Example**

  

To create a singleton, make the constructor private, disable cloning, disable extension and create a static variable to house the instance

```java

final class President

{

  private static President instance;

  

  private President()

  {

    // Hide the constructor

  }

  

  public static President getInstance()

  {

    if (instance == null) {

      instance = new President();

    }

  

    return instance;

  }

}

```

Then in order to use

```java

public class SingletonPattern {

  public static void main(String[] args) {

    President president1 = President.getInstance();

    President president2 = President.getInstance();

  

    System.out.println(president1.equals(president2));

  }

}

```

  
  
  

🎱 Proxy

-------------------

Real world example

> Have you ever used an access card to go through a door? There are multiple options to open that door i.e. it can be opened either using access card or by pressing a button that bypasses the security. The door's main functionality is to open but there is a proxy added on top of it to add some functionality. Let me better explain it using the code example below.

  

In plain words

> Using the proxy pattern, a class represents the functionality of another class.

  

Wikipedia says

> A proxy, in its most general form, is a class functioning as an interface to something else. A proxy is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes. Use of the proxy can simply be forwarding to the real object, or can provide additional logic. In the proxy extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked.

  

**Programmatic Example**

Taking our security door example from above. Firstly we have the door interface and an implementation of door

```java

interface Door {

    void open();

    void close();

}

  

public class RealDoor implements Door {

    public void open() {

        System.out.println("Opening the door");

    }

  

    public void close() {

        System.out.println("Closing the door");

    }

}

  

public class SecurityDoor implements Door {

    private Door door;

  

    public SecurityDoor(Door door) {

        this.door = door;

    }

  

    public void open() {

        System.out.println("Security door opening");

        door.open();

    }  

  

    public void close() {

        System.out.println("Security door closing");

        door.close();

    }

}

  

public class client {

}

```

  

🔗 Chain of Responsibility

-----------------------

  

Real world example

> For example, you have three payment methods (`A`, `B` and `C`) setup in your account; each having a different amount in it. `A` has 100 USD, `B` has 300 USD and `C` having 1000 USD and the preference for payments is chosen as `A` then `B` then `C`. You try to purchase something that is worth 210 USD. Using Chain of Responsibility, first of all account `A` will be checked if it can make the purchase, if yes purchase will be made and the chain will be broken. If not, request will move forward to account `B` checking for amount if yes chain will be broken otherwise the request will keep forwarding till it finds the suitable handler. Here `A`, `B` and `C` are links of the chain and the whole phenomenon is Chain of Responsibility.

  

In plain words

> It helps building a chain of objects. Request enters from one end and keeps going from object to object till it finds the suitable handler.

  

Wikipedia says

> In object-oriented design, the chain-of-responsibility pattern is a design pattern consisting of a source of command objects and a series of processing objects. Each processing object contains logic that defines the types of command objects that it can handle; the rest are passed to the next processing object in the chain.

  

**Programmatic Example**

  

Translating our account example above. First of all we have a base account having the logic for chaining the accounts together and some accounts

  

```java

  

public class COR {

    public static void main(String[] args) {

        NumberChain ch1 = new NegeativeHandler();

        NumberChain ch2 = new PositiveNumberHandler();  

        NumberChain ch3 = new ZeroHandler();

        ch1.setNextChain(ch2);

        ch2.setNextChain(ch3);

        ch1.handleNumber(new Number(1));

    }

}

  

interface NumberChain {

    public void setNextChain(NumberChain next);

    public void handleNumber(Number number);

}

  

class NegeativeHandler implements NumberChain {

    NumberChain nextHandler;

    public void setNextChain(NumberChain next) {

        if(nextHandler != null) {

            nextHandler = next;

        }

    }

  

    public void handleNumber(Number number) {

        System.out.println("Handling negative number");

        if(number.getNumber() < 0) {

            System.out.println("I can handle this number");

        } else {

            nextHandler.handleNumber(number);

        }

    }

}

  

class PositiveNumberHandler implements NumberChain {

    NumberChain nextHandler;

    public void setNextChain(NumberChain next) {

        if(nextHandler != null) {

            nextHandler = next;

        }

    }

  

    public void handleNumber(Number number) {

        System.out.println("HAndling positive Nsumber");

        if(number.getNumber() > 0) {

            System.out.println("I can handle Positive Numbers");

        } else {

            nextHandler.handleNumber(number);

        }

    }

}

  

class ZeroHandler implements NumberChain {

    NumberChain nextHandler;

  

    public void setNextChain(NumberChain next) {

        if (nextHandler != null) {

            nextHandler = next;

        }

  

    }

  

    public void handleNumber(Number number) {

        System.out.println("Handling Zeros");

        if(number.getNumber() == 0) {

            System.out.println("I can Handle zeros");

        } else {

            nextHandler.handleNumber(number);

        }

    }

}

  
  
  

class Number {

    private int number;

    public Number(int number) {

        this.number = number;

    }

  

    public int getNumber() {

        return number;

    }

}

  

```