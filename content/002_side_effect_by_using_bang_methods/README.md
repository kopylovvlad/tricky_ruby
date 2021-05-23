## Side effect by using bang methods

In Ruby all data are passed by reference. If you use bang method you also change data in arguments. If you use bang methods, please clone original object or use method without the exclamation point.

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

Alternative

```ruby
# clone original object
def change_object(object)
  object.clone.map!{ |i| i + 1 }
end

# use method without the exclamation point
def change_object(object)
  object.map{ |i| i + 1 }
end
```
