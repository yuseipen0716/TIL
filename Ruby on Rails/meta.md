## メタプログラミングmemo

### 文字列からClassのオブジェクトを取得
```ruby
model_name_string = 'User'
model_class = model_name_string.constantize
```

### Modelから、has_manyなどの関連付けを取得する
```ruby
# 全ての関連付けを取得
User.reflect_on_all_associations

# has_manyなどを指定して取得
User.reflect_on_all_associations(:has_many)
```

取得される関連付けは以下のような形
```ruby
[
 #<ActiveRecord::Reflection::BelongsToReflection:0x00007f19630fca70
  @active_record=Customer (call 'Customer.connection' to establish a connection),
  @klass=nil,
  @name=:supporter,
  @options={:optional=>true},
  @plural_name="supporters",
  @scope=nil>,
...
]
