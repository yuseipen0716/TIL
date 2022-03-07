## timestampsメソッド

timestampsを作成するメソッドでなるほど、と思った部分があったので、書き残しておく。

事前に`attr_accessor :created_at, :updated_at`が定義されており、例えば保存するための`saveメソッド`が実装されているとき

```ruby
before_save :record_timestamps

def save
  saveメソッドの内容
end

private

def record_timestamps
  current_time = Time.current
  
  self.created_at ||= current_time # created_atはまだ定義されていない可能性もあるためnilガードで。定義されているのであればその値のまま(?)
  self.updated_at = current_time
end
```
