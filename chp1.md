# Functional Programming (Part-1)

## What do functions look like?
They get some values as inputs, do something, and maybe return values as outputs.

```java
public static int add(int a, int b){ // get two ints, adds them, and returns the sum
    return a + b;
}
```

## Meet the Function
Basically, each function consists of a ***signature*** and a ***body***, which implements the signature.

```java
public static int add(int a, int b){ // signature
    return a + b;  // body
}
```
- **Signature** is publicly visible and enough to understand what's going on inside the box
- **Body** is hidden part inside the function box.

## When the code lies

Some of the most difficult problems a programmer encounters happen when a code does **something it's not supposed to do**.

These problems are often related to the **signature** telling a different story than the **body**.

## Imperative vs. Declarative

- **Imperative programming** focuses on **how the result should be computed**. It is all about defining specific steps in a specific order. We achieve the final result by providing a detailed step-by-step algorithm.

- **The Declarative approach** focuses on **what need to be done** - **not how**.

#### Calculating the score imperatively
```java
public static int calculateScore(String word){
    int score = 0;
    for(char c: word.toCharArray()){
        score++;
    }
    return score;
}
```

#### Calculating the score declaratively
```java
public static int wordScore(String word){
    return word.length();
}
```
- A **Noun** makes our brain switch into the **declarative mode** and focus on what needs to be done **rather than the details of how to achieve it**.
- **Declarative code** is usually **more succinct and more comprehensible** than imperative code.
- Like **JVM or CPUs** are strongly imperative.
- In FP, we focus on what needs to happen more often then how it should happen.
