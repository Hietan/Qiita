---
title: 【防衛省サイバーコンテスト2025】CTFコンテスト初見プレイ (Writeup)
tags:
  - CTF
  - writeup
  - mod-ctf
private: false
updated_at: '2025-02-02T22:33:37+09:00'
id: 3f6aa1969047d95208c2
organization_url_name: null
slide: false
ignorePublish: false
---

# はじめに

この記事は，2025年2月2日に開催された，「防衛省サイバーコンテスト2025」の解説・感想記事です．
筆者が初めて CTF に参加した感想も含めて記事を書いています．
初心者目線での記事にこだわっているため，CTF にトライしようと思っている方の参考になれば幸いです．

:::note warn
筆者は初めて CTF に参加したため，記事には不明瞭な点や間違いが含まれる可能性があります．
あらかじめご了承ください．
:::

## 防衛省サイバーコンテスト2025 について

https://www.mod.go.jp/j/approach/defense/cyber/c_contest/index.html

![c_contest_main_2025.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/5d64b349-6734-eb3b-b4fa-21228df3c325.png)

「防衛省サイバーコンテスト2025」は，防衛省主催の CTF (Capture The Flag) 大会です．
脆弱性や暗号の中にある答え (flag) を探して点数を稼ぎます．

本コンテストでは，PG (プログラミング), NW (ネットワーク), WE (Webアプリケーション), CY (暗号), FR (フォレンジック・バイナリ解析), PW (Pwnable・プログラムの脆弱性), TR (その他・トリビア) の 7分類 計30問 に 12時間で挑戦します．

## CTF にチャレンジしようと思った動機

筆者は元々競技プログラミングからプログラミングコンテストの界隈に参加していました．
元々セキュリティ周りに興味や知識があったので，いつかはCTFに参加したいと思っていたのですが，タイミングがなくずっと未参加でした．
大会開催の数ヶ月前に大学の掲示板で本大会の開催を知りました．
CTF の経験はおろか，CTF の対策も全くしていませんでしたが，まずは経験ということでチャレンジしてみることにしました．

## 筆者の前提知識・経験

前提として，筆者には以下のような前提知識・経験があります．

- 情報系の大学卒
- 情報処理安全確保支援士試験 合格
- フロントエンドエンジニアとしてアルバイト経験あり
- 競技プログラミング大会 (ICPC 2024) のアジア予選進出経験あり

このような経験があったので，CTF にも問題なく参加できたという事実はあると思っています．
問題のレベル感などを考える上での参考になれば幸いです．

## 結果

結果は，**15問** / 30問 クリアで，**2200点** / 5800点 でした．
順位としては，**181位** / 480位 でした．
初見プレイにしてはかなり大健闘した方だと考えています．
個人的には1,2問解けてギブアップだと思っていたので，思ったより解けて嬉しいです．

# 各問題の解法

このセクションでは，私が実際に問題を解いた手順や，考えたこと，感想などを書いています．

:::note info
この記事では，あくまで **筆者自身の解法・考え・感想** を掲載しています．
詳細で厳密な解法は，ぜひ他のハイレベルな方の記事をご覧ください．
:::

## PG (プログラミング)

### 「縮めるだけじゃだめ」（100点）

苦戦度：⭐️⭐️⭐️

本問題では，Excelファイルが与えられ，そこから flag を探し出す問題です．

ファイルの拡張子が `.xlsm` のマクロ形式だったため，マクロに何かあることを疑いました．
ただ，そもそも開発タブを表示させるところから躓きました...

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/72bd3a0a-dd89-e4ad-894a-960abe2b312f.png)

マクロ実行前も実行後も上記画像のような状態だったため，マクロを無効化した状態で開くなどもしましたがうまくいかず．

マクロでは，マス目の色を塗り替えることをしているのですが，冗長な部分があると感じブレークポイントを1行ずつ設置しながら確かめていくと，
ある1箇所で止めると数字列が出力されることがわかりました．

![image2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/e048a81b-6c6f-14b3-1810-b9e5f366adaf.png)

ソースコードを読むとき，全体を読んで処理を決めつけるのではなく，
きちんと細かい部分まで精査することが重要だと学びました．

### 「暗算でもできるけど？」（100点）

苦戦度：⭐️

本問題では，プログラムを実行した時の数字列から，68番目の値と，その数字列から推測される314番目の数字を足した値がフラグになります．

C言語のファイルが与えられたので，とりあえずコンパイルをして実行してみると，

![image3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/058b37fa-4e94-0e72-21f9-43fff115b80c.png)

明らかに素数列でした．
ソースコードを読んでみると，少し読みにくい形にはなっていましたが，確かに素数列生成のプログラムになっていました．

Webサイトで素数の一覧表を検索し，68番目と314番目を素直に足した値を答えとしました．

### 「loop in loop」（300点）

苦戦度：⭐️⭐️

本問題では，2つの値を引数とし，計算した結果が flag となる，競プロのような問題でした．
必要なことは，RIPEMD160 によるハッシュ化と XOR を用いた計算だけであり，
Python で両者とも問題なく使えることがわかったので，Jupyter Notebook で実装しました．

問題の解釈ミスなどもあり少し手こずりましたが，問題なく正解できました．

300点問題だったのですごく警戒していましたが，中身は思ったより簡単でした．
この問題で，点数や何問目かより，「何人が解けているか」を確認するのが大事だとわかりました．

### PG 総括

本セクションはセキュリティに直結しない内容で，かつ競技プログラミングに近いので，比較的楽に解くことができました．
PW (プログラミング言語の脆弱性) の問題と比べても，内部使用をがっつり知っておかなければいけないわけでもないので，取り組みやすい問題だと思いました．

## NW (ネットワーク)

### 「頭が肝心です」（100点）

苦戦度：⭐️⭐️⭐️

本問題では，メールの `.eml` ファイルが与えられ，対象のメールが2番目に経由したメールサーバの IP アドレスを特定する問題でした．

`.eml` ファイルのヘッダ部分(?) に IP アドレスなどの情報があることを知っていたので大きく苦戦はしませんでしたが，
どちらが送信側でどちらが受信側なのかがわからなかったので，上と下からそれぞれ2番目を回答しました．（正解は下から2番目でした）
正解はできましたが，なぜ正解かは理解できていないので，メールの仕様を見直してみようと思います．

### 「3 Way Handhshake？」（200点）

苦戦度：⭐️⭐️

TCPポートスキャン時のパケットログからオープンポートを見つける問題です．

パケットを調査できる Wireshark や 3 way handshake, TCP については大学時代にしっかり勉強していたので，比較的スムーズに進めることができました．

（3 Way Handshake は，Web企業のインターン面接で問題として出され，答えられなかった苦い思い出があります...）

パケットの情報に，55944 ポートが大量に登場していたので，こちらがポートスキャンの実行側だと解釈しました．
55944 ポートにレスポンスしているもののうち，[SYN, ACK] フラグが含まれるものを，以下の式でフィルタリングしました．

```
tcp.flags.syn == 1 && tcp.flags.ack == 1
```

![image4.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/68c9a598-8692-ee0a-aea2-1de257e82645.png)

いい感じにフィルタリングすることができたので，手動でピックアップして回答しました．

### 「さあ得点は？」（200点）

苦戦度：⭐️⭐️⭐️⭐️⭐️

パケットのログから攻撃を特定して，その CVE を調べ，CVE の BaseScore を答えとする問題でした．

BaseScore は "1.5" のような 2 桁の数字だったので， 総当たりで行けるかとも思ったのですが，なぜか正解せず．
答えの数字だけ飛ばしてしまってたのかもしれません．

ログを見ていると，やたら同じ IP から TCP, HTTP のリクエストが飛んでいたので，DoS だと判断しました．
ただ，DoS は攻撃であって脆弱性じゃないので，CVE を見つけるのに苦戦しました．

そんな中，パケットの中身も見なければいけないことに気づきました．
やたら数字の羅列が並んでいるパケットを見つけたので調べてみると，Apache Range Header DoS 脆弱性（CVE-2011-3192）だということが判明しました．

今回はたまたま見つけられましたが，どのように見つければよかったのかがまだ疑問として残っています．
パケットの Length が大きかったことがヒントなのかもしれません．

Wireshark で解析する時は，様々な情報を比較しながら観察することが大切だと感じました．

### NW 総括

多少の知識があったので，ツール等も問題なく使えましたが，まだまだできることが大きいのだと痛感しました．
大量のデータから必要な情報を見つけ出す，人力データマイニングの力を身につける必要があると感じました．

## WE (Web アプリケーション)

### 「簡単には見せません」（100点）

苦戦度：⭐️⭐️

Web ページのどこかにある flag を探すという単純な問題です．
TOP ページにアクセスすると，何もないすっからかんな Web ページでした．

![image10.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/2e5a5248-c6bd-1dc3-7a98-b1ad6246dc77.png)

フロントエンド開発をする時の癖で，ソースコードとコンソールを確認しましたがこちらにも何もなく．

ここで何を思ったのかわからないのですが，`robots.txt` にアクセスしました．
これが見事にビンゴし，`/blue/` という下層ページが `Disallow` として設定されていることが発見されました．

`/blue` の下層ページにアクセスすると，今度こそソースコードに flag がコメントアウトとして隠されていました．

![image8.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/07c54d35-4526-a14b-454b-732bfb5f0113.png)

なぜ，`robots.txt` が思いついたのかはわかりません．
CTF ベテランの方が，どのような順序で flag を探していくのかが非常に気になります．

### 「試練を乗り越えろ！」（100点）

苦戦度：⭐️⭐️⭐️⭐️

本問題の Web ページは，「今何問目」を 1万回答えないと flag をくれないという鬼畜仕様でした．

![image9.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/97e39063-1ae2-fe99-fa50-b0ca892032a5.png)

for 文で 1万回 POST リクエストを送信することも考えましたが，やめておきました．というか対策されていました．
`curl` コマンドでリクエストしようとすると，ユーザーエージェントの設定のせいなのか，弾かれてしまいました．

![image5.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/c8aac0cb-d821-0c58-0867-fc60001ffddc.png)

`qCount` という値が隠されており，この値を 10000 に設定することでスキップできることには早めの段階で気付くことができました．

ただ，どのようにリクエストを送ればいいのかがわかりませんでした．
Web で検索し，開発者機能に「編集して再送信」という，すでに送ったリクエストの内容をカスタマイズして再送できる機能があることを知りました．
この機能で，`qCount` と `answer` の値を 10000 に変更し，リクエストを再送することで，flag をゲットすることができました．

![image6.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/71b74f8a-3ac5-9b8f-0b0e-70b6a15ba791.png)

「編集して再送信」は Google Chrome でもできると思うのですが，大会中に見つけることができず，FireFox を追加でインストールして実行しました．
ぜひ有識者の方教えてください．

### ❌「直してる最中なんです」（200点）

(本問題は正解することができませんでした．わかったところまでを掲載して供養します．)

この問題では，ダウンロード機能が途中までしか実装されていない Web ページをハックし，flag のファイルを取得します．

コメントアウトで，ボタンやスクリプトを隠していることがわかったのですが，
スクリプトを活用してどのように flag を取得していいかがわからず，諦めました．

![image7.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/d2a63831-b899-5460-12ee-ce62ad8f352e.png)

### ❌「直接聞いてみたら？」（200点）

(本問題は正解することができませんでした．わかったところまでを掲載して供養します．)

API のテストを行うためのフォームが設置された Web ページから，flag を抽出する問題です．
レスポンスをコンソールに出力してみるとリクエストの内容が Base64 でエンコードされた文字列が返ってくることがわかるのですが，
そこからどのように flag を抽出したらいいのかが，見当がつきませんでした．

### WE 総括

筆者の中では，この WE (Web アプリケーション) 分野が最も CTF の第一印象として強く，
ついに CTF に参加できる人になったんだとしみじみとしていました．

Web 系の開発経験があったのでキーポイントを発見するところまでは行けるのですが，
そこからどのように答えまで辿り着くのかという CTF 力が足りないことを強く感じました．

また，CTF では XML でのリクエストをよくみるのですが，
XML は今まで避けてきたので，この機に勉強しようと思います．

## CY (暗号)

### 「エンコード方法は一つじゃない」（100点）

苦戦度：⭐️⭐️⭐️⭐️

以下の文字列をエンコードする問題です．

```
%26%23%78%35%35%3b%26%23%78%36%33%3b%26%23%78%36%31%3b%26%23%78%36%65%3b%26%23%78%34%32%3b%26%23%78%37%64%3b%56%6d%46%79%61%57%39%31%63%30%56%75%59%32%39%6b%61%57%35%6e%63%77%3d%3d%36%36%36%63%36%31%36%37%37%62
```

`%**` という形式は，日本語の URL を貼り付けた時によくみるので，とりあえず URL デコードしました．

```
&#x55;&#x63;&#x61;&#x6e;&#x42;&#x7d;VmFyaW91c0VuY29kaW5ncw==666c61677b
```

さらに前半の数値文字列参照を元に戻し，中間は Base64 でデコードし，後半は Hex エンコードを使ってデコードします．

```
UcanB}VariousEncodingsflag{
```

これらを並び替えて，`flag{VariousEncodingsUcanB}` となりました．

どうしてこれらのエンコードに辿り着けたのかは謎です．
このような特徴の文字列はこのエンコード，とすぐにわかるようになりたいです．

また，いちいちプログラムを書くことが面倒なので，特徴的なエンコード方式は先にメソッドを作って再利用できるようにするべきだと感じました．

### 「File Integrity of Long Hash」（100点）

苦戦度：⭐️⭐️⭐️

```
189930e3d9e75f4c9000146c3eb12cbb978f829dd9acbfffaf4b3d72701b70f38792076f960fa7552148e8607534a15b98a4ae2a65cb8bf931bbf73a1cdbdacf
```

上記に一致するファイルを探せ，という問題です．はて，どういうこと？

問題では zip ファイルが与えられ，展開すると大量のファイルがありました．
ただ，ファイルの内容は全て `flag{***}` の形式であり，上記の文字列はありませんでした．

ここで，問題文に `Hash` という単語があることから，ファイル内容のハッシュ値だということに気付きました．
PC でアプリケーションをダウンロードするときに，改竄されていないかを検知するためのハッシュ値が提供されていることから着想を得ました．

ファイルのあるディレクトリで，以下のコマンドを実行し，ハッシュ値が一致するファイルを探しました．

```
ls | xargs sha512sum | grep 1899
```

すると，`flags_89.txt` が一致しました．

### 「Equation of ECC」（200点）

苦戦度：⭐️

楕円曲線の情報と公開鍵が与えられ，そこから暗号鍵を見つける問題です．
何がなんだかという内容でしたが，"o3-mini-high" に問題文を与えると全て解いてくれました．

(生成 AI の利用は公式ルールで禁止されていない認識です．)

![楕円.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/27895e09-f001-9e89-078c-c55d6fecb8f0.png)

結果的には正解できましたが，きちんと仕組みを理解して正解したかったというのが反省です．

### CY 総括

このカテゴリでは，様々な暗号化について知ることができました．
問題で謎の文字列が与えられた時，それがどのエンコード or ハッシュ化なのかすぐに判断することが難しいので，
様々なエンコード方式を実装してみて，その癖を掴みたいです．

## FR (フォレンジック・バイナリ解析)

### 露出禁止！（100点）

苦戦度：⭐️⭐️

ログから脆弱性を見つけ，不正ログインする問題です．
ログで確認できる URL には，セッションキーとして Base64 エンコードされた文字列が含まれていました．
デコードすると，`タイムスタンプ,id,user` という情報が含まれていました．
タイムスタンプを未来の値にし，再度アクセスすることで，不正ログインすることができました．

タイムスタンプが最初のアクセス時間だと勘違いしており，アクセス期限であることに気付くまで少し時間がかかりました．

### 成功の鍵（200点）

苦戦度：⭐️⭐️⭐️

あるハッカーが攻撃した時のログから，パスワードを発見する問題です．
ハッカーは総当たり的にパスワードを試しており，
ログの中には，大量の認証失敗，さらにその中に1件だけ認証成功がありました．
認証成功の下に認証失敗が8つあったので，
アクセスのパケットのうち，下から8つ目を回答しましたが不正解でした．
結局最後のアクセス時に使用していたパスワードが正解だったのですが，なぜなのかはわかっていません．

### FR 総括

2問正解することはできましたが，きちんと理解して正解することができませんでした．
ログやバイナリの解析について，きちんと勉強しようと思います．

## PW (Pwnable：プログラムの脆弱性)

### 「CVE-2014-7169他」（100点）

苦戦度：⭐️⭐️

アクセスログから脆弱性を見つけて，Web アプリに侵入する問題です．
問題文の CVE は， "ShellShock" という脆弱性で，User-Agent にコマンドを埋め込むことができます．

![image11.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/5b46fbbe-f7d5-b1c8-3e1e-f62eec81f737.png)

ログファイルでも，確かに echo コマンドなどを確認できます．
1つだけ 200 ステータスが帰ってきている URL があるので，その URL で flag を含むファイルを出力させる echo コマンドを追加しました．

無事正解することができましたが，ログのそれぞれの値が何を意味するのか，また共通したフォーマットがあるのかが気になりました．

### ❌「認可は認証の後」（200点）

(本問題は正解することができませんでした．わかったところまでを掲載して供養します．)

Web アプリのなんらかの脆弱性を利用して，flag を取得する問題です．
Web アプリにはユーザー登録のフォームがあり，JS を確認するとバリデーションのコードがありました．
バリデーションがブラウザで実行されているので，POST リクエストでバリデーションされていない値を送信することができることがわかります．
ただ，どのように応用することができるのかがわかりませんでした．

正解数も少なそうだったので，スキップしました．

### PW 総括

このカテゴリは，CTF のメタ的な要素が多いと感じました．
セキュリティの知識は多少ありますが，CTF の傾向と対策を知らなかったので，CTF 向けの書籍等で学習することが必要だと感じました．

## TR (その他・トリビア)

### ❌「合体はロマン」（100点）

(本問題は正解することができませんでした．わかったところまでを掲載して供養します．)

4分割された QR コードの画像が与えられる問題です．
ただ組み合わせるだけではうまくいかず，なんらかの編集が必要なようです．
ファインダパターンや，白黒が交互になる列など，QR コードの特徴から考えてみましたが，ギブアップしました．

### 「Windowsで解きましょう」（200点）

苦戦度：⭐️⭐️

大量のファイルを生成する bat ファイルが与えられます．その中から印の付けられたファイルを見つける問題です．
スクリプトを確認すると，index が 25 の時だけ特殊な操作をしていたので，意味は理解できていませんが，25 のファイルが大切だと推測しました．
index が 25 の時のファイル内容を手動で再現し，flag を抽出しました．

### TR 総括

TRカテゴリは本当になんでもありという風に感じました．
プログラミングやネットワークに限らず，幅広い＆深い知識が必要となることを痛感しました．

# 感想

初めて CTF に参加してみて，その魅力に気付くことができました．
筆者は元々は競プロの畑にいましたが，それぞれに違った魅力があると感じています．
競プロはアルゴリズムに特化しており，陸上で言うと「短距離走」のイメージです．
CTF は，セキュリティに限らず，プログラミング言語，アプリケーション，ネットワークなど，幅広い知識・技能が必要となり，「10種競技」のイメージを持ちました．

CTF を学ことで，様々なプロトコル・アルゴリズムの細かな仕様を理解することができ，推理力を鍛えることができると感じたので，
体系的に勉強し，別の CTF でリベンジしようと思います．

また，いずれ機械学習コンペの界隈にも参加してみたいです．
