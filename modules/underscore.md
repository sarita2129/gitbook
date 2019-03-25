# Underscore

## **Underscore.js**

### What is it?

[Homepage](http://underscorejs.org/)

> "Underscore is a JavaScript library that provides a whole mess of useful functional programming helpers"

> "It’s the answer to the question: “If I sit down in front of a blank HTML page, and want to start being productive immediately, what do I need?"

It's a utility belt, over hundred functions that will help you get stuff done.

### Who built it?

* Jeremy Ashkenas \( @jashkenas \)
  * [Twitter](https://twitter.com/jashkenas)
  * [Github](https://github.com/jashkenas)
* He also built:
  * [Backbone](http://backbonejs.org/)
  * [Coffeescript](http://coffeescript.org/)

### Why do we teach it?

* It's incredibly useful
* Brings a lot of Ruby's style and functionality across \(as does Coffeescript\)
* It is a dependency of Backbone \(with one small caveat\)
  * There is another library called `lo-dash`
  * It is a fork of underscore and is a direct replacement
* Saves you from repeating code

### What's the approach?

* Just like jQuery has the `$`, Underscore uses the `_`
* All of it's functions are scoped, or nested, within that `_`
* Relies heavily on **predicate** methods - ones that always return `true` or `false`

```javascript
_.each( /* ... */ );
_.sortBy( /* ... */ );
_.where( /* ... */);
```

### What's the approach?

It breaks the functions down into six categories:

* Collections
* Arrays
* Objects
* Utilities
* Functions
* Chaining

Because there are over a hundred of these things, we aren't going to go through them all.

Let's look at some of them though!

### How do we use it?

Reference it just like any other Javascript library!

* Download the file \|\| Get it from the CDN \|\| Or use a gem
* Make sure you reference it before any JS that uses it!

e.g.

```markup
<script src="https://cdnjs.cloudflare.com/ajax/libs/underscore.js/1.8.3/underscore-min.js"></script>
```

### Collections

#### \_.each\(\)

```javascript
// Works on arrays...

_.each( [1, 2, 3], function (num) {
  console.log( num );
});

// Works on objects!

_.each( { one: 1, two: 2, three: 3 }, function ( value, key ) {
  console.log( value );
});
```

### Collections

#### \_.map\(\), \_.reduce\(\)

```javascript
// Works on arrays...

_.map( [1, 2, 3], function (num) {
  return num * 3;
});

// Works on objects... but returns an array

_.map( { one: 1, two: 2, three: 3 }, function ( value, key ) {
  return value * 3;
});

_.reduce( [1, 2, 3], function (sum, num) {
  return sum + num;
}, 0 ); // => 6

// reduce is an alias for inject
```

### Collections

#### \_.where\(\), \_.findWhere\(\), \_.filter\(\), \_.reject\(\), \_.find\(\)

```javascript
var data = [
  { id: 22, username: "Martin", active: true },
  { id: 23, username: "Max",    active: false},
  { id: 24, username: "Linda",  active: false}
];

_.where( data, { active: false } );     // => [{ id: 23, ... }, { id: 24, ... }]
_.findWhere( data, { active: false } ); // => { id: 23, ... }

var nums = [ 1, 2, 3, 4, 5, 6 ];

_.filter( nums, function ( num ) {
  return num % 2 === 0;
}); // => [ 2, 4, 6 ] - _.find will return the first one of this

_.reject( nums, function ( num ) {
  return num % 2 === 0;
}); // => [ 1, 3, 5 ]
```

### Collections

#### \_.sortBy\(\), .groupBy\(\)

```javascript
var stooges = [
  {name: 'moe', age: 40},
  {name: 'larry', age: 50},
  {name: 'curly', age: 60}
];
_.sortBy(stooges, 'name'); // => [{/* Curly */}, {/* Larry */, {/* Moe */}]

_.groupBy( [1.3, 2.1, 2.4], function(num) {
  return Math.floor(num);
}); // => { 1: [1.3], 2: [2.1, 2.4] }
```

### Collections

#### \_.every\(\), \_.some\(\), \_.contains\(\)

```javascript
var data = [ 1, 2, 3, 4, 5 ];

_.every( data, function ( num ) {
  return num % 2 === 0;
}); // => false

_.some( data, function ( num ) {
  return num % 2 === 0;
}); // => true

_.contains( data, 3 ); // => true
```

### Collections

#### \_.pluck\(\), \_.max\(\), \_.min\(\)

```javascript
var stooges = [
  {name: 'moe', age: 40},
  {name: 'larry', age: 50},
  {name: 'curly', age: 60}
];

_.pluck( stooges, 'name' ); // => [ 'moe', 'larry', 'curly' ]

_.max( stooges, 'age' ); // => 60
_.min( stooges, 'age' ); // => 40
```

### Collections

#### \_.countBy\(\), \_.shuffle\(\), \_.sample\(\), \_.size\(\)

```javascript
var data = [ 1, 2, 3, 4, 5 ];

_.shuffle( data ); // => [ 3, 2, 5, 4, 1 ] (random every time)
_.size( data ); // => 5
_.sample( data ); // => 3 (random every time)
_.sample( data, 3 ); //=> [ 3, 5, 1 ] (random every time)

_.countBy([1, 2, 3, 4, 5], function(num) {
  return num % 2 == 0 ? 'even': 'odd';
}); // => { odd: 3, even: 2 }
```

### Exercise Time!

[Here they are!](https://gist.github.com/ga-wolf/904a192f5ae50989e1fe)

### Arrays

#### \_.first\(\), \_.last\(\), \_.initial\(\), \_.rest\(\)

#### \_.compact\(\), \_.flatten\(\)

* `_.first()` and `_.last()` do exactly what you'd expect
* `_.initial()` returns everything except the last element\(s\)
* `_.rest()` returns everything except the first element\(s\)

```javascript
_.compact([0, 1, false, 2, '', 3]); // => [1, 2, 3]

_.flatten([1, [2], [3, [[4]]]]); // => [1, 2, 3, 4];
_.flatten([1, [2], [3, [[4]]]], true); // => [1, 2, 3, [[4]]];
```

### Arrays

#### \_.without\(\), \_.union\(\), \_.intersection\(\), \_.difference\(\), \_.uniq\(\)

```javascript
_.without([1, 2, 1, 0, 3, 1, 4], 0, 1);
// => [2, 3, 4]

_.union([1, 2, 3], [101, 2, 1, 10], [2, 1]);
// => [1, 2, 3, 101, 10] - all unique items

_.intersection([1, 2, 3], [101, 2, 1, 10], [2, 1]);
// => [1, 2] - all items that are present in all arrays

_.difference([1, 2, 3, 4, 5], [5, 2, 10]);
// => [1, 3, 4] - all items that are present in the first array and nowhere else
```

### Arrays

#### \_.zip\(\), \_.unzip\(\), \_.object\(\)

```javascript
_.zip(['moe', 'larry', 'curly'], [30, 40, 50], [true, false, false]);
// => [["moe", 30, true], ["larry", 40, false], ["curly", 50, false]]

_.unzip([['moe', 30, true], ["larry", 40, false], ["curly", 50, false]])
// => [['moe', 'larry', 'curly'], [30, 40, 50], [true, false, false]]

_.object(['moe', 'larry', 'curly'], [30, 40, 50]);
// => {moe: 30, larry: 40, curly: 50}

_.object([['moe', 30], ['larry', 40], ['curly', 50]]);
// => {moe: 30, larry: 40, curly: 50}
```

### Arrays

#### \_.indexOf\(\), \_.lastIndexOf\(\), \_.findIndex\(\), \_.findLastIndex\(\), \_.sortedIndex\(\)

* `_.indexOf()` and `_.lastIndexOf()` find a value and return it's index \(or -1\)
* `_.findIndex()` returns the index of the first thing that passes a predicate function

```javascript
var stooges = [{name: 'moe', age: 40}, {name: 'curly', age: 60}];
_.sortedIndex(stooges, {name: 'larry', age: 50}, 'age'); // => 1

// _.sortedIndex() tells you where you should insert an element
```

### Arrays

#### \_.range\(\)

```javascript
_.range(10);
// => [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

_.range(1, 11);
// => [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

_.range(0, 30, 5);
// => [0, 5, 10, 15, 20, 25]

_.range(0, -10, -1);
// => [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
```

### Exercise Time!

[Here they are!](https://gist.github.com/ga-wolf/79539b5b974fe1b56a2c)

### Objects

#### \_.keys\(\), \_.mapObject\(\), \_.pairs\(\), \_.invert\(\)

```javascript
var data = { name: "Unknown", location: "Unknown" };
_.keys( data ); // => [ "name", "location" ]

var data = { start: 100, end: 200 };
_.mapObject( data, function (value, key) {
  return value * 2;
}); // => { start: 200, end: 400 }

_.pairs(data); // => [["start", 100], ["end", 200]]

_.invert( data ); // => { "100" : "start", "200" : "end" }
```

### Objects

#### \_.pick\(\), \_.omit\(\), \_.defaults\(\), \_.has\(\), \_.isEqual\(\), \_.isMatch\(\)

```javascript
var data = { name: "N/A", location: "N/A", description: "N/A" };

_.pick( data, "name", "location" ); // => { name: "N/A", location: "N/A" }

_.omit( data, "description" ); // => { name: "N/A", location: "N/A" }

_.defaults( { name: "N/A" }, { drinksWater: true } ); // => { name: "N/A", drinksWater: true }

_.has( { name: "N/A" }, "name" ); // => true

_.isEqual( { name: "N/A" }, { name: "N/A" } );

_.isMatch( { name: "N/A", location: "N/A" }, { location: "N/A" } ); // => true
```

### Objects

#### \_.is\*\*\*\*\*\*\(\)

* isEmpty
* isElement
* isArray
* isObject
* isArguments
* isFunction
* isString
* isNumber
* isFinite
* isBoolean
* isDate
* isRegExp
* isNaN
* isNull
* isUndefined

### Exercise Time!

[Here they are!](https://gist.github.com/ga-wolf/0eb9595f074af427d6b8)

### Utilities

#### \_.times\(\), \_.random\(\), \_.escape\(\), \_.unescape\(\)

```javascript
_.times( 3, function () { console.log( "Hi" ); });

_.random(0, 100); // This is inclusive!

_.escape('Curly, Larry & Moe');
// => "Curly, Larry &amp; Moe"

_.unescape('Curly, Larry &amp; Moe');
// => "Curly, Larry & Moe"
```

### Utilities

#### \_.now\(\), \_.template\(\)

```javascript
_.now() // Returns a timestamp of the current time

var templateString = "<p> Hello <%= name %>! </p>";
var template = _.template( templateString );
var compiledTemplate = template( { name: "Jane" } );
```

### Functions

#### \_.delay\(\), \_.once\(\)

```javascript
_.delay(function () { console.log("Hi"); }, 1000);

var createApplication = function () {
  console.log("Hi");
}

var initialize = _.once(createApplication);
initialize();
initialize();
```

### Functions

#### \_.throttle\(\), \_.debounce\(\)

Very weird functions.

* Throttling enforces a maximum number of times a function can be called. As in execute this function at most once every 100 milliseconds.
* Debouncing enforces that a function not be called again until a certain amount of time has passed without it being called.

Use cases:

* Infinite Scroll
* Double Clicks - see [here](http://jsfiddle.net/missinglink/19e2r2we/)
  * Throttling won't filter double clicks
  * Debouncing will

[See here](http://codepen.io/chriscoyier/pen/vOZNQV) for a visual representation.

### Functions

#### \_.throttle\(\), \_.debounce\(\)

```javascript
var throttled = _.throttle(updatePosition, 100);
$(window).scroll( throttled );

var lazyLayout = _.debounce(calculateLayout, 300);
$(window).resize(lazyLayout);
```

### Chaining

* Two approaches

```javascript
_( [1, 2, 3] ).map( function (num) {
  return num * 3;
}); // => [ 3, 6, 9 ]
// Only works for one additional method!

_.chain( ["one", "two", "three"] )
 .map( function (word) {
   return word + " mapped";
 })
 .map( function (word) {
   return word.toUpperCase();
 }); // => [ "ONE MAPPED", "TWO MAPPED", "THREE MAPPED" ]
 // This approach is much better!
```

### That is Underscore!

Don't expect to remember all of that, but do remember that the documentation for Underscore is really good.

Important thing is, just as with Ruby, always see if a function exists that does what you are trying to do!

