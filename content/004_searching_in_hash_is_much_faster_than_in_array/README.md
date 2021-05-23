## Searching in hash is much faster than in array

If you have to search data in a huge array, use hash instead of using an array.

[Link to original post](https://stackoverflow.com/a/5552062/3517175)

```ruby
require 'benchmark'

items_array = []
items_hash = {}
searchlist = []
1.upto(10_000) do |n|
  searchlist << n
  items_array << n
  items_hash[n] = n
end

Benchmark.bm(10) do |x|
  x.report('array') do
    searchlist.each do |el|
      items_array.include?(el)
    end
  end
  x.report('hash') do
    searchlist.each do |el|
      items_hash.has_key?(el)
    end
  end
end

=begin
                 user     system      total        real
array        0.546875   0.003028   0.549903 (  0.552962)
hash         0.005654   0.002471   0.008125 (  0.010864)
=end
```
