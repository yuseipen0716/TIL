uniqメソッド

配列内の重複をなくす。

user_idの配列で、それぞれ条件の異なるuser_idを集めたものを足し合わせた配列にはuser_idの重複が生まれる可能性があるため、その重複を取り除くために使用した。

```ruby
user_ids_1 = [1, 3, 4, 5]
user_ids_2 = [1, 2, 5, 7]
user_ids_3 = [2, 3, 5]

(user_ids_1 + user_ids_2 + user_ids_3).uniq

# => [1, 2, 3, 4, 5, 7]
```
