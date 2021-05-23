## Break could take an argument and pass it

It can be useful when you work with loops or iterators.

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
