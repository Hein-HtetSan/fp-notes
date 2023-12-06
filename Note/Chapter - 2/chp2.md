# Pure Function

## Why do we need pure functions?

Pure functions are the foundation of functional programming.

- Passing copies of data is one of the fundamental things done in functional programming!
- Focus on the logic by passing the state.

## Separation of concerns

**Separation of cocerns**: every piece of code has its own responsibility and is concerned only about it.

- In FP, we separate concerns into different functions.

|     ArrayList <small>`or any other type implementing the List interface`</small>      |
|-----------|
| `add(String): void` |
| `remove(String): void` |
| `iterator(): Iterator` |


## Pure Function

We use three rules to create pure functions, which are less buggy.

#### Rules of a pure function 

- Functions always return a single value
- Function calculates the return value based only on its arguments.
- Function doesn't mutate any existing values.

These are the main rules we should all follow when programming functionally.

> Mathematical functions are pure   
> f(x) = x * 95 / 100
>
> f(20) = 19   
> f(100) = 95   
> f(10) = 9.5

## Recalculating instead of storing

Storing
```java 
    public void addPerson(String name){
        names.add(name);
        if(names.size() > 5){
            tipPercentage = 20;
        }else if(names.size() > 0){
            tipPercentage = 10;
        }
    }
```

Recalculating
```java
    public int getTipPercentage(){
        if(names.size() > 5){
            return 20;
        }else if(names.size() > 0){
            return 10;
        }
        return 0;
    }
```

### Passing copies of data (Passing state)

```java
public LIst<String> addPerson(List<String> names, String name){
    List<String> updated = new ArrayList<>(names);
    updated.add(name);
    return updated;
}
```

## Pure function in Programming Languagae

### It returns only a single value

The java version of the function, as its mathematical conunterpart, exists to do one thing and one thing only: it always returns exactly one value.

### It calculates the return value based on its arguments

The java version of the function, exactly like the math version, takes one argument and calculates the result based solely on this argument. Nothing more is used by the function.

### It doesn't mutate any existing values

Both the Java and math versions of the function don't change anything in their environment. They don't mutate any existing values, and they don't use nor change any state fields. We can call them many times, and we will get the same result when we provide the same list of arguments.

## Single Responsibility

When a function can return only a single value and can't mutate any existing values, it can **only do one thing and nothing more**. In computer science, we say that it has a single responsibility.

## Referential Transparency

If the function uses only its arguments to compute a value, and it doesn't mutate any existing values, it automatically becomes referentially transparent.

## Testing pure functions

One of the biggest benefits of working with pure functions is their testability.

#### Code
```scala
def getDiscountPercentage(items: List[String]): Int = {
    if(items.contains("Book")){
        5
    }esle{
        0
    }
}
```

#### Testing
```scala
getDiscountPercentage(List.empty) == 1  // false

getDiscountPercentage(List.empty) == 0 // true

getDiscountPercentage(List("Apple", "Book")) == 5 // true
```

## __*Summary*__
- ### Passing copies of Data
- ### Recalculating instead of storing
- ### Passing the state
- ### Testing pure functions