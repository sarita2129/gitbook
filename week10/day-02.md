---
description: 'To iterate is human, to recurse divine'
---

# Day 02

What we covered today:

* [Recursion](https://github.com/wofockham/wdi-30/tree/master/13-advanced/recursion)
* [Vue.js](https://github.com/wofockham/wdi-30/tree/master/13-advanced/vue-intro)
* [Node.js](https://github.com/wofockham/wdi-30/tree/master/13-advanced/node-intro)

Warmup

* [Happy Numbers](https://github.com/Yiannimoustakas/wdi30-homework/tree/master/warmups/week10/day02_happy_numbers)

## Recursion

Recursion in computer science is a method where the solution to a problem depends on solutions to smaller instances of the same problem \(as opposed to iteration\). Recursive algorithms have two cases: a recursive case and base case. Any function that calls itself is recursive.

Create a new folder and add a new ruby file into it.

```bash
mkdir recursion
cd recursion
touch countdown.rb
atom .
```

Iterative method:

```ruby
def countdown_i (n=10)
  while n >= 0
    puts n
    n -= 1
    sleep 1
  end
​
  puts "Blast off!"
​
end
​
require 'pry'
binding.pry
```

Call the method

```bash
ruby countdown.rb
countdown_i
```

Recursive method:

```ruby
def countdown_r (n=10)
  if n < 0 #Base case
    puts "Blast off"
  else
    puts n
    sleep 1
    countdown_r n-1 # Recursive case which moves us closer to the case
  end
end
​
require 'pry'
binding.pry
```

Call the method

```bash
ruby countdown.rb
countdown_r
```

Some things are better solved with recursion like factorials.

```bash
5! = 5 x 4 x 3 x 2 x 1
```

Create a new file.

```bash
touch factorial.rb
```

Iterative method:

```ruby
def factorial_i(n)
  result = 1
  while n > 1
    result = result * n # Mutation
    n -= 1 # Move closer to the base case
  end
  result
end
​
require 'pry'
binding.pry
```

Call the method

```ruby
ruby factorial.rb
factorial_i 5
```

Recursive method:

```ruby
def factorial_r(n)
  if n > 1 # Recursive case
    n * factorial_r(n -1)
  else
    1 # Base case
  end
end
​
require 'pry'
binding.pry
```

Call the method

```ruby
factorial.rb
factorial_r 5
```

Create another file. Fibonacci

```bash
touch fibonacci.rb
```

Iterative method:

```ruby
def fibonacci_i(n)
  a = 1
  b = 1
​
  while n > 1
    a, b = b, a + b  # Parallel assignment: a = b; b = a + b
    n -= 1
  end
  a # The final result will be in a
end
​
require 'pry'
binding.pry
```

Recursive method:

```ruby
# Very slow
def fibonacci_r(n)
​
  if n <= 2
    1 # Base case
  else
    fibonacci_r(n - 1) + fibonacci_r(n - 2)
  end
end
​
​
def factorial_r(n)
  if n > 1 # Recursive case
    n * factorial_r(n -1)
  else
    1 # Base case
  end
end

```

This example is extremely slow because we call the method twice with each iteration. This will exponential increase, doubling the If you call `fibonacci_r(100)` it looks for `fibonacci_r(98) + fibonacci_r(99)`. The problem is that it will then call `fibonacci_r()` twice for each of those `fibonacci_r(97) + fibonacci_r(98)`. As you can see you will be calling `fibonacci_r(98)` twice and create two copies of the branch even though you have already figured that out.

## Vue Intro

* [Guide](https://vuejs.org/v2/guide/)
* [Class Demo](https://github.com/wofockham/wdi-30/tree/master/13-advanced/vue-intro)



## Node.js <a id="node-js"></a>

​[Node](https://nodejs.org/en/) is an open-source, cross-platform JavaScript engine that's mostly used to create web servers using JavaScript. It was written in 2009 by Ryan Dahl while he was working at Joyent.

### JavaScript engines​ <a id="javascript-engines"></a>

A JavaScript engine is a compiler that turns JavaScript into machine code for microprocessors - typically, it will turn it into things like:

* **IA32** - Intel Architecture 32 bit
* **x86-64** - A 64 bit version of the Intel Architecture
* **ARM** - Acorn Reduced Instruction Set Computing \(RISC\) Machine
* **MISP ISAs** - Microprocessor without Interlocked Pipeline Stages Instruction Set Architecture

​Here are a few JavaScript engines: ​

* **SpiderMonkey** - Firefox, Netscape Navigator - the first JavaScript Engine
* **V8** - Chrome, Opera
* **Chakra** - Internet Explorer
* **Nitro** - Safari

**Node** is the V8 JavaScript engine, taken out of a browser, with a little bit extra. It lets us write server-side code in JavaScript, allowing us to do the same sorts of things we do with Ruby.

## _NodeJS - JavaScript Engines - Further Reading_ <a id="nodejs-javascript-engines-further-reading"></a>

* ​[Wikipedia - JavaScript Engines](https://en.wikipedia.org/wiki/JavaScript_engine#JavaScript_engines)​

## Why NodeJS? <a id="why-nodejs"></a>

### _What is Node good at?_ <a id="what-is-node-good-at"></a>

* Streaming data
* "Soft" real-time
* Every developer knows a bit of JS
* "Tooling"
* Unopinionated and flexible
* It can be isomorphic
* Corporate backing
* Performant
* Good community - easy to find developers

### _What is Node not so good at?​_ <a id="what-is-node-not-so-good-at"></a>

* "Tooling" - too much out there
* Being opinionated
* Memory leaks
* Dependency injection
* Serving static files \(like HTML\)
* Providing structure
* Generic JavaScript woes \(callbacks, etc.\)
* Allowing easy debugging
* Producing light-weight codebases
* Providing functionality out of the box
* Getting away from fancy buzzwords

## Installation and Usage <a id="installation-and-usage"></a>

### _Installation_ <a id="installation"></a>

```bash
# Initial installation using Homebrew
brew install node
# Upgrading installed version using Homebrew
brew upgrade node
```

### _Usage_ <a id="usage"></a>

```bash
# Open a Node REPL
node
# Run a Node application
node someFileName.js
```

## _NPM - Introduction_ <a id="npm-introduction"></a>

### Usage <a id="usage-1"></a>

We haven't gotten into Webpack yet, but this is an example of how we would create a new NodeJS application.

### _Initialize an NPM project_ <a id="initialize-an-npm-project"></a>

Running this command will create a package.json file for your NodeJS application. This describes your application, and allows NPM to manage your application's dependencies.

```bash
npm init
```

We can also skip the 'wizard'-style Q&A process with the -y flag.

```bash
npm init -y
```

### _Install packages_ <a id="install-packages"></a>

This will install Webpack and all its dependencies. The --save flag will also add webpack to your package.json.

```bash
npm install webpack --save
```

### _Update packages_ <a id="update-packages"></a>

Periodically, you should update the packages on which your application depends. This will update all local packages in your package.json file \(and all their dependencies\).

```bash
npm update
```

### _Further Reading_ <a id="further-reading"></a>

* ​[NPM](https://www.npmjs.com/)​
* ​[NPM - Introduction](https://docs.npmjs.com/getting-started/what-is-npm)​
* ​[NodeSource - Understanding NPM](https://nodesource.com/)​
* ​[NPM Dependency Tree - Example: ExpressJS](http://npm.anvaka.com/#/view/2d/express)​

## Creating a simple Node server <a id="creating-a-simple-node-server"></a>

Create a new directory and cd into it then open in Atom.

```bash
mkdir simple-node-server
cd simple-node-server/
touch index.js
atom .
```

Add console log into the `index.js` file you've just created.

```javascript
console.log(`hello world`);
```

Run the program by using the below code in the terminal. Make sure you're in the `simple-node-server/` directory.

```bash
node index.js
```

You should be able to see the console log in the terminal `hello world.`

Go back to the `index.js` file and remove the console log as it was only used to make sure we have a connection. Define a variable and require 'http'

```javascript
const http = require('http');
```

Create a connection to the server with a 'fat arrow' function that way we can retain the value of `this`.

```javascript
http.createServer( (request, response) => {
​
});
```

This is a direct comparison of the old way of writing the function.

```javascript
http.createServer( function(request, response){
​
});
```

Because node is unopinionated we need to specify our requests and an end to the response.

```javascript
http.createServer( (request, response) => {
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
});
```

We also have to tell the server to start with a 'listen' promise chained to the end of the function. This will watch port 8000.

```javascript
http.createServer( (request, response) => {
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
}).listen(8000);
```

Unfortunately, Node doesn't log anything to the console so we have no way of knowing what's happening. We can get around this by placing console logs in our code.

```javascript
const http = require('http');
​
http.createServer( (request, response) => {
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
}).listen(8000);
​
console.log('Server now running at http://localhost:8000');
```

Now go back to the terminal an run the server

```javascript
node index.js
```

Add some additional logs to let us know when someone is trying to request a page.

```javascript
http.createServer( (request, response) => {
  console.log( 'Page requested');
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
}).listen(8000)
```

### Further Reading <a id="further-reading-1"></a>

* ​[NodeJS](https://nodejs.org/docs/latest-v9.x/api/synopsis.html)​

  


