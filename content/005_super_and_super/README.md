## Super and super()

super - sends all arguments; super() - sends no arguments.

[Link to original post](https://stackoverflow.com/questions/31816149/difference-between-calling-super-and-calling-super)

```ruby
class A
  def self.say_hello(name = 'Bob')
    puts "Hello, #{name}"
  end
end

class B < A
  def self.say_hello(name = 'Bob')
    super # passing all arguments
    puts 'How_are_you?'
  end
end

class C < A
  def self.say_hello(name = 'Bob')
    super() # passing no arguments
    puts 'How_are_you?'
  end
end

A.say_hello
# Hello, Bob

B.say_hello('John')
# Hello, John
# How_are_you

C.say_hello('John')
# Hello, Bob
# How_are_you
```
