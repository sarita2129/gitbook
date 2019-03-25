---
description: Super Phaser Bros. No Copyright Infringement Intended.
---

# Phaser

![](../.gitbook/assets/childish_phasers.gif)

**What is it?**

Phaser is graphics library, it's one of the best and most common libraries used for creating games for the browser. It's incredibly powerful, incredibly versatile, and it has [hundreds of examples](https://phaser.io/examples) which are a great stepping stone for learning and creating your own games.

**Getting started**

[_Download me_](https://github.com/Phoboes/phaser_tutorial)

Essentially, I'll be talking you through the process outlined in the [create your first game](https://phaser.io/tutorials/making-your-first-phaser-game/index) tutorial.

Phaser has recently split into 2 different versions, Phaser 2, and Phaser-ce. Phaser CE is a community edition with a little bit less regulation, but it is updated more regularly and has a couple of new features -- most of which you won't notice.

I'm going to use CE for the purposes of this example, but the syntax between the 2 should be identical.

Firstly, we need to set the game canvas up:

```text
var game = new Phaser.Game( 800, 600, Phaser.CANVAS, 'phaser-example', { preload: preload, create: create, update: update } );
```

This will render an 800px x 600px canvas on the screen for us, titled 'phaser-example', and it's going to expect a few functions.

You'll notice that, if you open your page, nothing shows. The reason for this, is that we've told Phaser what we are expecting it to draw, but not told it to render that canvas on the page.

We need to add a few functions to get this going \(the ones we specified in the `game` variable. We will need to add these _above_ the `game` variable \(otherwise we'll run into scoping issues\).

These break down into 3 parts:

_Preload:_

```text
// Compiles assets, used to set graphic resources etc.
var preload = function() {};
```

_Update_

```text
// Loops repeatedly -- in charge of animations etc. This does almost all the grunt work of your app.
var update = function() {};
```

_Create_

```text
// Sets the initial game state -- placing people etc.
var create = function() {};
```

With these lines now added, you should be seeing a big black box on your screen.

**Preloading and assets**

The reason we're staring at a black box is because we've only told Phaser to render a canvas. As a result, it's drawn a massive black canvas \(which is actually quite helpful, a blank canvas is usually white, and invisible on a blank page\).

From here we need to start using the preloader to tell Phaser what image assets it has at its disposal.

```text
function preload() {

  game.load.image( 'sky', '../assets/images/sky.png' );
  game.load.image( 'diamond', '../assets/images/diamond.png' );
  game.load.image( 'firstaid', '../assets/images/firstaid.png' );
  game.load.image( 'platform', '../assets/images/platform.png' );
  game.load.spritesheet( 'baddie', '../assets/sprites/baddie.png', 32, 32 );
  game.load.spritesheet( 'playerModel', '../assets/sprites/dude.png', 32, 48 );

}
```

You might notice that we are loading 2 different asset types here; an image, and a spritesheet. Images are static assets which don't need animating.

Spritesheets are a little different. Spritesheets are long images with slightly different frames along sections of the image. Skipping along evenly spaced blocks of an image, we can generate an animation.

![static sprites](http://i.imgur.com/MSqH1RU.gif)

This is a static sprite sheet, each segment is an evenly spaced frame for animation.

![sprites squares](http://i.imgur.com/OmVLhBl.gif)

Phaser breaks the image into pieces of an array, with each index being a still image. These are the numbers following the sprites. Baddie for instance is 32px high, by 128 frames wide. It has 4 frames, which means each frame is 32 x 32.

![sprite frames animated](https://media.giphy.com/media/26zyZd7jQFh6tbUM8/giphy.gif)

By tying them together \(and reverse/looping\), we end up with a smooth animation, like so:

![sprite tied together](http://i.imgur.com/effKAV5.gif)

Those are the fundamentals of animation through Phaser \(and most things\), and why images need to be treated differently to spritesheets.

You are able to load other assets, things like audio and fots etc, but for the sake of this tutorial -- we'll stick with basics.

**Creating the game**

So, we've got a canvas and loaded our assets. Let's actually make a thing.

Within the `create` function, add the following line:

```text
game.add.sprite( 10, 10, 'diamond' );
```

This will try to draw a diamond on the screen, 10 pixels down and 10 pixels across from the top left corner of the canvas \(0x0\).

Unfortunately, you're probably seeing something like this:

![black box](http://i.imgur.com/swkB6pd.png)

The reason for this is that browsers don't exactly love just loading files from strange computers \(yours included\). A server is necessary to bypass this.

Fortunately, you've all got a python server you can use pre-installed. To launch it, simply type `python -m SimpleHTTPServer` and visit [localhost:8000](http://localhost:8000/) \(by default\).

With that done, hopefully you are seeing something more like this:

![Diamond on black](http://imgur.com/PUDfrIo.jpg)

From here, we may as well continue fleshing out our world. Images are drawn in the order they are listed, so in adding the next line:

```text
game.add.sprite( 0, 0, 'sky' );
```

Ensure it is _before_ the diamond in your code, or the diamond will simply be drawn behind the sky \(therefore invisible forever\).

Next, adding platforms:

```text
  var platforms = game.add.group();

  var ledge = platforms.create( 400, 400, 'platform' );
  ledge = platforms.create( -150, 250, 'platform' );

  var ground = platforms.create( 0, game.world.height - 64, 'platform' );
  ground.scale.setTo( 2, 2 );
```

A group is a means of storing similar characters or items together in the game. The create parameters are the same as the `add` method's, expecting an `( x, y, texture )`, the last of which was provided in the preload. The `ground` variable is treated slightly differently, offset from the bottom of the canvas by its height \(64 pixels\), and doubled in length -- beyond that it is just another platform.

Speaking of groups, a single floating diamond is a little bit out of place. Lets make a few more and start adding our first physics. Remove the original diamond and add the following lines _after_ the platforms:

```text
    diamonds = game.add.group();

    diamonds.enableBody = true;

    //  Here we'll create 12 of them evenly spaced apart
    for ( var i = 1; i < 13; i++ ) {
      //  Create a diamond inside of the 'diamonds' group
      var diamond = diamonds.create( i * 60, 0, 'diamond' );

      //  Give the diamonds a vertical gravitational pull so they fall
      diamond.body.gravity.y = 80;

      //  This just gives each diamond a slightly random bounce value
      diamond.body.bounce.y = 0.3 + Math.random() * 0.2;
    }
```

Amazing, right? Unfortunately, they fall through everything and off the map.

The reason for this is that we haven't told Phaser to care about physics. We'll need to turn them on, and also tell phaser to care about physics on all our objects and groups.

All together, it should look something like this:

```text
var platforms, diamonds;

// Sets the initial game state -- placing people etc.
function create() {

  // We're going to be using physics, so enable the Arcade Physics system
  game.physics.startSystem( Phaser.Physics.ARCADE );

  game.add.sprite( 0, 0, 'sky' );

  // // Grouping similar assets together
  platforms = game.add.group();

  //  We will enable physics for any object that is created in this group
  platforms.enableBody = true;

  var ledge = platforms.create( 400, 400, 'platform' );
  //  This stops it from falling away when you jump on it or another object hits it.
  ledge.body.immovable = true;
  ledge = platforms.create( -150, 250, 'platform' );
  ledge.body.immovable = true;


// Set the ground of the level. In this case, it's just a scaled up platform.
  var ground = platforms.create( 0, game.world.height - 64, 'platform' );
  ground.scale.setTo( 2, 2 );
  ground.body.immovable = true;

  // Add the diamonds to a unique group
  diamonds = game.add.group();
  // Enablebody tells phaser to care about the boundaries of the image. Important for things like collisions.
  diamonds.enableBody = true;

  //  Here we'll create 12 of them evenly spaced apart
  for ( var i = 1; i < 13; i++ ) {
    //  Create a diamond inside of the 'diamonds' group
    var diamond = diamonds.create( i * 60, 0, 'diamond' );

    //  Give the diamonds a vertical gravitational pull so they fall
    diamond.body.gravity.y = 80;

    //  This just gives each diamond a slightly random bounce value
    diamond.body.bounce.y = 0.3 + Math.random() * 0.2;
  }
}
```

Key points:

* `game.physics.startSystem( Phaser.Physics.ARCADE );` -- Tells phaser to start caring about physics
* `platforms.enableBody = true;` -- care about physics and boundaries on this object
* `ground.body.immovable = true;` -- Prevent other objects moving this object on contact \(like if it's hit by a diamond\).
* The variables `platforms` and `diamonds` have been moved _outside_ the create function's scope. This is necessary, as the _update_ function will need access to these variables.

**Speaking of update**

Okay, so, with all this added, there everything is still falling through the world.

While phaser now knows it cares about the physics of things, and items have bodies and boundaries, interaction between them hasn't been specified.

Within the _update_ function, add the following: `game.physics.arcade.collide( diamonds, platforms );`.

With that complete, hopefully you are seeing something more like this:

![Falling diamonds](https://media.giphy.com/media/3o8dFi2bqn44STRbZ6/giphy.gif)

**Finishing touches**

Alright, so placing things in the world is fairly easy. Lets go ahead and add a player.

In _create_ \(down the bottom -- we want the player on top of all scenery\):

```text
    // The player and its settings
    player = game.add.sprite( 32, game.world.height - 150, 'playerModel' );

    //  We need to enable physics on the player
    game.physics.arcade.enable( player );

    //  Player physics properties. Give the little guy a slight bounce.
    player.body.bounce.y = 0.2;
    player.body.gravity.y = 300;

    // Prevents the player from walking off the map.
    player.body.collideWorldBounds = true;

    //  Our two animations, walking left and right.
    player.animations.add( 'left', [0, 1, 2, 3], 10, true );
    player.animations.add( 'right', [5, 6, 7, 8], 10, true );
```

Most of this is straightforward stuff you've already seen, the only interesting thing here is `player.animations.add('left', [0, 1, 2, 3], 10, true)`, particularly the array, which nominates frames for a certain action \(in this case, moving left\). Phaser does support simply flipping the image, you don't actually _need_ the right hand frames, but in the interests of simplicity, we currently have both.

From here, the player is currently standing underground. To fix this, head to _update_ and specify that you want the platforms and the player to interact with one another, just as we've done with the diamonds and platforms.

```text
var hitPlatform = game.physics.arcade.collide( player, platforms )
```

In this instance, it's been stored as a variable -- this will be used later to determine when the player can jump \(jumping while jumping is really just flying, after all\).

Really though, that's all the drawing done. Yay us.

**Interactivity**

So, this is really pretty and all, but... it's really boring. You can't _do_ anything. Time to fix that.

Typically this is where you would need to start adding event listeners to keypresses and go from there. Fortunately, Phaser has this built in.

Within _update_:

```text
cursors = game.input.keyboard.createCursorKeys();
```

Cursors will store any keypress detected in the window, which can be used to move the player's avatar around, which will look something like this \(in update still\):

```text
  // Resets the player's velocity to nothing by default. This is then changed if a keypress is detected.
  player.body.velocity.x = 0;

  if ( cursors.left.isDown ){
      //  Move to the left
      player.body.velocity.x = -150;
      player.animations.play( 'left' );
  } else if ( cursors.right.isDown ) {
      //  Move to the right
      player.body.velocity.x = 150;
      player.animations.play( 'right' );
  } else {
      //  Stand still
      player.animations.stop();
      player.frame = 4;
  }
```

And also the jump command we prepared for earlier:

```text
  //  Allow the player to jump if they are touching the ground.
  if ( cursors.up.isDown && player.body.touching.down && hitPlatform ){
      player.body.velocity.y = -350;
  }
```

The only things you need to really know about the velocities here are that they work on a 4 way axis \(y being up and down, x being left and right\) and are based on the object's \(player in this case\) position. 0 velocity is static, -150 is moving left on the screen.

**Component interactions**

The last thing I'm going to add here is the ability to collect diamonds.

Repeating the same process as above, I will need to:

* Tell Phaser to listen for diamonds and the player interacting
* Have a function that removes _that_ diamond
* Have a way of tracking my score

Within update: `game.physics.arcade.overlap( player, diamonds, getDiamond, null, this );`

This line listens for an overlap between the player, and members of the `diamonds` group. When this happens, it calls a function called `getDiamond`.

Define `getDiamond` in the _global_ namespace \(make sure it is _not_ locked in a function\), preferrably up the top of your file. It will take 2 parameters, the player and the diamond:

```text
var getDiamond = function( player, diamond ) {
  // Removes the star from the screen
  diamond.kill();
};
```

While you are up there, add 2 more global variables:

```text
var score = 0;
var scoreText;
```

It might also be worth moving your other globals to the top while you are at it:

```text
var scoreText, cursors, platforms, diamonds
```

Now head back down to _create_ and define `scoreText`: `scoreText = game.add.text( 16, 16, 'score: 0', { fontSize: '32px', fill: '#000' } );`

This should look familiar by now. All it's doing is drawing some black text, slightly offset from the top of the screen, starting as "score: 0". _Make sure you add this line after_ drawing the sky

Now, for the _very_ last touch -- inside `getDiamond` add the following lines:

```text
//  Add and update the score
score += 10;
scoreText.text = 'Score: ' + score;
```

