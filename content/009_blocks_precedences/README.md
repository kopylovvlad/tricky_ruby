## Blocks precedences

It's better to use do/end block then brackets. Source: [https://tech.showmax.com/2019/10/how-ruby-can-surprise-you/](https://tech.showmax.com/2019/10/how-ruby-can-surprise-you/)

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
```
