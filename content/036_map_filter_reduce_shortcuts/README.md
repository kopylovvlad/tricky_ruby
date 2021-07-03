## Map/Filter/Reduce shortcuts

Userful shortcuts

```ruby
# .reject(&:nil?) equal to .compact
[1, 2, nil, 4, 5].compact
#  => [1, 2, 4, 5]
```

Userful shortcuts for array of arrays

```ruby
# .reduce(:+) equal to .flatten and it supports deep merge
[[1,2,3],[4,5]].flatten
# => [1, 2, 3, 4, 5]
```

Userful shortcuts for array of hashes

```ruby
# how to merge array of hashes
[{a: 1}, {b: 2}].reduce({}, :merge)
# => {:a=>1, :b=>2}
```
