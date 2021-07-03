## How to pass method like a block

In Ruby we are able to pass a method or a function like a block. In order to do it use `&method` keyword

```ruby
def add_one(number)
  number + 1
end

[1,2,3].map(&method(:add_one))
# [2,3,4]
```

and pass a method

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
