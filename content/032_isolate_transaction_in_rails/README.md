## Isolate transaction in Rails

Is your database supports setting the isolation level for a transaction, you can set it

```ruby
Post.transaction(isolation: :serializable) do
  # code is here
end
```

[Documentation](https://api.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/DatabaseStatements.html#method-i-transaction)
