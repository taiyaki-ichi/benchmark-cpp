
# 背景
スタックとヒープのメモリに書き込む速度について、どちらの方が早いのか。mmdのローダ書いている時に気になった。もちもん、確保についてはヒープの方が遅いが、書き込む際の参照外しがどのくらいのオーバーヘッドになるのか気になった。

# 結果
https://quick-bench.com/q/iZUHaIlkXAQ-LENiWdhizd4CiX0

ほとんど変わらない。

# その他

## 「参照外し」について
この呼び方はマイナーっぽい[参考](https://detail.chiebukuro.yahoo.co.jp/qa/question_detail/q14173370699)
「dereference」の直訳。「indirection」の訳の「間接参照」の方が意味が通りやすい。

## アセンブリ
どちらのコードもforループが展開されており主に「movdqa」「movdqa」「paddd」の3つの命令で処理が記述されている

### キーワード

#### 「movdqa」「movdqu」

拡張された128ビットの整数演算のSIMD命令であるSSE2の命令であり、128ビット単位でのメモリ間のコピーを高速に行うことができる。

2つの命令の違いは以下の通り

movdqa：転送対象のメモリが16バイトでアライメント*されている*

movdqu：転送対象のメモリが16バイトでアライメント*されていない*

[参考](http://kirihari.net/program/memcpy.html)


#### 「paddd」

パックされたDword整数を加算。

[参考](https://qiita.com/deta-mamoru/items/d9582d5c0d3fe7d61f85#543-mmx-packed-arithmetic-instructions)


#### 「nopl」「nopw」

展開されたforループの処理の直前にあった命令。

よくわからない→[参考](https://stackoverflow.com/questions/17030771/why-does-x86-nopl-instruction-take-an-operand)

処理には関係なさそう


#### 「%xmm」

### 処理の流れ

書いてく





