# Functions as values 😎

### Implementing requirements as functions 🎯

This time is working with **Pure functions and immutable values** how well these two concpets work together.

#### Ranking Words 🥪
- The score of a given word is calculated by giving one point for each letter that is not an 'a'.
- For a given list of words, return a sorted list that starts with the hightest-scoring word

Let's start with some pseudocode first
1. Score each word in the list based on an external function **score**.
2. Create a new list that has the same elements but sorted starting from the highest-scoring word.
3. Return the newly created list.


### Version #1: Using `Comparator` and `sort` 🐼

```java
static int score(String word){
    return word.replaceAll("a", "").length();
}

static Comparator<String> scoreComparator = new Comparator<String>() {
    public int compare(String w1, String w2){
        return Integer.compare(score(w2), score(w1));
    }
};

static List<String> rankedWords(List<String> words){
    words.sort(scoreComparator);
    return words;
}

List<String> words = Array.asList("ada", "haskell", "scala", "java", "rust");
List<String> ranking = rankedWords(words);
System.out.println(ranking);

// console output: [haskell, rust, scala, java, ada]
```

It worked! There is one problem though. **It mutate the existing words list inside the function**.

### Version #2: Ranking the words using `Streams` 🐼

```java
static List<String> rankedWords(List<String> words){
    return words.stream() // returns a new streams, which produces element from the words list
                .sorted(scoreComparator) // return a new stream, which produces elements from the previous `Stream` but sorts using the provided `Comparator`
                .collect(Collectors.toList()); // Returns a new `List` by copying all elements from the previous `Stream`.

static List<String> words = Array.asList("ada", "haskell", "scala", "java", "rust");
List<String> ranking = rankedWords(words);
System.out.println(ranking);
System.out.println(words);

// consle output
// [haskell, rust, scala, java, ada]
// [ada, haskell, scala, java, rust]
}
```

***Some mainstream languages expose APIs that embrace immutability. (e,g Java Stream)***   
Nothing is mutated in above code. Each method call returns a new immutable object, while `collect` returns a brand new `List`.

> ***Function signatures should tell the whole story*** 🎯

### Version #3: Passing algorithms as arguments

***Before***

```java
// impure functioon
static List<String> rankedWords(List<String> words){
    return words.stream()
                .sorted(scoreComparator) // not a parameter argument
                .collect(Collectors.toList());
}
```

***After***
```java
// The function calculates the return value based only on its arguments
// pure function
static List<String> rankedWords(Comparator<String> comparator, List<String> words){
    return words.stream()
                .sorted(comparator)
                .collect(Collectors.toList());
}
```

**By passing a `Comparator` instance, we pass the specific behavior - not an instance of some data**

### Version #4: Changing the ranking algorithm


|***Original Requirements***| ***Additional Requirements***|
|-----|------|
| The score of a given word is calculated by giving one point for each letter that is not an 'a'.|  A bouns score of 5 needs to be added to the score if the word contains a 'c'.|
|For a given list of words, return a sorted list that starts with the hightest-scoring word.|The old way of scoring (without the bonus) should still be supported in the code.|

***Score***

```java
static int score(String word){
    return word.replaceAll("a", "").length();
}

static Comparator<String> scoreComparator = new Comparator<String>(){
    public int compare(String w1, String w2){
        return Integer.compare(score(w2), score(w1));
    }
};

rankedWords(scoreComparator, words);
```

***ScoreWithBonus***

```java
static int scoreWithBonus(String word){
    int base = score(word);
    if(word.contains("c")) return base + 5;
    else return base;
}

static Comparator<String> scoreWithBonusComparator = new Comparator<String>(){
    public int compare(String w1, String w2){
        return Integer.compare(scoreWithBonus(w2), scoreWithBonust(w1));
    }
};

rankedWords(scoreWithBoundsComparator, words);
```

### Technique to cut a lot of code.

```java
Comparator<String> scoreComparator = 
(w1, w2) -> Integer.compare(score(w2), score(w1));
```

```java
rankedWords(
    (w1, w2) -> Integer.compare(score(w2), score(w1));
);
```

### Using Java's `Function` values

**Functions `stored as values` are what FP is really about.**

- To create a `Function` value, we need to use the `arrow syntax ->`. This definition is equivalent to the one on the left.
- We can call the functions stored as  `Function` values by calling the `.apply` method.
- We can reuse `Function` values in a way similar to how reuse functions.

```java
// scoreFunction is a reference to an object in memory that holds a function from String to Integer

Function<String, Integer> scoreFunction =
    w -> w.replaceAll("a", "").length();

scoreFunction.apply("java");
-> 2

// can also create another reference to the same value and use it.
Function<String, Integer> f = scoreFunction;
f.apply("java");
-> 2
```
### Version #5: Passing the scoring function

***Functions that take functions as parameters are ubiquitous in FP code***.

```java
Function<String, Integer> scoreFunction = w -> score(w);

Function<String, Integer> scoreWithBonusFunction = w -> scoreWithBonus(w);

static List<String> rankedWords(Function<String, Integer> wordScore, List<String> words){
    Comparator<String> wordComparator = (w1, w2) -> Integer.compare(
        wordScore.apply(w2),
        wordScore.apply(w1)
    );

    return words
        .stream()
        .sorted(wordComparator)
        .collect(Collectors.toList());
}

// note how the caller of the rankedWords function is responsible for providing the scoring algorithm.
rankedWords(w -> score(w), words);
rankedWords(w -> scoreWithBouns(w), words);
```

### Passing functions in Scala

```scala
def inc(x: Int): Int = x + 1
inc(2)
-> 3

def score(word: String): Int = word.replaceAll("a", "").length
score("java")
-> 2

val words = List("rust", "java")
-> words: List[String]

words.sortBy(score) // sortBy function takes another function as a parameter.
-> List("java", "rust")
```
### Signature with Function parameters in Scala

**Functions that don't lie are crucial for maintainable codebases***   

So, the reader of our code can just look at the signature and completely understand what's going on inside, without even looking at the implementation.

```scala
def score(word: String): Int = word.replaceAll("a", "").length

def rankedWords(wordScore: String => Int, words: List[String]): List[String] = {
    def negativeScore(word: String): Int = -wordScore(word)
    words.sortBy(negativeScore)
}
```
- `=>` to mark parameters that are functions (eg., String => Int)

### Practicing function passing 🌐

It's time to **pass some function in Scala**. Please use the Scala [REPL](https://replit.com/).

```scala
def len(s: String): Int = s.length
List("scala", "rust", "ada").sortBy(len)
-> List("ada", "rust", "scala")

def numberOfS(s: String): Int = s.length - s.replaceAll("s", "").length
List("rust", "ada").sortBy(numberOfS)
-> List("ada", "rust")

def negative(i: Int): Int = -i
List(5,1,2,4,3).sortBy(negative)
-> List(5,4,3,2,1)

def negativeNumberOfS(s: String): Int = -numberOfS(s)
List("ada", "rust").sortBy(negativeNumberOfS)
-> List("rust", "ada")
```