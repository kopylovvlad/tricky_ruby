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
