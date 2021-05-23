## All arguments are passed by reference

In Ruby all arguments are passed by reference. Keep it in your mind when you are manipulating with data and don't do side-effects

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

# example 1
puts my_number.object_id
# 25
puts my_string.object_id
# 180
puts my_hash.object_id
# 200
puts a.object_id
# 220


# example 2:
arr = [my_number, my_string, my_hash, a]
arr.each do |i|
  puts i.object_id
end
# 25
# 180
# 200
# 220

# example 3:
main(arr)
# 25
# 180
# 200
# 220
```
