---
title: Pattern matching
layout: gem-single
name: dry-monads
---

Ruby 2.7 introduces pattern matching, it is nicely supported by dry-monads 1.3+.

### Matching Result values

```ruby
# presumably you do it in a class with `include Dry::Monads[:result]`

case value
in Success(Integer => x)
  # x is bound to an integer
in Success[:created, user] # alternatively: Success([:created, user])
  # user is bound to the second member
in Success(Date | Time)
  # date or time object
in Success([1, *])
  # any array starting with 1
in Success(String => s) if s.size < 100
  # only if `s` is short enough
in Success(counter: Integer)
  # matches Success(counter: 50)
  # doesn't match Success(counter: 50, extra: 50)
in Success(user: User, account: Account => user_account)
  # matches Success(user: User.new(...), account: Account.new(...), else: ...)
  # user_account is bound to the value of the `:account` key
in Success()
  # corresponds to Success(Unit)
in Success(_)
  # general success
in Failure[:user_not_found]
  # Failure([:user_not_found])
in Failure[error_code, *payload]
  # ...
end
```

In the sippet above, the patterns will be tried sequentially. If `value` doesn't match any pattern, an error will be thrown.

### Matching Maybe

```ruby
case value
in Some(Integer => x) if x > 0
  # x is a positive integer
in Some(Float | String)
  # ...
in None
  # ...
end
```

### Matching List

```ruby
case value
in List[Integer]
  # any list of size 1 with an integer
in List[1, 2, 3, *]
  # list with size >= 3 starting with 1, 2, 3
in List[]
  # empty list
end
```
