## CPU アーキテクチャを調べる

``` ruby
require 'etc'
require 'pp'

pp Etc.uname
# => {:sysname=>"Linux",
#     :nodename=>"boron",
#     :release=>"2.6.18-6-xen-686",
#     :version=>"#1 SMP Thu Nov 5 19:54:42 UTC 2009",
#     :machine=>"i686"}
```
というように返ってくるらしいので(参考:[RubyリファレンスEtc.#uname](https://docs.ruby-lang.org/ja/latest/method/Etc/m/uname.html))

``` ruby
require 'etc'
require 'pp'

pp Etc.uname.values_at(:machine)
=> ["x86_64"]
```
