# Tricky Ruby notebook

ruby-2.6.5

## All arguments are passed by reference

```ruby
class A; end
my_number = 12
my_string = 'hello'
my_hash = { a: 1, b: 2 }
a = A.new

def print_obj_id(item)
  puts item.object_id
  item
end

def main(arr)
  arr.each { |i| print_obj_id(i) }
end

# print objects id

# 1
puts 'example 1:'
puts my_number.object_id
puts my_string.object_id
puts my_hash.object_id
puts a.object_id

# 2
puts 'example 2:'
arr = [my_number, my_string, my_hash, a]
arr.each do |i|
  puts i.object_id
end

# 3
puts 'example 3:'
main(arr)

=begin
example 1:
25
70219041399980
70219041399920
70219041399880
example 2:
25
70219041399980
70219041399920
70219041399880
example 3:
25
70219041399980
70219041399920
70219041399880
=end
```

## Side effect by using methods with '!'

```ruby
global_arr = [1, 2, 3, 4]

def change_object(object)
  object.map!{ |i| i + 1 }
end

def pass_variable(h)
  change_object(h)
end

puts global_arr.inspect
new_arr = change_object(global_arr)
puts new_arr.inspect
puts global_arr.inspect

=begin
[1, 2, 3, 4]
[2, 3, 4, 5]
[2, 3, 4, 5]
=end
```

## Side effect using hash

```ruby
global_hash = {
  'hello' => 'world!',
  'I love' => 'ruby'
}

def change_hash(hash)
  hash['new_key'] = 'new value'
  hash
end

def pass_variable(h)
  change_hash(h)
end

puts global_hash
new_hash = pass_variable(global_hash)
puts new_hash
puts global_hash

=begin
{"hello"=>"world!", "I love"=>"ruby"}
{"hello"=>"world!", "I love"=>"ruby", "new_key"=>"new value"}
{"hello"=>"world!", "I love"=>"ruby", "new_key"=>"new value"}
=end
```

## Searching in hash is much faster then in array

[Link to original post](https://stackoverflow.com/a/5552062/3517175)

```ruby
require 'benchmark'

items_array = []
items_hash = {}
searchlist = []
1.upto(10_000) do |n|
  searchlist << n
  items_array << n
  items_hash[n] = n
end

Benchmark.bm(10) do |x|
  x.report('array') do
    searchlist.each do |el|
      items_array.include?(el)
    end
  end
  x.report('hash') do
    searchlist.each do |el|
      items_hash.has_key?(el)
    end
  end
end

=begin
                 user     system      total        real
array        0.468200   0.002505   0.470705 (  0.499312)
hash         0.001410   0.000008   0.001418 (  0.001427)
=end
```

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
    puts 'How are you?'
  end
end

A.say_hello
B.say_hello('John')

=begin
Hello, Bob
Hello, John
How are you?
=end
```

```ruby
class A
  def self.say_hello(name = 'Bob')
    puts "Hello, #{name}"
  end
end

class B < A
  def self.say_hello(name = 'Bob')
    super() # passing no arguments
    puts 'How are you?'
  end
end

A.say_hello
B.say_hello('John')

=begin
Hello, Bob
Hello, Bob
How are you?
=end
```

## .freeze prevents modifications to obj.

```ruby
CONST = '12345'
puts CONST
CONST[0] = '0'
puts CONST

=begin
12345
02345
=end
```

```ruby
CONST2 = '12345'.freeze
puts CONST2
CONST2[0] = '0'
puts CONST

=begin
12345
can't modify frozen String (FrozenError)
=end
```

## Function definition

```ruby
require 'benchmark'

# just function
def foo1(arg1, arg2, arg3)
  arg1 + arg2 + arg3
end

# function with default params
def foo2(arg1 = 1, arg2 = 2, arg3 = 3)
  arg1 + arg2 + arg3
end

# function with a hash as main argument
def foo3(arg = {})
  arg[:arg1] + arg[:arg2] + arg[:arg3]
end

# function with built-in keyword arguments
def foo4(arg1: nil, arg2: nil, arg3: nil)
  arg1 + arg2 + arg3
end

# function with built-in keyword arguments with default value
def foo5(arg1: 1, arg2: 2, arg3: 3)
  arg1 + arg2 + arg3
end

n = 100_000
Benchmark.bm do |x|
  x.report('foo1') do
    for i in 1..n; foo1(1, 2, 3); end
  end

  x.report('foo2') do
    for i in 1..n; foo2(1, 2, 3); end
  end

  x.report('foo3') do
    for i in 1..n; foo3({arg1: 1, arg2: 2, arg3: 3}); end
  end

  x.report('foo4') do
    for i in 1..n; foo4(arg1: 1, arg2: 2, arg3: 3); end
  end

  x.report('foo5') do
    for i in 1..n; foo5(arg1: 1, arg2: 2, arg3: 3); end
  end
end

=begin
       user     system      total        real
foo1  0.008736   0.000055   0.008791 (  0.008877)
foo2  0.011641   0.000070   0.011711 (  0.011813)
foo3  0.035399   0.000907   0.036306 (  0.036632)
foo4  0.014392   0.000143   0.014535 (  0.014684)
foo5  0.015809   0.000087   0.015896 (  0.016030)
=end
```

## require and load

Calling load twice on the same file will execute the code in that file twice. Calling require on the same file twice will only execute it once.

## Blocks precedences

```ruby
puts [1, 2].any? { |i| i == 3 }
# false

puts [1, 2].any? do |i|
  i == 3
end
# true

# Why? It happened because `put` can get a block

# try to fix it
puts ([1, 2].any? do |i|
  i == 3
end)
# false

# or in one line
puts([1, 2].any? do |i|; i == 3; end)
# false

# source https://tech.showmax.com/2019/10/how-ruby-can-surprise-you/
```

## Natural Language in Ruby

```ruby
How many sane ways are there to use Ruby rescue
This is actually valid Ruby code rescue
Unfortunately we cannot use punctuation here rescue
```

## Finding Where Methods are Defined

```ruby
require 'securerandom'
SecureRandom.method(:hex).source_location
# ["/Users/my_user/.rvm/rubies/ruby-2.6.5/lib/ruby/2.6.0/securerandom.rb", 158]
```

## .clone, .dup, .deep_dup can't do deep clone for objects and Hash

```ruby
StruckObject = Struct.new(:value, :nested_object)
obj = StruckObject.new(1, StruckObject.new(2))

### example

obj.clone.object_id == obj.object_id
# false
obj.clone.value.object_id == obj.value.object_id
# true
obj.clone.nested_object.object_id == obj.nested_object.object_id
# true

# and we can easily change original object
obj.clone.nested_object.value = 33
obj.nested_object
# <struct StruckObject value=33, nested_object=nil>


### Solution: use Hash and .deep_clone (rails)
obj = { value: 1, nested_object: {value: 2, nested_object: nil } }

obj.deep_dup.object_id == obj.object_id
# false
obj.deep_dup[:value].object_id == obj[:value].object_id
# true
obj.deep_dup[:nested_object].object_id == obj[:nested_object].object_id
# false

obj[:nested_object]
# {:value=>2, :nested_object=>nil}
obj.deep_dup[:nested_object][:value] = 44
obj[:nested_object]
# {:value=>2, :nested_object=>nil}
```
