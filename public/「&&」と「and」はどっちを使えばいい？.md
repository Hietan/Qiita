---
title: 「&&」と「and」はどっちを使えばいい？
tags:
  - C++
  - PHP
  - 論理演算子
  - プログラミング言語
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

# はじめに

プログラミング言語で論理演算を扱いたいとき，演算子としてよく，`and` (`or`, `not`) と `&&` (`||`, `!`) が使われます．

例えば，Pythonでは，

```python
(a == 1) and (b == 2)
```

と書くことができますし，Javaでは，

```python
(a == 1) && (b == 2)
```

と書くことができます．

C++, PHP, Ruby などの一部言語では，`and` (`or`, `not`) と `&&` (`||`, `!`)，両方の演算子を採用しています．

それぞれの演算子にはどのような違いがあるのでしょうか？また，どちらの演算子を使えばよいのでしょうか？

気になったので調べてみました．

# 演算子の仕様

それぞれの言語で論理演算子がどのような仕様になっているのか調べてみました．

## C++ の場合

C++ では，`&&` がデフォルトで使用できる演算子で，`and` はその **代替表現** として使用できます．[[1]](#参考文献)

代替表現の演算子には，以下のようなものがあります．

| 代替表現 | 本来の字句 |
| :-- | :-- |
| `and` | `&&` |
| `and_eq` | `&=` |
| `bitand` | `&` |
| `bitor` | `\|` |
| `compl` | `~` |
| `not` | `!` |
| `not_eq` | `!=` |
| `or` | `\|\|` |
| `or_eq` | `\|=` |
| `xor` | `^` |
| `xor_eq` | `^=` |

あくまで代替表現であるので，演算子の意味や優先順位に違いはありません．

## PHP, Ruby の場合

PHP, Ruby では `and`, `&&` の両方を使用することが可能です．

ただし，`and` は `&&` より演算子が低く設定されています．[[2]](#参考文献) [[3]](#参考文献)

演算子の優先順位を整理すると以下の通りです．（優先順位の高い順）

- `!`
- `&&`
- `||`
- `not`
- `and`
- `or`

(PHP には論理演算子 `not` はありません)

# 動作の違い

`and` と `&&` について，C++では優先順位が同じ，PHP, Rubyでは `&&` の方が優先順位が高いということがわかりました．

この仕様の影響で，`and` (`or`, `not`) と `&&` (`||`, `!`) を混用すると，結果に違いが生じます．

本記事では，以下の２つの論理式を比較します．

```ruby
true || false &&  false
true || false and false
```

C++, PHP, Ruby の各項目で２つの論理式を実行した結果は以下の通りです．

| 論理式 | C++ | PHP | Ruby |
| :-: | :-: | :-: | :-: |
| `true \|\| false &&  false` | `true` | `true`  | `true` |
| `true \|\| false and false` | `true` | `false` | `false` |

言語ごとに違った結果が確認されました．

<details><summary>実験環境</summary>

- C++
  - Ubuntu 22.04 (WSL2)
  - g++ 11.4.0
- PHP
  - Docker (`php:8.2-cli`)
- Ruby
  - Ubuntu 22.04 (WSL2)
  - ruby 3.0.2p107

</details>

<details><summary>実験時のソースコード</summary>

- C++

```c++
#include <iostream>
using namespace std;

void printBool(bool b) {
    cout << (b ? "true" : "false") << endl;
}

int main() {
    printBool( true || false &&  false );
    printBool( true || false and false );
}
```

- PHP

```php
<?php
function printBool($bool) {
    echo ($bool ? 'true' : 'false') . "\n";
}
printBool(true || false && false);
printBool(true || false and false);
?>
```

- Ruby

```ruby
puts (true || false &&  false)
puts (true || false and false)
```

</details>


C++の場合，`and` と `&&` の優先順位が同じ = `||` より `and` の優先度が高いため，

```ruby
true || ( false and false ) # -> true || false -> true
```

といったように解釈されます．

PHP, Rubyの場合，`and` より `||` の方が優先度が高いため，

```ruby
( true || false ) and false  # -> true and false -> false
```

といったように解釈されます．

このように，演算子の優先順位の違いが，式の評価の違いを生じさせています．

# 結局どちらを使うのがいいのか？

`and` (`or`, `not`) と `&&` (`||`, `!`) は実行上はどちらを使っても問題ありませんが，どちらを使うのが望ましいのでしょうか？

C++ のコーディング規約 (Google C++ Style Guide) では以下のように記述されていました．

> `and` ではなく，`&&` を使わなければならない [[4]](#参考文献)

他の言語のコーディング規約では確認できませんでしたが，一般論として，`and` ではなく `&&` を使うのが望ましいようです．

# まとめ

本記事では，`and` (`or`, `not`) と `&&` (`||`, `!`) の違いについて調べてきました．

言語によって，`and` (`or`, `not`) の優先順位の扱いに違いがあり，`and` (`or`, `not`) と `&&` (`||`, `!`) を混在させると出力結果に違いが出ることがわかりました．

コーディングする際には，少なくとも混在させることを避けてどちらかに統一し，わかりやすさや混乱防止のためにも，`&&` (`||`, `!`) を使うことが望ましいと考えられます．

# 参考文献

1. 柴田望洋『新・明解C++入門』初版（第8刷）, SBクリエイティブ株式会社, 初版：2017年（第8刷：2022年）, 66ページ．

2. PHP公式マニュアル『演算子の優先順位』The PHP Documentation Group，[https://www.php.net/manual/ja/language.operators.precedence.php](https://www.php.net/manual/ja/language.operators.precedence.php)．（最終アクセス日: 2025年1月26日）

3. Ruby公式ドキュメント『Ruby 演算子の仕様』，[https://docs.ruby-lang.org/ja/latest/doc/spec=2foperator.html](https://docs.ruby-lang.org/ja/latest/doc/spec=2foperator.html)．（最終アクセス日: 2025年1月26日）

4. Google, "Google C++ Style Guide", [https://google.github.io/styleguide/cppguide.html#Boolean_Expressions](https://google.github.io/styleguide/cppguide.html#Boolean_Expressions). （最終アクセス日: 2025年1月26日）

https://www.sbcr.jp/product/4797394634/

https://www.php.net/manual/ja/language.operators.precedence.php

https://docs.ruby-lang.org/ja/latest/doc/spec=2foperator.html

https://google.github.io/styleguide/cppguide.html#Boolean_Expressions


