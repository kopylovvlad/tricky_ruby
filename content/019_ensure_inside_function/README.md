## Ensure inside function

You can use `ensure` with and without begin/rescue block.

Without begin/rescue block.

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
```

With begin/rescue block.

```ruby
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
