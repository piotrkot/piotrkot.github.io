---
layout: posts
title: Procedural Object-Oriented Programming
---

### Object-Oriented Paradigm
I am sure you know what Object-Oriented Programming (OOP) is. You heard the principles of SOLID, YAGNI, KISS, DRY, etc. I can even safely assume you already use a language supporting OOP, have created inheritance classes, encapsulation, polymorphism, and even implemented some design patterns.
That's good. Very good. Since it is a dominant programming methodology aimed to improve the maintainability, decomposition, modeling, extensibility, code reuse, and drastically decrease the code base!

At least from theory. But is it so from your experience as well?

I can almost hear that hesitation in your voice.

And why is that? We know the definitions and practices. How come it does not work out?

### The devil is in the details
Let's look from the historical angle to the times of Simula 67 and Smalltalk. Initiative for creation of Object-Oriented thinking was idea of objects and messages as the basis for computation. There were solid requirements to carefully design, assign responsibilities, and define collaborators. However, perhaps due to business urge in delivering value, rejection of software art, and need for structured, processed, and manufactured software it led to data abstraction and modular programming, instead. Eventually, became Procedural Object-Oriented Programming (POOP)

You might think you are still doing it right.

* Internal data and routines are carefully encapsulated within objects. Yet, for the sake of extensibility you let the data be visible with getters and setters.
* Complicated programs are decoupled with modularity and scoping in mind, and, to avoid duplication, the common code is extracted to abstract classes.
* Sometimes you find it difficult to choose the right place or name for functionality but that is what you have utility classes and static methods for, don't you?
* Of course, that global mutable variables are terrible practice but Singleton design pattern from the famous book is a different thing, isn't it?
* Maybe the whole idea of declarative programming is not that clear to you since, after all, you are the computer programmer and you must know what the computer does and when. So you feel more familiar having control over the explicit methods calls, their state of execution, the data operated by. Why would you limit yourself from debugging the program?

This is sadly all wrong. The devil is in the details.

### Doing laundry the procedural way 
Consider a simple task as doing laundry. This is a good task as forces us to focus on business aspects and not technical details.

Without too much thinking, as for Procedural Object Oriented Programming, we can do following.

```java
get Bathroom().fetchDirtyClothes();
get WashingMachine().add(dirtyClothes);
get WashingMachine().add(washingPowder);
// assume it's an old-fashioned machine, without programs
get WashingMachine().set(temperature, spinning, waterAmount);
// as wife knows it all, better check with her
get WashingMachine().validateValues(Wife.knowledge());
get WashingMachine().doLaundry();
```

Pretty simple, isn't it? We use objects, and call methods. No need for static calls at this point.
There is a setter which doesn't make the class immutable and so thread-safe but it is not the real problem for now. 
The tricky part might be checking with `Wife`. I suspect there might be a loop, or a validator, or even sophisticated rule engines referring to `Wife`'s knowledge. So let it be. We need to make it extensible, right?

Unfortunately, as with every specification there are few details missing.

* you cannot mix clothes with different color and material - this could require a filter functionality
* dirty clothes can also be found in other `Rooms` - we should enable global `Room` search
* what if there is no washing powder? We should buy some! - it is a difficult case and smells like `GOTO` statement
* we must have money to buy powder - Money is always good to keep eye on, perhaps as a utility method
* <del>what if there is no money?</del> - No, no. Getting too complicated.
* we should find the `Shop` with our product (closest one or cheapest one?) - an advanced filter is needed
* get to the `Shop`
* fetch washing powder
* maybe we should also fetch other products? I rarely go just for the powder - this is advanced feature, not related; hope we convince customer they don't need it.

Honestly, I think the scenario is quite possible. Often we initially get a specification for a *window* while we end up with the whole *glass house*.
Anyway, isn't it getting a bit overwhelming? How about the estimation?

Unfortunately, this is very common scenario. Code gets implemented and changed without thinking too much.

Added abstractions are based on code similarities and not objects and responsibilities - `fetch` & `add` methods could be shared at some point, though they are completely different. Programmers will identify other technical (not business) decompositions and code reuses like automated talking to different objects. They will make a singleton which stores all objects and can serve on request. Don't you think it is possible? And what Dependency Injection Container is doing?

Further can be required to support very dirty clothes. So dirty that for safety reasons one needs to wear gloves and helmet. As the clothes can have two forms some data oriented programmers will discover yet another singleton that converts those dirty clothes to the ones safely carried and back. Don't you think it is possible? And what Object-Relational Mapping is doing?

Singletons and utility methods already crept in. As it is getting complicated there might be a proposal of layers for separation, like `Room` data layer, `House` service layer, `Shop` service layer. Eventually all gets technically structured and data oriented with maintenance hell.

### Doing laundry the object-oriented way
With object-orientation in mind, all starts from thinking of the classes, their responsibilities, and collaborators. They will form tools for the solution like the little libraries and the solution itself will be defined in the main executed class. It is analogous to casting and directing a play. You select the players, the objects that will collectively complete the tasks. Next, provide the cast with the script and dialogs.

Our script could be as simple as following.

```java
wife.doLaundry(Type(color, material))
```

In fact we should rather very politely ask. Indeed, since she knows it better we can suggest her to carry that responsibility (a bouquet of flowers once a while helps here).

With loose coupling between `House`, `Bathroom`, `WashingMashine` and knowing the `WashingProgram` we can proceed with ease.


```java
public void doLaundry(Type type) {
  clothes = house.get(Bathroom).clothesBy(type);
  house.get(Bathroom).get(WashingMachine)
    .new WashingProgram(type).doLaundry(clothes);
}
```

New functionality of collecting dirty clothes delegated to `Room` objects can be added smoothly.

```java
public void collectDirtyClothes() {
    dirty = house.iterateRooms().findDirtyClothes();
    house.get(Bathroom).add(dirty);
}
```

Encouraged with straight-forward laundry, you might think shopping is as simple as buying both available and required list of Products. After all it is the same Product, isn't it? The product has its type, price, producer, due date, ingredients, size, weight etc. Wouldn't that be right to represent it as an entity? No, it would not, because it is not the data that is important!

Even though all those attributes describe the data, it is not the same attributes that every party is interested in. For product's producer perhaps weight, size, price, and shipping strategy is important. For the shop it could be shelf location and pricing strategy. And for the customer ingredients and due date. Being myself a customer I often get notes on my shopping lists: "*while cheese but not the one you bought last time*", "*good red wine - we have visitors*", or "*some fish*" - but then I find no fresh one and take shrimps, instead.

With different purpose we look at things differently. And the only way to understand that right is to closely talk to the business. By separating from the business you end up with sophisticated, theoretical solution to the problem that does not exist.

Now, the OOP pseudo-code altogether.

```java
class Application {
  public static void main() {
    wife.doLaundry(Type(color, material));
    background.execute(
      new Runnable() {
        public void run() {
          wife.collectDirtyClothes();
        }
      },
      new Runnable() {
        public void run() {
          me.doShopping(wife.demand());
        }
      }
    );
  }
}
```

```java
class Wife {
  private House house;
  public void doLaundry(Type type) {
    clothes = house.get(Bathroom).clothesBy(type);
    house.get(Bathroom).get(WashingMachine)
      .new WashingProgram(type).doLaundry(clothes);
  }
  public void collectDirtyClothes() {
    dirty = house.iterateRooms().findDirtyClothes();
    house.get(Bathroom).add(dirty);
  }
  public Demand demand() {
    return new Demand(
      house.iterateRooms().lowProducts().sum(),
      new ShoppingStrategy()
    );
  }
}

class House {
  private Iterable<Room> rooms;
  public Iterable<Room> iterateRooms() {...}
  public Room get(Room) {...}
}

class Bathroom {
  private Iterable<Wear> clothes;
  public Iterable<Wear> clothesBy(Type type) {...}
  public add(Iterable<Wear> clothes) {...}
}

class WashingMachine {
  class WashingProgram {
    public void doLaundry(Iterable<Wear> clothes) {...}
  }
}

enum Type {
  private Color color;
  private Material material;
  public temperature() {...}
  public spinning() {...}
  public waterAmount() {...}
  public powder() {...}
}

class Me {
  private Area area;
  private Money money;
  public void doShopping(Demand demand) {
    shops = area.findShops(demand);
    for (Shop shop in shops) {
      basket = shop.sell(demand, money);
      demand -= basket.products();
      money -= basket.price();
    }
  }
}

class Area {
  private Iterable<Shop> shops;
  public findShops(Demand demand) {...}
}

class Shop {
  private Supply supply;
  public Basket sell(Demand demand, Money money) {...}
}
```

This is just a sketch but I hope you got the idea. Proper Object-Oriented code, makes it clear to understand and extend while keeping the complexity low.

You may say that this theoretical exercise is far from practical uses. Then please consider that `clothes` can be in fact records in **Bathroom Database**. And `clothes` in other rooms could be **REST resources in external systems**. Under such circumstances the problem becomes technical as well. But this is not important as a matter of facts, because Object-Oriented thinking is about real-life objects than IT infrastructure itself.

To quote Grady Booch from 1991:

> Let there be no doubt that object-oriented design is fundamentally different than traditional structured design approaches: it requires different ways of thinking about decomposition, and it produces software architectures that are largely outside the realm of the structured design culture. 


