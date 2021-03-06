## Chaining then to organise your code

`.then` isn't a pipeline operator like `|>` in elixir, but it can helps you to organise your code.

```ruby
# instead of it:
def handle_data(json)
  data = JSON.parse(json)
  user = User.new(data)
  user.create!
end

# you can write code like:
def handle_data(json)
  json
    .then { |str| JSON.parse(str)  }
    .then { |data| User.new(data) }
    .then { |user| user.create! }
end
```

What about if condition? You can also use it with `.then`

```ruby
# instead of it:
def handle_data(json)
  data = JSON.parse(json)
  user = User.new(data)
  user.create if user.valid?
end

# you can write code like:
def handle_data(json)
  json
    .then { |str| JSON.parse(str)  }
    .then { |data| User.new(data) }
    .then { |user| user.valid? ? user.create! : user }
end
```
