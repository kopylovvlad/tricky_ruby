## Function definition

In Ruby there are few way to define params in a fuction. There is a benchmark for they all.

```ruby
require 'benchmark'

# just func
def foo1(arg1, arg2, arg3)
  arg1 + arg2 + arg3
end

# func with default params
def foo2(arg1 = 1, arg2 = 2, arg3 = 3)
  arg1 + arg2 + arg3
end

# func with a hash as main argument
def foo3(arg = {})
  arg[:arg1] + arg[:arg2] + arg[:arg3]
end

# func with built-in keyword arguments
def foo4(arg1: nil, arg2: nil, arg3: nil)
  arg1 + arg2 + arg3
end

# func with built-in keyword arguments with default value
def foo5(arg1: 1, arg2: 2, arg3: 3)
  arg1 + arg2 + arg3
end

n = 100_000
Benchmark.bm do |x|
  x.report('just func') do
    for i in 1..n; foo1(1, 2, 3); end
  end

  x.report('func with default params') do
    for i in 1..n; foo2(1, 2, 3); end
  end

  x.report('func with a hash as main argument') do
    for i in 1..n; foo3({arg1: 1, arg2: 2, arg3: 3}); end
  end

  x.report('func with built-in keyw args') do
    for i in 1..n; foo4(arg1: 1, arg2: 2, arg3: 3); end
  end

  x.report('func with built-in keyw args with default') do
    for i in 1..n; foo5(arg1: 1, arg2: 2, arg3: 3); end
  end
end

=begin
                                            user     system      total        real
just func                                  0.013483   0.000200   0.013683 (  0.013926)
func with default params                   0.015669   0.000042   0.015711 (  0.015785)
func with a hash as main argument          0.034613   0.001180   0.035793 (  0.036117)
func with built-in keyw args               0.014099   0.000019   0.014118 (  0.014150)
func with built-in keyw args with default  0.020912   0.000208   0.021120 (  0.022070)
=end
```
