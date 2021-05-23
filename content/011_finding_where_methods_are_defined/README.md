## Finding Where Methods are Defined

Method .source_location shows you path to course code

```ruby
require 'securerandom'
SecureRandom.method(:hex).source_location
# ["/Users/my_user/.rvm/rubies/ruby-2.7.0/lib/ruby/2.7.0/securerandom.rb", 175]
```
