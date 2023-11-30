***Abstraction*** 
___
Do not provide access to data and processes beyond where they are needed. 
For example in Ruby you can write that a method is private, then that method can only be called from within that class (private methods are used to do things that are only needed to be done within that class). 

```ruby
class Sample
   private
   def privateMethod
      puts "Trying..."
   end
end

object1 = Sample.new
print object1.privateMethod
# output: NoMethodError: private method `privateMethod' called for #<Sample:0x0000000106c75b80>
```

*Ruby cannot tell that `object1` has a method called privateMethod as it has been coded under the `private` keyword*

***Inheritance***
___
A class inherits behaviour from another class.
For example in Ruby you can copy over instance variables from the superclass of a class using the `super` keyword.

```ruby
class Superclass
	def initialize(from, to)
		@from = from
		@to = to
	end
end

class Subclass
	def initialize(from,to)
		super
	end
end
```

Another example is that an instance of a subclass has any public method of a superclass.

```ruby
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
end

class Cat < Animal
end

sparky = GoodDog.new
paws = Cat.new
puts sparky.speak           # => Hello!
puts paws.speak             # => Hello!
```

Remember you can override methods in ruby.

```ruby
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
  attr_accessor :name

  def initialize(n)
    self.name = n
  end

  def speak
    "#{self.name} says arf!"
  end
end

class Cat < Animal
end

sparky = GoodDog.new("Sparky")
paws = Cat.new

puts sparky.speak           # => Sparky says arf!
puts paws.speak             # => Hello!
```

*`self` refers to the instance*

***Encapsulation***
___
This refers back to the abstraction part. 

_Encapsulation_ is the deliberate erection of boundaries in code that prevents erroneous accessing and modifying of states and behaviours that don’t make sense for what our intention is.

Polymorphism
___
When a single interface performs different functionality depending on the context in which it’s invoked

