<!--
Market: SF
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

# Callbacks & Iterators

## Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

Using callback functions is an effective way to write declarative, functional JavaScript. JavaScript was built to deal with asynchronous events; callbacks help appropriately time and coordinate our program.

## What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

- Pass a function as a callback to another function
- Use callbacks to build more declarative iterators
- Pick the best iterator for the job
- Build certain iterators from scratch


## Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

- Write and call functions in JavaScript
- Explain what a first-order function is
- Use a `for` loop

## Examples of Callbacks

Let's walk through a couple of examples of code that utilize callbacks:

```javascript
var element = document.querySelector("body");
var counter = 0;
element.addEventListener("click", function(){
  counter += 1;
  console.log("clicked " + counter + " times.");
})
```
*Discuss the above example with your neighbor.*

## Example of an Iterator

```javascript
var potatoes = ["Yukon Gold", "Russet", "Yellow Finn", "Kestrel"];
for(var i=0; i < potatoes.length; i++){
    console.log(potatoes[i] + "!")
}
```

*Discuss the above example with your neighbor.*

###[forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/ForEach)

Let's combine our knowledge of callbacks & iterators to write better, more declarative code.

```javascript
var fruits = ["Apple", "Banana", "Cherry", "Durian", "Elderberry",
"Fig", "Guava", "Huckleberry", "Ice plant", "Jackfruit"];

fruits.forEach(function (value, index) {
    console.log(index + ". " + value);
});

// 0. Apple
// 1. Banana
// 2. Cherry
// 3. Durian
// 4. Elderberry
// 5. Fig
// 6. Guava
// 7. Huckleberry
// 8. Ice plant
// 9. Jackfruit
//=> ["Apple", "Banana", "Cherry", "Durian", "Elderberry",
//    "Fig", "Guava", "Huckleberry", "Ice plant", "Jackfruit"];
```

### [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Map)
Similar to `forEach()`, `map()` traverses an array; this method, however, performs whatever callback function you pass into it on each element.  It outputs the results
of the operation on each element as a new array.

Often we want to do more than just perform an action, like console.log(), on every loop.  When we actually want to modify/manipulate our array, map is our best friend!

#### Example: Double every number

```JavaScript
var numbers = [1, 4, 9];
var doubles = numbers.map(function doubler(num) {
  return num * 2;
});
// doubles is now [2, 8, 18]. numbers is still [1, 4, 9]
```

#### Example: Pluralize all the fruit names

```javascript
pluralized_fruits = fruits.map(function pluralize(element) {

  // if word ends in 'y', remove 'y' and add 'ies' to the end
    var lastLetter = element[element.length -1];
     if (lastLetter === 'y') {
      element = element.slice(0,element.length-1) + 'ie';
  }

    return element + 's';
});

fruits // ORIGINAL ARRAY IS UNCHANGED!
//=> ["Apple", "Banana", "Cherry", "Durian", "Elderberry",
//    "Fig", "Guava", "Huckleberry", "Ice plant", "Jackfruit"];

pluralized_fruits // MAP OUTPUT
//=> [ "Apples", "Bananas", "Cherries", "Durians", "Elderberries",
//    "Figs", "Guavas", "Huckleberries", "Ice plants", "Jackfruits"  ]
```

#### Example: Square each number

```javascript
var numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

numbers.map(function square(element) {
  return Math.pow(element, 2);
});
//=> [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

#### Exercise - [Map](exercises.md#challenge-1-map)

### [filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Filter)
With the `filter()` method you can create a *second* array filled with elements that pass certain criteria that you designate.  This is great for creating a sub array of fruits that start with vowels, a list of even numbers from a bigger list, and so on.  
  *It's important to remember that a filter method on an array requires a `boolean` return value for the callback function you pass as an argument.*

#### Example: Return a list of fruits that start with vowels

```javascript
var vowels = ["A", "E", "I", "O", "U"];
function vowelFruit(fruit) {
  var result = vowels.indexOf(fruit[0]) >= 0; // indexOf returns -1 if not found
  console.log("result for " + fruit + " is " + result);
  return result;
}
var vowelFruits = fruits.filter(vowelFruit);
//=> ["Apple", "Elderberry", "Ice plant"]
```

Or alternatively:

```javascript
var vowels = ["A", "E", "I", "O", "U"];

var vowelFruits = fruits.filter(function vowelFruit(fruit) {
  return vowels.indexOf(fruit[0]) >= 0; // indexOf returns -1 if not found
});
//=> ["Apple", "Elderberry", "Ice plant"]

```

#### Example: Find all the even numbers that are greater than 5 

```javascript
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

even = numbers.filter(function filterEvens(num) {
  var isEven = num%2==0;
  var greaterThanFive = num > 5;
  return isEven && greaterThanFive;
});
//=> [6, 8, 10]

```

#### Exercise - [Filter](exercises.md#challenge-2-filter)

## [reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
The `reduce()` method is designed to create one single object that is the result of an action performed among all elements in an array.  It essentially 'reduces' the values of an array into one single element.

#### Example: Return the first letter of every word, as a single string

```javascript
avgLen = fruits.reduce(function concatFirstLetter(previous, current, index) {
    if (index == 1) {
      current = current[0];
    }
  return previous + current[0];
});
//=> "ABCDEFGHIJ"

```

#### Example: Find the sum of all of the numbers in an array

```javascript
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

sum = numbers.reduce(function add(previous, current) {
  return current + previous;
});

sum
//=> 55

```

> In the above examples, notice how the first time the callback is called it receives `element[0]` and `element[1]`.
 
There is another way to call this function and specify an initial starting point. 

```javascript
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

sum = numbers.reduce(function add(previous, current) {
  return current + previous;
}, 100);

sum
//=> 155
```

In the above example, the first time the callback is called it receives `100` and `1`.

> **Note**: We set the starting value to `100` by passing in an optional second argument to reduce. 

#### Exercise - [Reduce](exercises.md#challenge-3-reduce)

## Exercises

Before we move on let's finish the [Array Manipulation Exercises](exercises.md) for `map`, `filter`, and `reduce`.


# Building Iterators Lab

#### How does `forEach` work?
Let's think about `forEach` again. What's happening behind the scenes?

* What are our inputs?
* What is our output?
* What happens on each loop?
* What does the callback function do?
* What gets passed into our callback function? i.e. what arguments?
  * Where does it come from?
  * How do we know what to name it?

Let's check:

```javascript
[0, 100, 200, 300].forEach(function printer(element, index, array) {
  console.log(element, index, array);
});
```


<details><summary>
**What are our inputs?** (Click Here)
</summary>
Inputs to `forEach`:
* 1. `callback` (a function) - e.g. `printer`
* 2. `thisArg` (an optional argument) - not used

Inputs to the callback:
* 1. `currentValue` - e.g. `300`
* 2. `index` - e.g. `3`
* 3. `array` - e.g. `[0, 100, 200, 300]`
</details>
  
<details><summary>
**What is our output?** (Click Here)
</summary>
Output from calling `forEach`:
* `undefined` - You can confirm this in your console.

Output from the callback:
* We're not explicitily returning anything in this case, but we do `console.log` every time the callback is called (every loop).
</details>

Given the above, how would you build `forEach` yourself?

## Exercise: Build your own iterators

We are going to [**implement our own iterators**](https://github.com/sf-wdi-29/js-building-iterators-lab), from scratch. 
