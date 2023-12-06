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

```

**Shared mutable state is the building block of imperative programming.**