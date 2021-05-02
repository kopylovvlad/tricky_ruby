# Tricky Ruby notebook

ruby-2.7.0

## Сontents

- [All arguments are passed by reference](#all-arguments-are-passed-by-reference)
- [Side effect by using methods with '!'](#side-effect-by-using-methods-with-)
- [Side effect using hash](#side-effect-using-hash)
- [Searching in hash is much faster than in array](#searching-in-hash-is-much-faster-than-in-array)
- [Super and super()](#super-and-super)
- [.freeze prevents modifications to obj.](#freeze-prevents-modifications-to-obj)
- [Function definition](#function-definition)
- [require and load](#require-and-load)
- [Blocks precedences](#blocks-precedences)
- [Natural Language in Ruby](#natural-language-in-ruby)
- [Finding Where Methods are Defined](#finding-where-methods-are-defined)
- [.clone, .dup, .deep_dup can't do deep clone for objects and Hash](#clone-dup-deep_dup-cant-do-deep-clone-for-objects-and-hash)
- [Variable and fuction names](#variable-and-fuction-names)
- [Attr_writer doesn't work inside methods](#attr_writer-doesnt-work-inside-methods)
- [Break could take an argument and pass it](#break-could-take-an-argument-and-pass-it)
- [Implicit string concatenation](#implicit-string-concatenation)
- [Retry in begin/rescue block](#retry-in-begin-rescue-block)
- [Trailing Comma in functions](#trailing-comma-in-functions)
- [Ensure inside function](#ensure-inside-function)
- [Creating variables and self assignment](#creating-variables-and-self-assignment)
- [Converting time to string](#converting-time-to-string)

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
# example 1:
# 25
# 180
# 200
# 220

# 2
puts 'example 2:'
arr = [my_number, my_string, my_hash, a]
arr.each do |i|
  puts i.object_id
end
# example 2:
# 25
# 180
# 200
# 220

# 3
puts 'example 3:'
main(arr)
# example 3:
# 25
# 180
# 200
# 220
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
# [1, 2, 3, 4]
new_arr = change_object(global_arr)
puts new_arr.inspect
# [2, 3, 4, 5]
puts global_arr.inspect
# [2, 3, 4, 5]
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
# {"hello"=>"world!", "I love"=>"ruby"}
new_hash = pass_variable(global_hash)
puts new_hash
# {"hello"=>"world!", "I love"=>"ruby", "new_key"=>"new value"}
puts global_hash
# {"hello"=>"world!", "I love"=>"ruby", "new_key"=>"new value"}
```

## Searching in hash is much faster than in array

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
array        0.546875   0.003028   0.549903 (  0.552962)
hash         0.005654   0.002471   0.008125 (  0.010864)
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
# 12345

CONST[0] = '0'
puts CONST
# 02345
```

```ruby
CONST2 = '12345'.freeze
puts CONST2
# 12345

CONST2[0] = '0'
puts CONST
# can't modify frozen String (FrozenError)
```

## Function definition

```ruby
require 'benchmark'

# just func
def foo1(arg1, arg2, arg3)
  arg1 + arg2 + arg3
end

# func with default params
def foo2(arg1 = 1, arg2 = 2, arg3 = 3)
  arg1 + arg2 + arg3
end

# func with a hash as main argument
def foo3(arg = {})
  arg[:arg1] + arg[:arg2] + arg[:arg3]
end

# func with built-in keyword arguments
def foo4(arg1: nil, arg2: nil, arg3: nil)
  arg1 + arg2 + arg3
end

# func with built-in keyword arguments with default value
def foo5(arg1: 1, arg2: 2, arg3: 3)
  arg1 + arg2 + arg3
end

n = 100_000
Benchmark.bm do |x|
  x.report('just func') do
    for i in 1..n; foo1(1, 2, 3); end
  end

  x.report('func with default params') do
    for i in 1..n; foo2(1, 2, 3); end
  end

  x.report('func with a hash as main argument') do
    for i in 1..n; foo3({arg1: 1, arg2: 2, arg3: 3}); end
  end

  x.report('func with built-in keyw args') do
    for i in 1..n; foo4(arg1: 1, arg2: 2, arg3: 3); end
  end

  x.report('func with built-in keyw args with default') do
    for i in 1..n; foo5(arg1: 1, arg2: 2, arg3: 3); end
  end
end

=begin
                                            user     system      total        real
just func                                  0.013483   0.000200   0.013683 (  0.013926)
func with default params                   0.015669   0.000042   0.015711 (  0.015785)
func with a hash as main argument          0.034613   0.001180   0.035793 (  0.036117)
func with built-in keyw args               0.014099   0.000019   0.014118 (  0.014150)
func with built-in keyw args with default  0.020912   0.000208   0.021120 (  0.022070)
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
# ["/Users/my_user/.rvm/rubies/ruby-2.7.0/lib/ruby/2.7.0/securerandom.rb", 175]
```

## .clone, .dup, .deep_dup can't do deep clone for objects and Hash

```ruby
StructObject = Struct.new(:value, :nested_object)
obj = StructObject.new(1, StructObject.new(2))

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
# <struct StructObject value=33, nested_object=nil>


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

## Variable and fuction names

```ruby
class Foo
  def initialize
    @counter = 0
  end

  def call1
    @counter = bar # bar is funtion
    @counter # counter eqials 13
  end

  def call2
    bar = 22 # bar is variable
    @counter = bar # assign 22 to counter
    @counter # counter equals 22
  end

  def call3
    bar = 22 # bar is variable
    @counter = bar() # assign 13 to counter, because bar() is function
    @counter # counter equals 13
  end

  private

  def bar
    12 + 1
  end
end

puts Foo.new.call1
# 13
puts Foo.new.call2
# 22
puts Foo.new.call3
# 13
```

## Attr_writer doesn't work inside methods

```ruby
class Foo
  attr_accessor :row, :counter

  def initialize
    @row = { a: nil, b: 0 }
    @counter = 0
  end

  # the method raises `undefined method `+' for nil:NilClass`
  # it happens because the interpreter changes line `counter += 1`
  # into `counter = counter + 1`
  # therefore it thinks that `counter` is a variable with value `nil`
  def call1
    row[:a] = 1 # `row[:]` calls attr_reader and change value by reference
    counter += 1
    counter
  end

  def call2
    row[:a] = 1
    counter = 13 # it is a variable
    counter += 1 # it the same variable with value 14
    counter # the method return 14. instance variable `@counter` still has value 0
  end

  def call3
    row[:a] = 1
    @counter = 14 + 1 # it the same variable with value 14
    [counter, @counter] # it returns [15, 15] because the interpreter considers `counter` as not a variable, as attr_reader
  end

  def call4
    row[:a] = 1
    @counter += 1 # the line equals `@counter = @counter + 1`
    counter # it returns 1, because `counter` is not a variable, it's attr_reader
  end
end

puts Foo.new.call1
# NoMethodError (undefined method `+' for nil:NilClass)
puts Foo.new.call2
# 14
puts Foo.new.call3
# [15, 15]
puts Foo.new.call4
# 1
```

## Break could take an argument and pass it

```ruby
collection = [1, 2, 3, 4, 5]

stopped_at = collection.each do |i|
   break i if i == 3

   puts "Processed #{i}"
end
# Processed 1
# Processed 2

puts "Stopped at and did not process #{stopped_at}"
# Stopped at and did not process 3
```

## Implicit string concatenation

```ruby
array = ['Item 1' 'Item 2']
puts array
# ["Item 1Item 2"]
```

## Retry in begin/rescue block

[Link to interesting post](https://blog.appsignal.com/2018/06/05/redo-retry-next.html)

```ruby
i = 0
begin
  p "run xxx func"
  xxx(ooo)
rescue => e
  i += 1
  p "err: #{e.message}"
  sleep 1
  retry if i < 3
end
# "run xxx func"
# "err: undefined local variable or method `ooo' for main:Object"
# "run xxx func"
# "err: undefined local variable or method `ooo' for main:Object"
# "run xxx func"
# "err: undefined local variable or method `ooo' for main:Object"

i
# => 3
```

## Trailing Comma in functions

```ruby
def add_twelve(a)
  a + 12
end
# => :foo
add_twelve(1)
# => 13
add_twelve(1,)
# => 13
foo(1,'')
# ArgumentError: wrong number of arguments (given 2, expected 1)
# from (pry):11:in 'add_twelve'
```

## Ensure inside function

```ruby
def foo
  puts 'before_exception'
  12 / 0 # it raises an exception
  puts 'after_exception'
ensure
  puts 'in_ensure_block'
end

foo()
# before_exception
# in_ensure_block
# divided by 0 (ZeroDivisionError)

def foo2
  begin
    puts 'before_exception'
    12 / 0
    puts 'after_exception'
  rescue => e
    puts "We have exception #{e.inspect}"
  ensure
    puts 'in_ensure_block'
  end
  puts 'last line of the function'
end

foo2()
# before_exception
# We have exception #<ZeroDivisionError: divided by 0>
# in_ensure_block
# last line of the function
```

## Creating variables and self assignment

```ruby
2.7.0 :001 > a = a
2.7.0 :002 > a
# nil

2.7.0 :003 > b = b.to_s + ''
2.7.0 :004 > b
# ""

2.7.0 :005 > c = c.to_i + 1
2.7.0 :006 > c
# 1
```

## Converting time to string

```ruby
# in plain Ruby time isn't wrapped in quotes
eval({a: 'string', time: Time.now}.to_s)
# SyntaxError ((eval):1: syntax error, unexpected integer literal, expecting '}')
# ...>"string", :time=>2021-05-02 14:35:24.064579 +0300}

eval({a: 'string', time: Time.now.to_s}.to_s)
# {:a=>"string", :time=>"2021-05-02 14:35:24 +0300"}



# with Rails we have the save behaviour
require 'rails'
eval({a: 'string', time: Time.now}.to_s)
# SyntaxError ((eval):1: syntax error, unexpected integer literal, expecting '}')
# ...>"string", :time=>2021-05-02 14:35:24.064579 +0300}
eval({a: 'string', time: Time.now.to_s}.to_s)
# {:a=>"string", :time=>"2021-05-02 14:35:24 +0300"}

# but .to_json always wrappes time in quotes
eval({a: 'string', time: Time.now}.to_json)
# {:a=>"string", :time=>"2021-05-02T14:35:24.005+03:00"}
```
