## Creating variables and self assignment

You can assign variable to the same variable :-)

```ruby
2.7.0 :001 > a = a
2.7.0 :002 > a
# nil

2.7.0 :003 > b = b.to_s + ''
2.7.0 :004 > b
# ""

2.7.0 :005 > c = c.to_i + 1
2.7.0 :006 > c
# 1
```
