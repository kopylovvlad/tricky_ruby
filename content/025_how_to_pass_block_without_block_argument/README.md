## How to pass block without &block argument

Using `&block` makes your code slower, in order to avoid it use `&Proc.new`

```ruby
class A
  def self.call(&block)
    return unless block_given?

    new.call { block.call }
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
