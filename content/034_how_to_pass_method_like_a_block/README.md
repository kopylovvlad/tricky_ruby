## How to pass method like a block

In Ruby we are able to pass a method or a function like a block. In order to do it use `&method` keyword

```ruby
def add_one(number)
  number + 1
end

[1,2,3].map(&method(:add_one))
# [2,3,4]
```

You can also use `&method` with a method

```ruby
class Foo
  def call(array)
    array.map(&method(:add_one))
  end

  private

  def add_one(number)
    number + 1
  end
end

puts Foo.new.call([1,2,3])
# [2,3,4]
```

In order to pass a proc or lambda, use just `&`

```ruby
add_two = Proc.new { |i| i + 2 }
[1,2,3].map(&add_two)
# [3, 4, 5]

add_three = lambda { |i| i + 3 }
[1,2,3].map(&add_three)
# [4, 5, 6]
```
