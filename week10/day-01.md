---
description: '/ bb | [^b] {2} /, that is the question...'
---

# Day 01

What we covered today:

* [Regular expressions](https://github.com/wofockham/wdi-30/tree/master/0x-bonus/regular-expressions)
* [Rspec On Rails](https://github.com/wofockham/wdi-30/tree/master/09-tdd/fruitstore)

### Warmup <a id="warmup"></a>

* [Prime Factors](https://github.com/Yiannimoustakas/wdi30-homework/tree/master/warmups/week10/day01_prime_factors)

## Regular Expressions I <a id="regular-expressions-i"></a>

Regular Expressions are pattern matchers for text. Sounds relatively dry \(or very, very dry\) but they are incredibly powerful. They are often considered a write-only language. You can write it, but it is virtually impossible to read. They occur in lots and lots of programming languages \(they are even in javascript\), and if you are interested in the history of them - see [here](https://en.wikipedia.org/?title=Regular_expression#History) - but the way that Ruby works with them stems from the Perl programming language.

### So, how do we work with them in Ruby? <a id="so-how-do-we-work-with-them-in-ruby"></a>

#### _Literal Characters_ <a id="literal-characters"></a>

We can match literal characters in virtually the same way as we would with two equals signs. Instead of using the two equals signs though, we use `=~` , think of this as a fuzzy or dynamic search. To specify a regular expression in Ruby \(and most other languages\), we use `//` \(these are considered Regexp delimiters\). Let's have a muck around with them.

```ruby
"bob" == "bob" # This obviously returns true.
​
"bob" =~ /bob/
# This is true, they do match. But instead of returning true, it returns the index of where the pattern started to match. In this case, it returns 0.  It returns nil if it doesn't match anything.
​
"Hello, my name is bob" =~ /bob/
# Again, this is true, but returns 18
```

Obviously this isn't the powerful part though, considering this does virtually the same thing as the multiple equals signs. So let's get into some of the cooler stuff.

#### _Metacharacters_ <a id="metacharacters"></a>

In regular expressions, there are characters that mean far more than literal characters. This can prove problematic \(but there are ways to escape metacharacters\), but also the source of some of the most powerful parts.

```ruby
"o!o" =~ /o.o/ # Returns 0
```

#### _Character Classes_ <a id="character-classes"></a>

These are often used to check for capitalized and uncapitalized versions, but can also be used to check for multiple letters. The metacharacters for these are `[]`.

```javascript
"bob" =~ /[Bb]ob/  # Returns 0
"Bob" =~ /[Bb]ob/  # Returns 0
"cob" =~ /[Bbc]ob/ # Returns 0
"dog" =~ /[Bb]ob/  # Returns nil
"any vowels" =~ /[aeiou]/ # Returns 0
```

For this sort of stuff, we can also use ranges. These are quite useful.

```javascript
"A" =~ /[A-Z]/      # Returns 0
"a" =~ /[A-Z]/      # Returns nil
"a" =~ /[A-z]/      # Returns 0
```

There are lots of inbuilt ranges, and these are really useful to know. Some of them are...

* `\s` - Will match any space characters \(spaces, new lines etc.\)
* `\S` - Anything other than space characters
* `\w` - Will match any word character \(i.e. actual letters or numbers\)
* `\W` - Will match any non-word character

```ruby
"Wolf" =~ /[\w]/    # Returns 0
"Wolf" =~ /[\W]/    # Returns nil
"Wolf " =~ /[\W]/   # Returns 4
​
"Wolf " =~ /[\s]/   # Returns 4
"Wolf " =~ /[\S]/   # Returns 0
```

We can obviously add lots and lots of things between those square brackets. But there are other ways we can do this as well. We can use `()` and `|` to check for multiple things.

```ruby
"Jane" =~ /(Jane|Serge)/    # Returns 0
"Serge" =~ /(Jane|Serge)/   # Returns 0
```

The round brackets are metacharacters in Regexp as well! They are a way to say this or this \(or this or this or this\). The pipe is the delimiter for saying "or".

There are a lot more metacharacters. The `.`, for example, is a wildcard, it will match anything.

```ruby
"jane" =~ /.ane/            # Returns 0
"zane" =~ /.ane/            # Returns 0
"Serge and Jane" =~ /.ane/  # Returns 10
```

This is still relatively hardcoded though, we need to specify where the characters are and what they are. To help solve this problem, there are quantifiers.

#### _Quantifiers_ <a id="quantifiers"></a>

Quantifiers are a way to check in Regexp whether things exist, or exist more than once etc.

The ones that you will actually use:

* `+` means one or more
* `?` means one or zero
* `*` means zero or more

It can look like the following:

```ruby
"Hi there" =~ /i+/      # Returns 1
"Hi there" =~ /the?/    # Returns 3
"Hi theeere" =~ /the*/  # Returns 3
```

These are regularly used for existence checks.

#### _Capturers_ <a id="capturers"></a>

These are difficult to understand. Basically, you match something in parentheses and can refer back to them. You capture something by using round brackets, and refer back to them using a `\` and an integer \(that mimics the order of capture\).

```ruby
"WolfWolf" =~ /(....)\1/
# Matches any four characters, but then needs to have the same four characters straight after.  This returns zero.
​
"ArcticWolf ArcticWolf" =~ /(......)(....) \1\2/
# Matches "Arctic" in the first brackets, then "Wolf" in the second brackets.  "Arctic" is saved as \1 and "Wolf" is saved as \2
```

#### _Regex Cheatsheet_ <a id="regex-cheatsheet"></a>

* ​[Cheatsheet](https://www.ralfebert.de/snippets/ruby-rails/regex_cheat_sheet/)​



## Ruby on Rails - Testing - Rspec <a id="ruby-on-rails-testing-rspec"></a>

* ​[Rspec-rails documentation](https://github.com/rspec/rspec-rails)​

We have done a little bit of Rspec now, but it has all been with pure Ruby, no Rails. We want to get it working with Rails, and luckily, that isn't difficult.

### Set up a Rails project <a id="set-up-a-rails-project"></a>

Start by creating a new Rails project, using the `-T` option to tell Rails _not_ to generate tests for us.

```bash
rails new fruitstore -T --skip-git
cd fruitstoreatom .
```

Then, include the Rspec gem in the test and development group in our **GEMFILE**:

```bash
gem 'rspec-rails'
```

Then bundle the gems.

```bash
bundle
```

### Install Rspec in the project <a id="install-rspec-in-the-project"></a>

This still hasn't installed Rspec in our project. To do that, run the following command from Terminal:

```bash
rails generate rspec:install
```

This does a bunch of things, it generates us a spec folder \(where all of our tests will live\), but it also changes the behaviour for other rails generators. For example, when you generate a model, the generator will now add a test for that model. Same for controllers, etc, generated using a generator.

### Create a model <a id="create-a-model"></a>

```bash
rails generate model Fruit
rails db:migrate
```

Go back to the terminal and run `rspec` and you will see it is now connected and working.

We're going to create a single string column for the name attribute of a Fruit object.

```ruby
rails generate migration add_name_to_fruits name:string
rails db:migrate
```

#### _Testing Single Table Inheritance_ <a id="testing-single-table-inheritance"></a>

The whole purpose of this project is to make it easy to upload Fruits, whether they're apples or pears. We could have multiple tables for these things, but we don't have to. We can use a thing called Single Table Inheritance \(S.T.I\). This is where a model \(that doesn't have a table\) inherits from another model \(that has a database table associated with it\). It looks something like the following:

```ruby
# In app/models/fruit.rb
class Fruit < ActiveRecord::Base
end
​
# In app/models/apple.rb
class Apple < Fruit
end
​
# In app/models/pear.rb
class Pear < Fruit
end
```

Because Pear and Apple inherit from a Fruit, and Fruit inherits from ActiveRecord::Base, Pear and Apple also inherit from ActiveRecord::Base.

This structure of inheritance means that we can create Fruits, Apples or Pears and it still just makes a record in the Fruits table. Example:

```ruby
pear = Pear.new
apple = Apple.new
fruit = Fruit.new
```

First, thought, we need to a `type` column to our fruits table.

```ruby
rails generate migration add_type_to_fruits type:string
rails db:migrate
```

To test this out we could have a block in our Rspec tests.

```ruby
Rspec.describe Shelf, :type => :model do
  describe "A pear" do
    before do
      @pear = Pear.create :name => "Nashi"
    end
​
    it "should remember the class via Single Table Inheritance (S.T.I)" do
      pear = Fruit.find( @pear.id )
      expect( pear ).to_not be_nil
      expect( pear.class ).to eq Pear
      expect( pear ).to eq @pear
      expect( pear.is_a?( Fruit ) ).to be true
      expect( pear.class.ancestors ).to include Fruit
    end
  end
end
```

And that should be all sorted! Single Table Inheritance doesn't regularly come up so if it is seeming a bit weird, don't worry about it. Neither Joel nor I have ever used it professionally. But it is cool to show how to test that sort of stuff, and it shows why type is a reserved column name in a database.

#### _Testing Associations_ <a id="testing-associations"></a>

One thing that you might be interested in, is testing associations in Rails. Unfortunately Rspec doesn't have something that automates this. To test associations, use a gem called [Shoulda Matchers](https://github.com/thoughtbot/shoulda-matchers).

Add the following to the test environment block in your **GEMFILE**:

```bash
gem 'shoulda-matchers'
```

Then bundle your gems:

```bash
bundle
```

Now you need to tell the gem a couple of things:

* Which test framework you're using
* Which portion of the matchers you want to use

You can supply this information by using a configuration block. Place the following in `rails_helper.rb`:

```ruby
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
```

Now let's create a shelf table for our Fruit, and set up the associations

```ruby
rails generate model Shelf
rails db:migrate
```

Now, in **app/models/shelf.rb**:

```ruby
class Shelf < ActiveRecord::Base
  has_many :fruits
end
```

And, in **app/models/fruit.rb**:

```ruby
class Fruit < ActiveRecord::Base
  belongs_to :shelf, :optional => true
end
```

To test these associations...

```ruby
# Put this at the top of the thing describing fruits
it { should belong_to :shelf }
​
# Put this at the top of the thing describing shelves
it { should have_many :fruits }
```

### Testing controllers <a id="testing-controllers"></a>

We have only tested models so far, so lets figure out how to test controllers. We need to add another gem for controller testing:

* ​[rails-controller-testing](https://github.com/rails/rails-controller-testing)​

```bash
gem 'rails-controller-testing'
```

```bash
bundle
```

Let's generate a fruits controller and set up our routes:

```bash
rails generate controller Fruits
```

Now, in our **config/routes.rb** file, add restful resources for the fruits controller:

```ruby
resources :fruits, only => [:index]
```

The next error that pops up will be that the action doesn't exist, so lets add an index method in the **app/controllers/fruits\_controllerrb**:

```ruby
class FruitsController < ApplicationController
  def index
  end
end
```

After that, we will have a missing template \(or view\), so let's make that file as well.

```bash
touch app/assets/views/fruits/index.html.erb
```

Now lets add some tests to our **fruits\_controller\_spec.rb** file.

```ruby
RSpec.describe FruitsController, type: :controller do
  describe "GET to index" do
    before do
      3.times { |i| Fruit.create :name => "Fruit number #{ i }" }
    end
​
    describe "As HTML" do
      before do
        get :index # Fake browser
      end
​
      it "should respond with a status of 200" do
        expect( response ).to be_success
        expect( response.status ).to eq( 200 )
      end
​
      it "should respond with @fruits that contains all fruits in reverse order" do
        # We need to do a bit of work in the controllers for this.  Add @fruits = Fruit.all.reverse into the index method.
​
        # Remember that this all runs of the before block.
​
        # Checks existence
        expect( assigns( :fruits ) ).to be
​
        # Checks how many things are in @fruits
        expect( assigns( :fruits ).length ).to eq 3
​
        # Make sure the class is correct
        expect( assigns( :fruits ).first.class ).to eq( Fruit )
​
        # Validates whether it is in reverse order.
        expect( assigns( :fruits ).first.name ).to eq( "Fruit number 2" )
      end
​
      it "should render the index view" do
        expect( response ).to render_template( :index )
      end
    end
  end
end
```

#### _Testing JSON responses_ <a id="testing-json-responses"></a>

That will work for normal HTML, but what if we wanted to test JSON responses? Lets add some tests.

```ruby
describe "As JSON" do
  before do
    get :index, :format => :json
  end
​
  it "should response with a status 200" do
    expect( response ).to be_success
    expect( response.status ).to eq(200)
  end
​
  it "should have the right content type" do
    expect( response.content_type ).to eq( "application/json" )
  end
​
  it "should include the fruit data in JSON format" do
    # response.body returns a string so we need to turn it into an actual object
    fruit = JSON.parse( response.body )
    expect( fruits.length ).to eq( 3 )
    expect( fruits.first['name'] ).to eq( "Fruit number 2" )
  end
end
```

In the index method in **app/controllers/fruits\_controller.rb**, lets add this

```ruby
def index
  @fruits = Fruit.all.reverse
  # @fruits = Fruit.order('id DESC') will actually use less memory as it doesn't need to create a duplicate version of the results and reverse it.
  respond_to do |format|
    format.html {}
    format.json { render :json => @fruits }
  end
end
```

That should work out the errors!

_A Bit more on Running Rspec_

* `rspec` - Run all tests
* `rspec spec/models` - Run all model tests
* `rspec spec/controllers` - Run all controller tests
* `rspec -e "The description of the test you want to run"` - Tell it an individual test to run.

### Rspec terminal commands <a id="rspec-terminal-commands"></a>

Here are a few of the common ones:

```ruby
rspec # Run all tests
rspec spec/models  # Run all model tests
rspec spec/controllers  # Run all controller tests
rspec -e "The description of the test you want to run"  # Tell Rspec to run a particular test
```

### _Ruby on Rails - Testing - Rspec - Further Reading_ <a id="ruby-on-rails-testing-rspec-further-reading"></a>

* ​[Rspec](http://rspec.info/)​
* ​[Rspec Rails - Documentation](http://rspec.info/documentation/3.5/rspec-rails/)​
* ​[Better Specs](http://betterspecs.org/)​
* ​[Team Treehouse - An Introduction to Rspec](http://blog.teamtreehouse.com/an-introduction-to-rspec)​
* [How to Test Rails Models with RSpec](https://semaphoreci.com/community/tutorials/how-to-test-rails-models-with-rspec)
* [9 Benefits of Test Driven Development](https://www.madetech.com/blog/9-benefits-of-test-driven-development)
* [Why TDD](https://builttoadapt.io/why-tdd-489fdcdda05e)

