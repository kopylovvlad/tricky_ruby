## Heredoc without leading and trailing whitespace

If you want to use heredoc for string and you are fatigue of using `.strip`, use `<<~`.

```ruby
class A
  def call
    str1 = <<-STR
      first string

      second string
    STR

    # NOTE: leading and trailing whitespace will be removed
    # without using .strip
    str2 = <<~STR
      first string

      second string
    STR

    # original string with all spaces
    puts str1.inspect
    # "      first string\n\n      second string\n"

    # string without leading and trailing whitespace
    puts str2.inspect
    # "first string\n\nsecond string\n"
  end
end

A.new.call

```
