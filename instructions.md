This is a markdown file. To view it in the correct format, click preview
on the cloud9 menu above (between Support and Run).

We sped through the first few days of introductory material. We covered one or
two semesters of computer science classes in the first few days alone! If you
are new to programming, you may feel a little lost. This exercise is meant to
build up your skills and confidence.

Try to solve these on your own. You can refer to other exercises and the
internet, but don't put anything in your program until you 100% understand
what it does and why you needed.

To solve a problem, think about how you would solve it step by step in plain
English and write those steps down as comments in your code.

Only after you have mapped out the solution in English, start thinking about
the code. If you don't know how to do code of the steps, ask Eric or try
searching online.

We're going to build a simple card game. The rules of the game are as follow:

1) There are two players to the game. They play with a standard deck of cards.
2) The dealer gives the top five cards on the deck to player 1. Then
     the dealer gives the top five cards on the remaining deck to player 2.
3) The players add up the numbers on their cards (ignoring suits). Ace is 1,
     J is 11, Q is 12, and K is 13.
4) The player with the higher hand wins.

To start, we need a `Card` class. For now, our card class will have two instance
variables:

Create a file called `card.rb` in the `lib/` directory. In this file write your card class. To tell
ruby that you are making a class, write:

```ruby
class Card

end
```

### What are classes, objects, and instances?

##### With some comments on state and behaviors

Simply put, a "class" is a blueprint for making "objects". We call the process
of using a class blueprint to make an object "instantiation", so we will some
times refer to objects as "instances". This can be a little confusing, but
"objects" and "instances" are used interchageably. If you care deeply about
the difference, this stack overflow post attempts to explain it:
http://stackoverflow.com/questions/1215881/the-difference-between-classes-objects-and-instances

Objects have two characteristics: state and behavior. A car object's state is
described with things like position, current speed, headlights on/off, etc. A
car object's behaviors include things like accelerate, brake, turn on
headlights, etc. These behaviors all change the state of the object. Some
aspects of a car's state, such as the car's make and model, are established
once, when the car object is created and they are not affected by any behaviors.
If two objects have the same state, we can consider them to be identical. If you
told me you see a Red Honda Civic on Mill Ave adjacent to our office in the
right lane driving exactly 40.2 mph with its headlights on, and someone else
described a car with exactly those same characteristics, I would conclude that
the two of you were describing the same car.

### Back to our code

To start off, our card class will have two things that determine its state: a
suit ("hearts", "spades", "clubs", "diamonds") and a rank (2-10, "A", "K", "Q",
"J"). We call these state characteristics "instance variables". Like a car's
make and model, these instance variables will not change over time.

Let's write an initializer for our Card class. The initializer (aka
"constructor") will take two parameters, `suit` and `rank` and set the instance
variables, `@suit` and `@rank` equal to their respective parameters.

Refer to other exercises or search online for examples of how to write a
initializer in ruby.

The initializer allows us to instantiate (create a new object) the `Card` class
by running:

```ruby
c = Card.new('diamonds', 2) # create a new card object representing 2 of diamonds
```

### On Readers and Writers

By default in ruby, there is no way for other parts of your program to observe
or modify an object's state (instance variables). To "expose" an object's state
to other parts of your program, you need "accessor" methods. Broadly speaking,
there are two types of accessor methods: "readers" and "writers" (also called
"getters" and "setters").

A basic reader method simply returns the raw value of an instance variable.
If a reader method is just going to return a raw value, it does not need any
parameters. Reader methods should not change the state of the object.

Writer methods change the value of an instance variable. They take just one
parameter, which is the value we wish to set the instance variable equal to.
Writer methods change the state of the object.

### Back to our code

We are going to need some readers for our `Card` class so that other parts of
the program can access the `suit` and `rank` variables. Ruby provides us with
a shortcut to create these (`attr_reader`), but in order to practice, let's
write our own.

A basic reader method for `suit` looks like this:

```ruby
def suit
    return @suit
end
```

Notice that we are calling the reader the same name as the instance variable.
This naming convention makes it more natural for other parts of our program to
access this variable.

Add the suit reader, and write your own reader method for `rank`.

Since these instance variables will not change after the `Card` is initialized,
we do not need writer methods.

We are going to want a few other methods that transform the instance variables
before returning them. You can play around with these methods as you write
them in `pry`. To install `pry`, run the following in your bash terminal:

```
gem install pry
```

Once it is installed, just type `pry` in your terminal to open up a ruby
console. Be sure to `pry` from the root directory of your project. The
root directory of the project is the folder that contains `instructions.md`
and the `lib` directory.

To load your code into `pry` use the following command:

```ruby
load("lib/card.rb")
```

Now you can create cards and run methods on them like this:

```ruby
c = Card.new('spades', 6)
c.suit
c.rank
```

Whenever you change your `card.rb` file, you will have to tell `pry` to
reload it. You can do so by re-running the `load("lib/card.rb")` command.

#### rank_as_num

We need a method that will convert the cards rank to a number. We'll use
this method when we count up the number of points each player has.

This method should use an `if`, a few `elsif`'s, and an `else`.

Here are some examples:

```ruby
two_of_diamonds = Card.new('diamonds', 2)
two_of_diamonds.rank_as_num # => 2

king_of_spades = Card.new('spades', 'K')
king_of_spades.rank_as_num # => 13

ace_of_hearts = Card.new('hearts', 'A')
ace_of_hearts.rank_as_num # => 1
```

#### to_s

Write a method called `to_s` that returns a description of the card in
two characters: one for the rank and one for the suit. Here are some examples:

```ruby
two_of_diamonds = Card.new('diamonds', 2)
two_of_diamonds.to_s # => "2D"

king_of_spades = Card.new('spades', 'K')
king_of_spades.to_s # => "KS"

ace_of_hearts = Card.new('hearts', 'A')
ace_of_hearts.to_s # => "AH"
```

Note that the character for the rank is first, and both characters are
capitalized. To write this method, first think in English about all the steps
you need to do. Write these steps as comments inside your `to_s` method. Then
write the code to do those steps. If you get stuck, ask Eric for help.

### On to the deck!

Cards alone aren't that useful. We'll need a deck of them to play our game.
Let's create a blueprint for a `Deck` with a new class. Create a new file called
`deck.rb`. Since we'll need to refer to the `Card` class in our `Deck` class,
we need to `require` the `card.rb` file:

```ruby
require_relative "card"
```

A deck has one piece of state: the set of cards that are in it. We'll implement
this in our code with an instance variable called `@cards` that will contain an
array of `Card` objects.

#### Initializer and accessors

The initializer for the `Deck` class will be more complicated than the `Card`
class's initializer. The `Deck` initializer will not take any parameters, but
it will be responsible for creating 52 `Card` objects and inserting them into
the `@cards` instance variable.

Think about the steps you would need to do before reading the next paragraph.
If you are stuck, read the next paragraph for my ideas on how to create the 52
cards.

One way to implement this would be to create two arrays inside of the
initializer. One array will contain all the possible suits (so it will have 4
elements). The other array will contain all the possible ranks. You can then
loop over the `suits` array. For each `suit`, you can loop over the `ranks`
array, and for each `rank` you can create a new card and insert it into `@cards`.
Remember to create an empty array called `@cards` before you try inserting
things into it.

Once you have figured out the steps (or used my suggestion), add each step as a
comment on its own line in your program. Now that you have a list of things to
do, try coding them. If you get stuck try to search online or ask Eric.

After you're done with the initializer, add a reader method for `@cards` the
same way we created reader methods for `@suit` and `@rank`.

#### Utility methods

##### #deal(n)

We'll need a method that takes the top `n` cards off the deck. Call this method
`deal` and define it so that it takes one parameter, `n`. This method should use
a loop to remove the first `n` items and add them to a new array.
Look up the `#shift` method in the ruby documentation for Array. Even though
shift takes a parameter, let's use it without one, so that we can practice
using loops.

Your `deal` method should return the array with the cards you removed.


##### #shuffle_cards

We'll want a method that shuffles the cards. This one is very easy thanks to
a built in method called shuffle provided by the Array class. Look it up in the
ruby documentation for Array.

All this method needs to do is shuffle the `@cards` array.

Once you've written this method, add a `shuffle_cards` command at the end of
your initializer.

### Don't Hate the `Player`...

Now we need a class to represent the players of the game. Each `Player` has the
following instance variables: a `@name`, a `@hand` (an array of cards this
`Player` has drawn), a `win_count` (how many games the player has won), and a
`loss_count`. Make a new file called `player.rb` that will contain your
`Player` class.

Create an initializer that only takes `name` as a parameter. The initializer
will `@name` equal to the parameter, set `@hand` equal to an empty array and set
`@win_count` and `@loss_count` equal to zero.

Create reader methods for all three instance variables, and create writer
methods for `@win_count` and `@loss_count`. The writer method for `@win_count`
should look like this:

```ruby
def win_count=(new_win_count)
    @win_count = new_win_count
end
```

Create similar writer method for `loss_count`.

#### Hand methods

##### #add_cards_to_hand(card_array)

This method will take an array of cards, and add it to this player's `@hand`.
Like before, think about what steps this method needs to do before trying to
code them.

##### #score_hand

This method will add up the `rank_as_num` for each card in the `@hand` and
return the score as an integer.

##### #hand_to_s

This method will return the player's `@hand` as a string (use the Array class's
`#join` method)

##### #clear_hand

This method will discard all cards the player has, so that `@hand` is an empty
array.

### Hate the `Game`

We now need a `Game` class. This class will set up and play the game. Create a
`game.rb` class. You'll need to require all of the other class files like this:

```ruby
require_relative "Card"
require_relative "Deck"
require_relative "Player"
```

A `Game` object will have an array of `@players` and a `@deck`. Write an
initializer that will take parameters for a players array. The initializer
should also provide the `Game` object with a fresh `Deck`.
You don't need any readers and writers here.

#### Game methods

##### #player_info

The `player_info` method will return a string with some information about the
players -- specifically each player's name and win/loss counts. You can
format this however you want.

##### #deal_to(player_index)

The `deal_to` method will give the player at `player_index` in `@players` 5
cards. This method should use the `Player#add_cards_to_hand` method as well
as the `Deck#draw` method.

##### #deal_to_all

This method will call `deal_to` for each player in our array.

##### #player_hands

This method should return a string that contains both players' names, their
hands, and a the points value of the hand. Use the methods we wrote for the
`Player` class.

##### #determine_winner

The `determine_winner` method will calculate the players' scores and choose
which player is the winner. It will add/subtract to each player's `win_count`
and `loss_count`.

##### #reset_game

The `reset_game` method will clear the players' hands and replace `@deck` with
a fresh `@deck`.


### main.rb

All we need now is a program that will play some games and print the results to
the console.

This file won't have a class -- it will just be a series of commands.

This file should require `player.rb` as well as `game.rb`.

First, create two `Player` objects (and give them names).

Then, create a `Game` object (remember to pass it an array of players).

Then, create a loop that will play three games. Each play of thegame should
print out the `player_info`, `deal_to` the players, print out the
`player_hands`, `determine_winner`, print out the `player_info` again (so we
see the updated win/loss records) and then `reset_game` so its ready to be played
again.

Your results should look something like:

```
Jack (0-0), Jill (0-0)
Dealing...
Jack's hand: 3H, 5S, 5C, KS, 10D (36 pts)
Jill's hand: 4D, AC, 4S, QD, 8D (29 pts)
Jack (1-0), Jill (0-0)
Resetting game...

(3x)
```

### Further practice

Here are some ideas of things you could change or additional features you could
implement for the game.

1. Take input from the user to decide how many games to play, the names of the
    players, etc.
2. Change the rules of the game. It would be pretty easy to make the game into a
    simple version of blackjack. Just change the number of cards to two and
    make the winner be the player who is closest to 21 without going over
    (note that it's impossible to go over 21 with two cards).
3. Allow players to make choices such as replacing cards that they drew with
    cards from the deck.
4. Write tests for your methods using rspec.