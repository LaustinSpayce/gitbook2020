# Active Record

## Learning Objectives

- Define the term "ORM" and why we use it over a database language
- Explain what Active Record is and what problems it solves
- Explain "convention over configuration" and how it relates to Active Record
- Define a class that inherits from Active Record
- Utilize Active Record to perform CRUD actions on a database
- Differentiate between class and instance methods in Active Record classes/objects

## Framing

So far, we've learned principles of object-oriented programming and how to get data to persist in a database using SQL. However, we now need some way to connect the two. We need to be able to retrieve data from a SQL database and store it in Ruby objects that we can use in our application. While we could use the `pg` gem to write and execute SQL commands in our Ruby code, this process is onerous and can result in a lot of repetitive code. We would have to write very long SQL statements to do even simple CRUD actions.

It'd be **really** nice if a bunch of genius programmers had already worked out some kind of way to interface between the database and our servers/applications in order to streamline the process of reading and writing data to and from a database. Enter **ORMs** and [The Active Record pattern](https://en.wikipedia.org/wiki/Active_record_pattern).


## ORM's & Active Record

- Object Relational Mapping: A programming technique for converting data between incompatible type systems in object-oriented programming languages - from [Wikipedia](https://en.wikipedia.org/wiki/Object-relational_mapping)

We need a way to encapsulate the data from our databases into objects so that we can use them in our server applications. ORM's serve that purpose. Remember those tables we created in SQL? Well, now those rows of data are objects (instances of classes) on our server now. That's what ORM's do.

![ORM-ActiveRecord](https://github.com/wdi-sg/activerecord-intro/blob/master/orm.jpg?raw=true)

More concretely ORM's:

- *'Map'* (translate) objects to rows in our DB (and vice versa)
- **Conventions**:
  - 1 table per Model/Class/Entity
  - Model names are singular and capitalized, table names are lowercase and plural
  - each column represents an attribute for that model
- Table associations are handled using foreign keys

It just so happens you will be learning one of the best ORM's on the market. It has some of the best documentation and best syntax (because Ruby is awesome). This ORM is Active Record.


> Active Record is the M in MVC - the model - which is the layer of the system responsible for representing business data and logic. Active Record facilitates the creation and use of business objects whose data requires persistent storage to a database. It is an implementation of the Active Record pattern which itself is a description of an Object Relational Mapping system. (from AR docs)

## Active Record

Active Record is an ORM (packaged in a Ruby gem) that allows us to translate database records into objects that we can use in our Ruby applications.


In order to use Active Record in our Ruby code to manipulate data in a database, we need to be able to talk about the **models** of our data.

But before we even do that, we have to decide on what our data is! How much of the real world are we going to attempt to represent in our programs? What data do our programs need to fulfill their purpose? To be able to think about how to model our data, we need a process where we can precisely identify what we need represented in our programs as data.

Programmers are constantly modeling domains, real world, fictional, abstract, mathematical or otherwise.

<br>
<details>

<summary>What is Domain modeling and why do we do it?</summary>

> Domain modeling is the act of describing entities and their relationships in an application's data. This method is useful for deciding data what needs to be persisted and how it should be organized.

</details>
<br>



When we model data, we tend to be talking about the **Nouns** in our application. These are the names of the *tables* in our database and the names of our Ruby *classes*.

Likewise when we write queries, we use **Verbs** to describe the specific data we want.


Essentially, in order to store and retrieve information, a lot of what we do today in Ruby will look like some form of the equation:


> **Noun** + **Verb** = **Data**
>
> Class.method # Class (Noun) method (Verb)
>
> Taco.all # returned all Taco data!
>
> 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮


With the help of Active Record, we can begin to write programs that follow this simple pattern to manipulate data.




### Convention Over Configuration (10 min / 0:30)

Before we get started with code, let's highlight a reoccurring theme with Active Record, Rails, and frameworks in general. You'll often hear us say, "Convention Over Configuration."



Before we discuss the concept as a class, take 30 seconds to think about what that phrase means--why *might* we prefer convention over configuration?

<details>
<summary><strong>Question:</strong>  Without getting into the specifics of AR, what do you think we mean by convention over configuration?</summary>
<br>



> Active Record, Rails, and other frameworks have a whole bunch of conventions that they follow so that you do not have to mess with different configuration details later. These conventions exist because developers arrive at a consensus on best practices. These road-tested conventions allow us to spend less time trying to configure when there already is an accepted way to do things. Thanks to the programmers who have come before us, we inherit a well-designed, default configuration that spares us from many headaches that we'd encounter if we were building things from scratch (yikes!).


>
> Some of the common ones we will encounter are naming conventions such as:
  - plural vs single
  - Capitalized
  - ALL_CAPS_SNAKE_CASE
  - lowercase
  - camelCase
  - kabob-case
  - snake_case.

  Obeying the naming conventions in Active Record, particularly regarding what is singular vs. what is plural, saves you a good deal of headaches.

</details>



##### If you don't follow the conventions, you're going to have a bad time.

Obeying the naming conventions in Active Record saves you a good deal of headaches.

### Defining a Model

In the `artist.rb` file, we define our `artist` model:

```ruby
class Artist < ApplicationRecord
  # AR classes are singular and capitalized by convention
end
```

> In this Ruby file, we create a class of Artist that inherits from `ApplicationRecord`. Essentially, when we inherit from `ApplicationRecord`, it gives this class a whole bunch of functionality that maps the Ruby `Artist` class to the `artists` table in Postgres.


```ruby
create_table :artists do |t|
  t.string :name
  t.string :photo_url
  t.string :nationality
  t.timestamps
end
```

## Inheritance
We are using this stntax for ruby inheritance:
```
class Artist < ApplicationRecord
```

This means we can use Artist class in much the same way we would use an ActiveRecord class. Except that we can add in all our Artist class stuff. The code looks clean and isn't cluttered by active record code, and we don't care how the active record methods we get for free *actually* work.

If you want a bit more detail on inheritance in ruby check the [gitbook.](../../ruby-inheritance/readme.html)

### Setting up the db

#### migrations
We will generate a migration as normal.

### seeds.rb
To seed the db, we use ruby instead of sql.

### running the db configs:

```bash
rails db:create
rails db:migrate
rails db:seed
```

## Using Active Record

- Run it (`$ rails console`)

Your output should be something like this (it won't be the same letters and numbers next to `#<Artist:`)
```bash
>> Article.first
  Article Load (0.5ms)  SELECT  "articles".* FROM "articles" ORDER BY "articles"."id" ASC LIMIT $1  [["LIMIT", 1]]
#<Article id: 1, title: "some title", text: "some text stuff", created_at: "2019-08-08 03:47:51", updated_at: "2019-08-08 06:02:40">
```

We'll create an instance of the `Artist` object on the Ruby side:

> **Note** the syntax for creating a new instance.

```ruby
kanye = Artist.new(name: "Kanye West", nationality: "American")
```

To save our instance to the database we use `.save`:

```ruby
kanye.save
```

If we want to initialize an instance of an object AND save it to the database we use `.create`:

```ruby
elvis = Artist.create(name: "Elvis Presley", nationality: "American")
```

<!-- SQL for create  -->
<details>
<summary>This would roughly translate to `SQL` code</summary>

<code>
INSERT INTO artists (name, nationality) VALUES ('Elvis Presley', 'American');
</code>

</details>

<br>

**Question:** Why is there a distinction between when it's saved in one command versus two?

One really handy feature we get from an Active Record inherited class is that all of the attribute columns of our model have `attr_accessor`'s as well. So we can do things like:

```ruby
kanye.name
# gets the name of kanye
kanye.name = "Yeezus"
# sets name to "Yeezus"
kanye.save
# saves the model to the database
```

> **Note:** Should be noted that when we set the attribute using a setter, it doesn't automatically save to the database, it's not until we call `.save` on the object that it saves to the database


To get all of the artists we use `.all`:

```ruby
Artist.all
```


We can also find an artist by its ID using `.find`:

```ruby
Artist.find(1)
```

<!-- SQL for find  -->
<details>
<summary>This would roughly translate to `SQL` code</summary>
<code>
SELECT * FROM artists WHERE id = 1 LIMIT 1;
</code>
</details>

<br>


Additionally we can also find an artist by an attribute using `.find_by`:

```ruby
Artist.find_by(name: "Elvis Presley")
```

> Note that find_by only returns the first object that meets the requirements of the arguments



If you want all artists that meet a certain query then we use `.where`:

```ruby
Artist.where(nationality: "American")
```



We can also update attributes and save at the same time using the `.update` method:

```ruby
elvis = Artist.find_by(name: "Elvis Presley")
elvis.update(nationality: "Canadian")
```

> If we want to update the attribute and not save we would have to use something from this [post](http://www.davidverhasselt.com/set-attributes-in-activerecord/)



Finally we can also just destroy/delete rows in our database with the `.destroy` method:

```ruby
kanye = Artist.find_by(name: "Yeezus")
kanye.destroy
# goodbye kanye you're gone forever
```


> This is exciting stuff by the way, imagine, while we do these things, that our artists model is instead a post on Facebook, or a comment on Facebook. So the next time you comment on someone's Facebook page you have an idea now of whats happening on the database layer. Maybe not the whole picture, but you have an idea. We're going to build on that idea in the coming week and half, and thats really exciting.


### Pairing Exercise

We will use `rails console` in your rails app to test out ActiveRecord class and instance methods.

Create a migration file with: 

```bash
rails generate migration artists
```

put this inside:
```ruby
create_table :artists do |t|
  t.string :name
  t.string :photo_url
  t.string :nationality
  t.timestamps
end
```

seeds.rb
```ruby
w_al = Artist.create(
  :name => 'Weird Al Yankovich', :photo_url => 'http://i.huffpost.com/gen/1952378/images/o-WEIRD-AL-facebook.jpg', :nationality => 'American'
)

kiss = Artist.create(
  :name => 'Kiss', :photo_url => 'http://www.gannett-cdn.com/-mm-/b0ad212381eab60e31d1f067f1c478cea741469a/c=0-10-3443-1963&r=x1683&c=3200x1680/local/-/media/USATODAY/GenericImages/2014/03/31//1396298223000-KISS-KISS-BAND-JY-0718-62187918.jpg', :nationality => 'American'
)

gwar = Artist.create(
  :name => 'Gwar', :photo_url => 'http://www.heavymetal.com/v2/wp-content/uploads/2014/08/gwar_promo.jpg', :nationality => 'American'
)
```

```bash
rails db:migrate
rails db:seed
```

Run the active record code above from `rails console`.

#### Further
Use the links to the rails documentation below to call other methods of active record.

### Resources
- [Active Record Basics](http://guides.rubyonrails.org/active_record_basics.html)
- [Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html)

### Appendix


#### Instance vs Class Methods

| method              | instance | class    |
|---------------------|:--------:|:--------:|
| .new                |          |     x    |
| .save               |     x    |          |
| .create             |          |     x    |
| attribute accessors |     x    |          |
| .all                |          |     x    |
| .find               |          |     x    |
| .find_by            |          |     x    |
| .where              |          |     x    |
| .update             |     x    |          |
| .destroy            |     x    |          |
| .destroy_all        |          |     x    |

---

#### Some Conventions in AR

|description      | Rule    |
|-----------------|---------|
|table names      |snake case and plural|
|model file names |snake case and singular|
|model definition |Camel case and singular|
