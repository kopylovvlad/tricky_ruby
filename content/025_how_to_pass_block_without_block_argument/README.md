## How to pass block without &block argument

Using `&block` makes your code slower, in order to avoid it use `&Proc.new`

Note: It does not work with Ruby 3 => ``new': tried to create Proc object without a block (ArgumentError)`

```ruby
class A
  def self.call
    return unless block_given?

    new.call(&Proc.new)
  end

  def call
    result = block_given? ? yield : nil
    puts "Result is #{result}"
  end
end

A.call { 1 + 1 }
# 2

def foo(a)
  1 + a
end
A.call { foo(2) }
# 3
```

[Source](https://gist.github.com/mudge/786953)
