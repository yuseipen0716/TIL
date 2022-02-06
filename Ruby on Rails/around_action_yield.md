## around_action の動作について

「パーフェクトRuby on Rails」p.80 <2-3-2 アクションに対してフックで処理を差し込む> のなかで、

> aroundフックはアクションの前後で実行する都合上、呼び出すメソッドではbefore相当の処理を実行したあとでyieldを使って、アクション側に処理を戻してあげる必要がある。

とありました。destroyアクションに対してaround_actionを使用する例として

``` ruby
class BooksController < ApplicationController
  protect_from_forgery except: [:destroy]
  around_action :action_logger, only: [:destroy]

  def destroy
      @book.destroy
      respond_to do |format|
          format.html { redirect_to "/"}
          format.json { head :no_content}
      end
  end

  private

  def action_logger
      logger.info "around-before"
      yield
      logger.info "around-after"
  end
end
```
のようなコードが紹介されていました。

--- 

### 処理の流れのイメージ　

処理の流れとしては

1. <destroy>アクションのリクエストを受け取る
1. アクション実行前に、メソッドaction_loggerが呼ばれる
1. before相当の処理（"around-before")が実行される
1. yieldによりdestroyアクションが呼ばれ、実行される。
1. destroyアクション完了後にafter相当の処理("around-after")が実行される
  
という感じかな、とイメージはできるのですが、いまいちこの部分のyieldの使い方が腑に落ちません。
  
--- 
  
### yieldの所作イメージ 


yieldを使用する際は以下のように、自分で定義したメソッドのブロック内の処理を呼び出す際にするときに使うイメージでした。

``` ruby
# ブロック付きメソッドの定義、
# その働きは与えられたブロック(手続き)に引数1, 2を渡して実行すること
def foo
  yield(1,2)
end

# fooに「2引数手続き、その働きは引数を配列に括ってpで印字する」というものを渡して実行させる
foo {|a,b|
  p [a, b]
}  # => [1, 2] (要するに p [1, 2] を実行した)
```
[Ruby リファレンス yield](https://docs.ruby-lang.org/ja/latest/doc/spec=2fcall.html#yield)より抜粋
  
上記したRailsのコードではyieldでdestroyアクションを呼んでいるようにみえるのですが、Railsにおけるyieldの所作が生のRubyと少々異なるのか、それとも実は裏側ではアクションがブロック（またはProcオブジェクト)として渡されているのでしょうか。その点が気になりました。
  
  
  --- 

### around_actionを定義している部分やその周辺のソースコードも見てみる
  
[around_action 定義まわりのソースコード](https://github.com/rails/rails/blob/5-2-stable/actionpack/lib/abstract_controller/callbacks.rb#L186)の187行目で`define_method`メソッドを使用して動的に`around_action`メソッドを作り、`_insert_callbacks`メソッドが実行され、ブロック内でset_callbackしている。
  
  おそらくこの、`set_callback`でコールバックの設定をしている？
  
  ソースコードの[この部分](https://github.com/rails/rails/blob/5-2-stable/actionpack/lib/abstract_controller/callbacks.rb#L40)で`process_action(上記の例で言うdestroyアクション?`が呼ばれるたびに`run_callbacks`メソッドが動くようになっている(?)
  
  ---
  
### ここまでで考えられること

とりあえず、yieldについてのモヤモヤについてですが、`action_logger`メソッドの中のyieldで`destroy`アクションが呼べているということは、callbackの処理によって、`destroy`アクションが呼ばれると、`action_logger(&:destroy)`みたいな感じで、`destroy`アクションを行うブロックオブジェクト付きで`action_logger`メソッドが実行されているのではないかと予想。
  
それも踏まえて、ここまでの処理をまとめて、順番に並べてみる

1. ｀destroy`アクションを起こすよう呼ばれる
1. コールバック発火
1. `destroy`アクションが実行される前に`action_logger`メソッドがブロックオブジェクト付きで呼ばれる
1. `action_logger`実行。前半のbefore_action相当の部分（yield以前）が処理される
1. yieldでブロックオブジェクトが呼び出され、`destroy`アクションが実行される
1. アクション実行後、`action_logger`の続き、つまりafter_action相当の部分が実行される


