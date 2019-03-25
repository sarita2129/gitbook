---
description: 'Express.js: Fast, unopinionated, minimalist, just like my sex life'
---

# Day 04

What we covered today:

* [Express](https://github.com/wofockham/wdi-30/tree/master/14-node/express-intro) 
* [Stupid Front End Tricks](https://github.com/wofockham/wdi-30/tree/master/13-advanced/stupid_frontend_tricks)

Warmup

* [Ajax Proxy](https://github.com/Yiannimoustakas/wdi30-homework/tree/master/warmups/week10/day04_ajax_proxy)

### Express

Express is a light-weight web application framework to help organise your web application into an MVC architecture on the server side. You can use a variety of choices for your templating language \(like EJS, Jade, and Dust.js\). EJS would be a similar syntax to what we're use to with Rails erb.html files.

You can then use a database like MongoDB with Mongoose \(for modeling\) to provide a backend for your Node.js application. Express.js basically helps you manage everything, from routes, to handling requests and views.

### _Installing Express_ <a id="installing-express"></a>

Navigate to the root directory of your project and install express.

```bash
npm install --save express
```

`require` function is the easiest way to include modules that exist in separate files. The basic functionality of `require` is that it reads a javascript file, executes the file, and then proceeds to return the exports object.

```javascript
const express = require('express');
const app = express();

const server = app.listen(3000, () => {
  console.log("Server listening on port 3000...");
});
```

Now run `node server.js` from the terminal,  we should be able to get the following output:

```javascript
ba-express-server-no-db$ node server.js
Server listening on port 3000...
```

### Enable server restart on file changes <a id="enable-server-restart-on-file-changes"></a>

Any changes you make to your Express website are currently not visible until you restart the server. It quickly becomes very irritating to have to stop and restart your server every time you make a change, so it is worth taking the time to automate restarting the server when needed. One of the easiest such tools for this purpose is [nodemon](https://github.com/remy/nodemon). This is usually installed globally \(as it is a "tool"\), but here we'll install and use it locally as a developer dependency, so that any developers working with the project get it automatically when they install the application. Use the following command in the root directory for the skeleton project:

```bash
npm install --save nodemon
```

## Stupid Front-End Tricks

```text
mkdir stupid
cd stupid/
touch index.html
mkdir css
mkdir js
touch css/stupid.css
touch js/stupid.js
curl http://code.jquery.com/jquery-1.12.4.js > js/jquery.js
atom .
```

Open the index.html file and add the below code. Remember you can use emmet on the `p*10>lorem`line to populate the paragraph tags with lorem ipsum. We will link the two script tags and the css file.

```text
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="stylesheet" href="css/stupid.css">
  <script src="js/jquery.js" charset="utf-8"></script>
  <script src="js/stupid.js" charset="utf-8"></script>
  <title>Stupid Front-End Tricks</title>
</head>
<body>

  <div class="container">
    <h1>Stupid Fron End tricks</h1>
    p*10>lorem

    <div class="bill">

    </div>
    p*10>lorem

  </div>

</body>
</html>
```

Add some styling to the CSS file.

```text
body {
  background-image: url(http://placekitten.com/1024/1025);
  background-size: cover;
}

.container {
  max-width: 960px;
  margin: 0 auto;
  background-color: rgba(255, 255, 255, 0.6);
  padding: 2em;
  box-sizing: border-box;
  font-size: 140%;
}

.bill {
  height: 10em;
  background-image: url(http://fillmurray.com/900/1200);
  background-size: cover;
}
```

Now we want to start doing something with the image of Bill Murray. Select the `div` with the class of `.bill` and save it in a variable. Remember the naming convention when selecting an element with `jquery` we start it with a `$`. Then we also need to place an event listener on the `window` that will be waiting for a scroll event.

```text
$(document).ready(function(){
  // Parallax effects
  const $bill = $('.bill');

  $(window).on('scroll', function(){
    console.log(`scroll`);
  });
});
```

Run the code, open the console and check to see that we're getting a console log with the string of `scroll`.

We now want to get the scrollTop and save that in a variable.

```text
$(document).ready(function(){
  // Parallax effects
  const $bill = $('.bill');

  $(window).on('scroll', function(){
    const scrollTop = $(window).scrollTop();
    console.log(scrollTop);
  });
});
```

Open the console and scroll the page to make sure you have the correct values outputting.

Now we want to make the image of Bill Murray move at a different pace to the body when we scroll.

```text
$(window).on('scroll', function(){
  const scrollTop = $(window).scrollTop();
  $bill.css('background-position-y', -scrollTop / 3);
});
```

We can use the same technique to change the scroll speed of the `background-image`.

```text
$(document).ready(function(){
  // Parallax effects
  const $body = $('body');
  const $bill = $('.bill');

  $(window).on('scroll', function(){
    const scrollTop = $(window).scrollTop();
    $bill.css('background-position-y', -scrollTop / 3);
    $body.css('background-position-y', -scrollTop * 0.75);
  });
});
```

This is all well and good but what this page now needs is BUBBLES! Create another event listener that will watch out for when the mouse has been moved of the page.

```text
$(document).ready(function(){
  // Parallax effects
  const $body = $('body');
  const $bill = $('.bill');

  $(window).on('scroll', function(){
    const scrollTop = $(window).scrollTop();
    $bill.css('background-position-y', -scrollTop / 3);
    $body.css('background-position-y', -scrollTop * 0.75);
  });

  $(window).on('mousemove', function(e){
    console.log( e );
  });
});
```

Open the console and have a look at the object that is getting logged out. If you open the object you will see all the event information that is being recorded. ALl was care about is the `pageX` and `pageY`values.

Lets use those values to place some bubbles on the page. Define what a bubble is and append it to the body element.

```text
$(window).on('mousemove', function(e){
  // const x = e.pageX;
  // const y = e.pageY;
  // same as below
  const {pageX: x, pageY: y} = e; // Destructuring

  const $bubble = $('<div class="bubble"></div>').css({
    left: x,
    top: y
  }).appendTo($body);
});
```

This will create the bubbles but they have no css so we cant see them. We need to add some more CSS to the `style.css` file.

```text
.bubble {
  position: absolute;
  width: 10em;
  height: 10em;
  border: 1px solid aliceblue;
  border-radius: 100%;
}
```

Now when you mouse around they will appear on the page.

Bubbles are as unique as a snowflake so we don't want them to have the same size each time they are populated on the page. Lets add some randomisation.

```text
$(window).on('mousemove', function(e){
  // const x = e.pageX;
  // const y = e.pageY;
  // same as below
  const {pageX: x, pageY: y} = e; // Destructuring

  const size = (Math.random() * 10) + 'em';

  const $bubble = $('<div class="bubble"></div>').css({
    left: x,
    top: y,
    width: size,
    height: size
  }).appendTo($body);
});
```

Looking pretty cool! But don't bubbles float?

Lets get our bubble to move to the top of the page.

```text
$(window).on('mousemove', function(e){
  // const x = e.pageX;
  // const y = e.pageY;
  // same as below
  const {pageX: x, pageY: y} = e; // Destructuring

  const size = (Math.random() * 10) + 'em';

  const $bubble = $('<div class="bubble"></div>').css({
    left: x,
    top: y,
    width: size,
    height: size
  }).appendTo($body);

  $bubble.animate({ top: -200 }, 2000);
});
```

They're now looking and acting like real bubbles but if you open the `elements` tab in the console you'll see that they're just bunching up at the top and using all our memory. Lets fix that!

There are a couple of ways that we could approach this: We could add a set time out and remove the bubbles from the page once the timer reaches the end.

```text
$(window).on('mousemove', function(e){
  // const x = e.pageX;
  // const y = e.pageY;
  // same as below
  const {pageX: x, pageY: y} = e; // Destructuring

  const size = (Math.random() * 10) + 'em';

  const $bubble = $('<div class="bubble"></div>').css({
    left: x,
    top: y,
    width: size,
    height: size
  }).appendTo($body);

  $bubble.animate({ top: -200 }, 2000);

  setTimeout(function(){
    $bubble.remove();
  }, 2000);
});
```

But this means you would have to change two timers. First in the `setTimeOut` but also in the `animate`function.

A better method would be to add another function to the end of your animation, as a second argument, and remove the bubble.

```text
$bubble.animate({ top: -200 }, 2000, function(){
  $bubble.remove(); //Clean up after ourselves to conserve memory.
});
```

