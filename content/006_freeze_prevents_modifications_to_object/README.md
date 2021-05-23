## .freeze method prevents modifications to object

```ruby
CONST = '12345'
puts CONST
# 12345

CONST[0] = '0'
puts CONST
# 02345
```

```ruby
CONST2 = '12345'.freeze
puts CONST2
# 12345

CONST2[0] = '0'
puts CONST
# can't modify frozen String (FrozenError)
```
