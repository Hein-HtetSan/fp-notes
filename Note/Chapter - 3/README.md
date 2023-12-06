# Immutable Values

### Functional Programming 😊

FP is programming using ___pure function___ that manipulate ___immutable values___.

🚨🚨 If pure function make up the engine of functional programming, immutable values are its fuel, oil and fumes.

_____
**Question**: ❓ How is it even possible to write fully working applications using only pure functions and values that can never change ❓❓

**Answer**: 🌿🌿Pure function make copies of data and pass them along. We need specific constructs in the language to be able to easily program using copies.
___

### Mutability is dangerous

Avoiding mutability lies at the heart of functional programming.

🎷🎷 Example   
Let's look at one of the possible implementations of replan function.

```java
List<String> planA = new ArrayList<>();
planA.add("Paris");
planA.add("Berlin");
planA.add("Krakow");
List<String> planB = replan(planA, "Vienna", "Krakow");

// replan method
static List<String> replan(List<String> plan, String newCity, String beforeCity){
    // plan is just a reference to the same list in memory as palnA, so any modification to plan is also done to PlanA.
    int newCityIndex = plan.indexOf(beforeCity);
    plan.add(newCityIndex, newCity);
    return plan;
}
```

#### Is **Replan** pure function? 🚨🚨

**replan** returns only one value and calculate it based on the provided arguments. But, as it turned oout, it **mutates existing values**. So, the answer is this: **no, replan is not a pure function**.

> 🧠 **Intuition** is very important in programming. The more intuitive the API you work with, the more effective you will be and the fewer bugs you will make.


## Fighting mutability by working with copies

Pure functions don't mutate any existing values. They can't modify anything from the argument list or the global scope. **However, they can mutate locally created values**.

```java
static List<String> replan(List<String> plan, String newCity, String beforeCity){
    int newCityIndex = plan.indexOf(beforeCity);
    // coping
    List<String> replanned = new ArrayList<>(plan);
    replanned.add(newCityIndex, newCity);
    return replanned;
}
```

## Shared Mutable State

A **state** is an instance of a value that is stored in one place and can be accessed from the code. If this value can be modified, we have **mutable state**. Furthermore, if this state can be accessed from different parts of the code, it's a **shared mutable state**.

```mermaid
Flowchart TD
    **Shared** --> This value can be accessed from many parts of the program   
    **mutable** --> This value can be modified in place.
    **State** --> This value is stored in a single place and is accessible.

```

**Shared mutable state is the building block of imperative programming.**

## Dealing with the moving parts

- **Our approach - Java**

```java
// this is the pure function
static List<String> replan(List<String> plan, String newCity, String beforeCity){
    int newCityIndex = plan.indexOf(beforeCity);
    List<String> replanned = new ArrayList<>(plan); // make sure it doesn't mutate any existing values
    replanned.add(newCityIndex, newCity);
    return replanned;
}
```

- **Object-oriented approach**

Use ***encapsulation*** to guard the changing data.

> ***Encapsulation*** is a technique that isolates a mutable state, usually inside an object. This object guards the state by making it **private** and making sure that all mutations are done only through this object's interface. Then, code responsible for manipulating the state is kept in one place. ALl the moving parts are hidden.

```java
// in oop, data and methods are coupled together. Data is private; it can be mutated only by method
private List<String> plan = new ArrayList<>();

// this method returns void because it mutates data in place.
public void replan(String newCity, String beforeCity){
    int newCityIndex = plan.indexOf(beforeCity);
    plan.add(newCityIndex, newCity);
}

// if we allow mutations, we need to explicity expose them as separate methods.
public void add(String city){
    plan.add(city);
}

// careful not to leak the internal data
// need to return a copy or a view to make sure nobody elese mutates state
public List<String> getPlan(){
    return Collections.unmodifiableList(plan);
}
```

- **Functional Approach** `used in OO code as well, and getting popular`

It tries to **minimize** the amount of moving parts - and ideally get **rid of them** completely.

**FP codebases don't use shared mutable states, they use immutable states, or simply immutable values that act as states.**

```scala
// List in Scala is immutable. The following code works as intended and doesn't mutate any values.

def replan(plan: List[String], newCity: String, beforeCity: String): List[String] = {
    val beforeCityIndex = plan.indexOf(beforeCity);
    val citiesBefore = plan.slice(0, beforeCityIndex);
    val citiesAfter = plan.slice(beforeCityIndex, plan.size);
    citiesBefore.appended(newCity).appendedAll(citiesAfter);
}
```

`List[String]`   
`slice(from, to): List[String]`: Return a new list that contains some elements from this list: a slice beginning at the **`from`** index and ending before the **`to`** index.

`List[String]`   
`appended(element): List[String]`: Returns a new list contains all the elements from this list with the **`element`** at the end  

`List[String]`   
`appendedAll(suffix: List[String]): List[String]`: Returns a new list that contians all the elements from this list with all the elements from the **`suffix`** list appended at the end.

## Immutable values in Scala

Scala really has built-in support for immutability. You can prove that 

```scala
val appleBook = List("apple", "Book")
-> List("apple", "Book")

val appleBookMango = appleBook.appended("Mango")
-> List("apple", "Book", "Mango")

appleBook.size
-> 2

appleBookMango.size
-> 3
```

***Immutability makes us focus on relations between values***

## Summary

- Mutability is dangerous
- Fighting mutability by using copies
- Shared mutable state
- Fighting mutability by using immutable values
- Using immutable APIs of String and List