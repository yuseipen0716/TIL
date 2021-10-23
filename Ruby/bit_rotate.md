## ビット回転

ビット回転についてはrubyに組み込まれておらず、解きながら学ぶJavaのQ7-12で苦戦した。

大変見苦しいコードかもしれないけれど、自分で頑張って実装できたので供養

``` ruby
# 整数xを右にnビット回転した値を返すメソッドrRotateと、左にnビット回転した値を返すメソッドlRotateを作成せよ。

# 実行例
# 整数xをnビット回転します。
# x : 1565857138
# n : 6
# 回転前 : 01011101010101010001010101110010
# 右回転 : 11001001011101010101010001010101
# 左回転 : 01010101010001010101110010010111

def rRotate(x, n)
    # puts '整数xをnビット回転します。'
    # print "x : "
    # x = gets.chomp.to_i
    # print "n : "
    # n = gets.chomp.to_i
    # 符号ビットを含むようにしてxを二進数に基数変換
    # x_b = sprintf("%#b", x)
    # # このままだと、変換後の文字列の先端が0bみたいになっているはずなので、扱いやすいようにbの文字を削除
    # x_b = x_b.delete('b')
    # # =========回転処理まとめ==========
    # 回転開始: x_bの文字数と回転したいビット数nの差の数でx_bを分割 → reverseメソッドを使用して分割したものの順番を入れ替えて連結
    # そうすると、右で弾き出されたn個の数字を前方に移動できる（右回転）
    # x_b.sizeがnより小さい場合はsplit(/(.{#{x_b.size - n}})/)の部分の挙動が変わる（{num}が最短一致のため）ので、条件を分けた。
    # nビット右回転は(文字数-n)ビット左回転と同じ？なことを利用して、x_b.size - n < n の場合は(x_b.size - n)ビット左回転する式を使う。
    if x_b.size - n > n
        x_b_right = x_b.split(/(.{#{x_b.size - n}})/).reverse.join
    else 
        x_b_right = x_b.split(//).reverse.join.split(/(.{#{n}})/).map{|i| i.reverse}.join
    end
    puts "右回転 : #{x_b_right}"
end

def lRotate(x, n)
    # puts '整数xをnビット回転します。'
    # print "x : "
    # x = gets.chomp.to_i
    # print "n : "
    # n = gets.chomp.to_i
    # 符号ビットを含むようにしてxを二進数に基数変換
    # x_b = sprintf("%#b", x)
    # # このままだと、変換後の文字列の先端が0bみたいになっているはずなので、扱いやすいようにbの文字を削除
    # x_b = x_b.delete('b')
    # =========回転処理まとめ==========
    # 回転開始: まずx_bを1文字ずつ分割して、順番を入れ替える。 → x_bの文字数と回転したいビット数nの差の数でx_bを分割
    # mapメソッドを使用して、分割して配列に格納された要素の並び順を逆転させた新しい配列を作る。 → 連結して完成
    # こうすると、左に弾き出されたn個の数字を後方に移動できる（左回転）
    # x_b.sizeがnより小さい場合はsplit(/(.{#{x_b.size - n}})/)の部分の挙動が変わる（{num}が最短一致のため）ので、条件を分けた。
    # nビット左回転は(文字数-n)ビット右回転と同じ？なことを利用して、x_b.size - n < n の場合は(x_b.size - n)ビット右回転する式を使う。
    if x_b.size - n > n
        x_b_left = x_b.split(//).reverse.join.split(/(.{#{x_b.size - n}})/).map{|i| i.reverse}.join
    else # split の中身
        x_b_left = x_b.split(/(.{#{n}})/).reverse.join
    end
    puts "右回転 : #{x_b_left}"
end

# 今回実行したい処理はrotateというメソッドにまとめる。
def rotate
    puts '整数xをnビット回転します。'
    print "x : "
    x = gets.chomp.to_i
    print "n : "
    n = gets.chomp.to_i
    x_b = sprintf("%#b", x)
    # このままだと、変換後の文字列の先端が0bみたいになっているはずなので、扱いやすいようにbの文字を削除
    x_b = x_b.delete('b')
    # =========ここまでが回転処理のための準備============
    puts "回転前 : #{x_b}"
    rRotate(x, n)
    lRotate(x, n)
end
rotate

```
