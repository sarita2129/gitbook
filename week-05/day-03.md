---
description: Now to throw away everything you learned about sqlite3. PostgresQL is here
---

# Day 03

What we covered today:

* PostgreSQL - Install
* Ruby on Rails - Migrations
* Ruby on Rails - Generators
* Ruby on Rails - Associations I

**Warmup**

* [Atbash Cipher - Ruby](https://github.com/liaa2/wdi30-homework/tree/master/warmups/week05/day03_atbash_cipher)

#### Classwork

* [MoMA](https://github.com/wofockham/wdi-30/tree/master/08-rails/moma)



## PostgreSQL - Install

**Installing PostgreSQL**

1. [Download Postgres](http://www.postgresapp.com/).
2. Drag **Postgres** from your Downloads folder to your Applications folder.
3. Configure your $PATH to allow access to Postgres CLI tools
   1. Open your bash profile.

      ```bash
      atom ~/.bash_profile
      ```

   2. Add this line to your bash profile.

      ```bash
      export PATH=$PATH:/Applications/Postgres.app/Contents/Versions/latest/bin
      ```
4. Open an new window in your terminal. This will allow the change to take effect.
5. Install the Postgres Gem - the Ruby interface for PostgreSQL

   ```bash
   gem install pg
   ```

6. Confirm that Postgres has been installed and your $PATH configured properly:

   ```bash
   postgres #This should return:
   # => postgres does not know where to find the server configuration file.
   # => You must specify the --config-file or -D invocation option or set the PGDATA environment variable.
   ```

7. Open Postgres - type 'Postgres' into Spotlight Search and open it up. Close the dialog box that opens up - you should then see the silhouette of an elephant in your Mac's menu bar.

### **Using Postgres**

Generally, you should use PostgreSQL for your Rails applications - it is far more scalable, pretty much just as easy to set up as any other database and works on Heroku \(where we will be hosting our applications during the course\). To start a Rails application with a postgreSQL database, run:

```bash
rails new app_name -T --skipt-git --database=postgresql
```

or simply

```bash
rails new app_name -T --skipt-git -d postgresql
```

## Ruby on Rails - Migrations

Migrations are a way to modify your database schema using Ruby code. Using migrations, we can:

* Avoid writing SQL by hand;
* Change our database schema progressively over time;
* Roll back to earlier versions of our database schema;
* Rebuild our database schema from scratch at any time.

Note: Generally, we use migrations for changes to the database schema, not the data in a database; ie, to modify the tables in the database, not to modify the records in the database.

There are a few terminal commands that will generate a migration. These include:

```bash
rails generate model Kitten
rails generate migration add_species_to_kitten
```

Migrations are stored in the db/migrate directory and the filenames are prefixed with a timestamp representing the actual time the migration file was generated, eg: **20160706022128\_create\_kittens.rb**. Rails runs migrations according to the order of their timestamps, so it's important that you do NOT edit the filename of a migration.

We most commonly use migrations to:

1. Create a table in our schema;
2. Modify a table in our schema.

### **Migrations to create a table**

Creating a model using `rails generate model` will create a migration for creating the table for that model:

```bash
rails generate model Kitten
# => create    db/migrate/20160706031227_create_kittens.rb
```

The migration file will look something like this:

```ruby
class CreateKittens < ActiveRecord::Migration
  def change
    create_table :kittens do |t|

      t.timestamps null: false
    end
  end
end
```

In its most basic form, we would then add columns to that table...

```ruby
class CreateKittens < ActiveRecord::Migration
  def change
    create_table :kittens do |t|
      t.string :name
      t.string :name
      t.integer :age
      t.timestamps null: false
    end
  end
end
```

Once we have filled out the migration, we run the migrations - this will run all migrations that have yet to be run, in order according to their timestamps:

```bash
rails db:migrate
```

It's important to check that the migration was successful - ideally, the terminal response should be something like this:

```bash
== 20160706031227 CreateKittens: migrating ====================================
-- create_table(:kittens)
```

If not, there was a problem with your migration! Read the terminal response and try again.

### **Migrations to modify a table**

You can also generate migrations to modify an existing table. If you follow a very specific format in your command, Rails will often be able to generate the necessary code in the migration for you. For example, if the migration name is of the form "AddXXXToYYY" or "RemoveXXXFromYYY" and is followed by a list of column names and types then a migration containing the appropriate add\_column and remove\_column statements will be created.

```bash
rails generate migration AddBreedToKittens breed:string
```

This will generate a migration which can be run without editing the migration itself at all:

```ruby
class AddBreedToKittens < ActiveRecord::Migration
  def change
    add_column :kittens, :breed, :string
  end
end
```

Otherwise, you can generate a migration \(with any name you like\) that you need to complete yourself:

```bash
rails generate migration ChangesToKittens
```

This will generate a migration which needs to be edited before running the migration:

```ruby
class ChangesToKittens < ActiveRecord::Migration
  def change
  end
end
```

### **Bad migrations**

As a general rule, you should not edit or delete a migration once it has been run. This is likely to affect the integrity of your migration history, and cause you a _lot_ of headaches down the line. If you've run a migration and want to change it, the best approach is to write another migration that corrects the issue in your schema.

If you need to delete a migration, you _can_ roll back the migration \(by running `rails db:rollback` until you see that the problematic migration has been reversed\), then edit/edit the migration before running `rails db:create` again. You still need to be careful that none of the subsequent migrations depended on the migration you have edited/deleted.

**Ruby on Rails - Migrations - Further Readings**

* [Rails Guides - ActiveRecord Migrations](http://guides.rubyonrails.org/v4.2/active_record_migrations.html)

## Ruby on Rails - Generators

```ruby
rails generate controller ControllerName list of actions
rails generate controller Greetings index create
rails g controller Greetings index create
# THIS WILL CREATE VIEWS, JS, CSS AND ACTIONS IN CONTROLLERS (PLUS TESTS)

rails g model ModelName field:type
rails g model Painting name:string year:date
# THIS WILL CREATE MODELS, MIGRATIONS, AND TESTS

rails g scaffold ModelName field:type field:type
rails generate scaffold Painting name:string year:date
# THIS WILL CREATE EVERYTHING
```

### Ruby on Rails - Form Helpers

Form helpers are designed to make working with models much easier compared to using just standard HTML elements by providing a set of methods for creating forms based on your models. Example.

```markup
<%= form_for @artist do |f| %>
  <fieldset>
    <%= f.label :name %>
    <%= f.text_field :name %>
  </fieldset>
  <%= end %>
```

### Ruby on Rails - Strong Parameters

To reduce the chance of bad people trying to insert things into the database, we can use strong parameters to disallow all but the fields we have whitelisted. IN the example below, the `Artist` is being created with only the :name, :nationality, :dob, :period, :image from the :artist field \(table\)

```ruby
  def create
    artist = Artist.create artist_params
    redirect_to artist # GET the show page
  end

  # Strong params: create a whitelist of permitted parameters
  private
  def artist_params
    params.require(:artist).permit(:name, :nationality, :dob, :period, :image)
  end
end
```

### Ruby on Rails - Partial

Best example is that the `new` and `edit` are almost exactly the same. We can move the bulk of the code into `_form.html.erb` which will look like...

```markup
<%= form_for @artist do |f| %>
  <fieldset>
    <%= f.label :name %>
    <%= f.text_field :name %>
  </fieldset>
<%= end %>  
```

In this partial form, it is the `@artist` variable which tells the form how to behave. If `@artist` is populated with data, then we have an edit page, if it is a new empty variable then we have a new page.

To place this partial in it's parent form do this;

```markup
<h1>Edit Artist</h1>
<%= render :partial => 'form' %>
```

## Ruby on Rails - Associations I <a id="ruby-on-rails-associations"></a>

Associations allow you to create relationships between any two models \(that inherit from ActiveRecord::Base\). By declaratively telling Rails that two models have a certain association with each other, we can greatly streamline our code.

Rails supports six types of associations, the two simplest associations are `belongs_to` and `has_many`.

### `belongs_to` <a id="belongs_to"></a>

A belongs\_to association sets up a one-to-one connection with another model, such that each instance of the declaring model "belongs to" one instance of the other model. For example, if your application includes customers and orders, and each order can be assigned to exactly one customer, you'd declare the order model this way:

```ruby
  class Order < ActiveRecord::Base    
    belongs_to :customer, optional :true  
  end
```

A few things to note about `belongs_to` associations:

* The singularized form of the 'other' model \(`customer`\) _must_ be used for the association to work.
* The `order` table must also include the `customer_id` for the customer records to which it belongs.
* A `belongs_to` association often has a reciprocal `has_one` or `has_many` association on the other model.

### `has_many` <a id="has_many"></a>

A has\_many association indicates a one-to-many connection with another model. You'll often find this association on the "other side" of a belongs\_to association. This association indicates that each instance of the model has zero or more instances of another model. For example, in an application containing customers and orders, the customer model could be declared like this:

```ruby
class Customer < ActiveRecord::Base  
  has_many :orders
end
```

A few things to note about `has_many` associations:

* The pluralized form of the 'other' model \(`orders`\) _must_ be used for the association to work.
* A `has_many` association often has a reciprocal `belongs_to` association on the other model.
* The model with the `has_many` association doesn't store the 'foreign key' of the related model. That goes on the model with the `belongs_to` association.

### _Ruby on Rails - Associations_ <a id="ruby-on-rails-associations-1"></a>

* ​[Rails Guides - Association Basics](http://guides.rubyonrails.org/v4.2/association_basics.html)​

## **Homework**

* Create a CRUD system for two associated models, similar to today's codealong. a director has many films, an author has many books, a musician has many songs, a sportsball team has many sportsball players
* Read:
  * [Rails Guides - Active Record Migrations](http://guides.rubyonrails.org/v4.2/active_record_migrations.html)
  * [Rails Guides - Active Record Associations](http://guides.rubyonrails.org/v4.2/association_basics.html)
  * [https://guides.rubyonrails.org/form\_helpers.html\#dealing-with-model-objects](https://guides.rubyonrails.org/form_helpers.html#dealing-with-model-objects)
  * [https://guides.rubyonrails.org/action\_controller\_overview.html\#strong-parameters](https://guides.rubyonrails.org/action_controller_overview.html#strong-parameters)
  * [https://guides.rubyonrails.org/routing.html\#resource-routing-the-rails-default](https://guides.rubyonrails.org/routing.html#resource-routing-the-rails-default)
  * [https://guides.rubyonrails.org/command\_line.html\#rails-generate](https://guides.rubyonrails.org/command_line.html#rails-generate)

