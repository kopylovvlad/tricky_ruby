## Side effect by using hash

In Ruby all data are passed by reference. Please don't change hash that you receive as an argument. If you have to do it, please clone original object

```ruby
global_hash = {
  'hello' => 'world!',
  'I love' => 'ruby'
}

def change_hash(hash)
  hash['new_key'] = 'new value'
  hash
end

def pass_variable(h)
  change_hash(h)
end

puts global_hash
# {"hello"=>"world!", "I love"=>"ruby"}
new_hash = pass_variable(global_hash)
puts new_hash
# {"hello"=>"world!", "I love"=>"ruby", "new_key"=>"new value"}
puts global_hash
# {"hello"=>"world!", "I love"=>"ruby", "new_key"=>"new value"}
```
