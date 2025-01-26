---
title: Debian12とWindows11でデュアルブートする手順
tags:
  - Linux
  - Debian
  - デュアルブート
  - debian12
private: false
updated_at: '2023-10-26T13:43:42+09:00'
id: 3f3867ffca1db341633e
organization_url_name: null
slide: false
ignorePublish: false
---
Linuxを物理環境で使用するために，Windows11が入ったPCにデュアルブートでDebian12をインストールした時の手順を備忘録として記事に残します．

# はじめに

実行環境，必要な前提条件等です．
必ずご確認ください．

### 検証日

2023/10/26 (木)

### 実行環境

| 項目 | バージョン等 |
|:-:|:-|
| OS | Windows 11 Home 22H2 |
| Linux | Debian 12.2.0 |
| CPU |  Intel Core i7-10700K |
| Rufus | 4.3.2090 |

### 前提条件

* Windows11の入ったPC
* USBメモリ

:::note warn
本記事では，ディスクのパーティションを行わず，1つのSSD全体をLinux用に使用しています．
ディスクのパーティションを行う場合は，事前にパーティション分割の設定を済ませてください．
:::

# インストールメディアの作成

まずはDebian12をインストールするためのインストールメディアをUSBメモリで作成します．

## ISOイメージのダウンロード

まずは，[Debianの公式HP](https://www.debian.org/)にアクセスします．

TOPページ右側の「Download」ボタンからISOイメージをダウンロードします．

![Debian公式HP](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/6794e8fa-968e-ef65-c8fb-240d73f169a8.png)

エクスプローラーを確認すると，`debian-12.2.0-amd64-netinst.iso`ファイルがダウンロードされました．

![ISOファイルがダウンロードされたエクスプローラー](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/525f3e16-55fc-8d0c-4d0a-d0368dcb41af.png)

## Rufusのダウンロード

Rufusは，ISOイメージからインストールメディアを作成するためのソフトウェアです．

[Rufusの公式HP](https://rufus.ie/ja/)から，exeファイルをダウンロードします．
それぞれの環境に適したファイルをダウンロードしてください．
今回は，`rufus-4.3.exe`を選択しました．

![Rufusの公式HP](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/9b19c064-712c-2c4d-8332-6529b89f8fcf.png)

ダウンロードしたexeファイルを開き，Rufusを起動します．

![Rufusがダウンロードされたエクスプローラー](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/72c8e098-f530-cf21-1f71-a55b4bbf4ffa.png)

「このアプリがデバイスに変更することを許可しますか?」と聞かれた場合は，「はい」を選択します．

## ISOファイルからインストールメディアの作成

:::note alert
USBメモリに既に入っているデータは削除されるので注意
:::

インストールメディアを作成するための設定を行います．

以下の2点を変更します．
2点以外はデフォルト設定で問題ありません．

* デバイス
    * インストールメディアを作成するUSBメモリを選択します．
* ブートの種類
    * 右側の「選択」ボタンから，ダウンロードしたISOイメージを選択します．

![Rufusの設定画面](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/c2e1df34-9c67-573f-cbdf-4f6cc463aee5.png)

「スタート」をクリックし，ISOイメージを作成します．

書き込む際のモードは，デフォルトのままで問題ありません．

![ISOHybridイメージの検出](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/f7334c19-d8e4-69ed-96b8-3eb811773149.png)

データの削除に関する忠告が現れます．
USBメモリに，必要なデータが残っていないことを確認し，「OK」をクリックしてください．

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/1122b6d5-e6f2-925c-729d-02c22dc80e75.png)

インストールメディアの作成が始まります．

状態が「準備完了」になれば作成完了です．

![準備完了になったRufus](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/788bb139-c30c-7e55-018c-515d80f6575d.png)


# Debian12のインストール

## インストールメディアの起動

PCを再起動します

BIOSが起動した画面で，Delキー（Escキー・F2キー）を複数回押し，BIOSの設定画面を起動します．

:::note warn
PCによって，BIOSを起動するためのキーは異なります．各自で確認してください．
:::

起動するメディアで，USBメモリを選択します．
(選択の方法は，各PCで異なります)

これで，Debian12のインストーラーが起動されます．

## Debian12のインストール

インストーラーメニューの項目では，`Graphical install`を選択し，GUIのインストーラを起動します．

### 言語の選択

OSの言語を選択します．
私は英語で統一したかったため，`English`を選択しました．

![言語の選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/5875f6f6-1000-e740-6daf-33ebd306ac76.png)


### 地域の選択

タイムゾーンを指定するために，地域を選択します．
`Japan`を選択します．
`Japan`がない場合，`other`→`Asia`→`Japan`の順番で選択することで，日本に設定することができます．

![地域の選択(other)](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/65b8a89b-aee4-7cb5-c668-5b1da27ef000.png)

![地域の選択(アジア)](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/7c81aa65-6c5f-d4f8-ab83-099321034659.png)

![地域の選択(日本)](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/73d30440-b050-35b7-9bc7-b7f2c7fc0b98.png)

### 文字コードの選択

言語は英語，地域は日本を選んだことで，言語と地域の組み合わせが定義されていないことにより，この確認画面が表示されました．
`United States`を選択しました．

![文字コードの選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/dcfec17a-5d2b-e70e-a206-0e9ee2d85a1e.png)

### キーマップの選択

キーボードのレイアウトを選択します．
JISキーボードの場合は，`Japanese`，
USキーボードの場合は，`American English`を選択します．

![キーマップの選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/75a873ae-721b-9310-6bef-baecbb87cc70.png)

### ネットワークの確認

自動で以下の2点が自動で確認されます．

* インストールメディアの確認
* ネットワークの確認

ホスト名の入力は，自分が識別しやすい名前が良いと思います．

![ホスト名](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/61ca9091-5ce3-5f06-52bf-67cd81e38301.png)

ドメイン名も，同様にしてください．

![ドメイン名](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/49a26500-9e91-283a-ed42-da50b08793f5.png)

### ルートユーザのパスワード設定

ルートユーザのパスワード設定です．
各自でパスワードの設定を行なってください．

![ルートユーザのパスワード設定](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/eb0acaf6-af84-c13b-a34b-fa523d5cb0b8.png)

### ユーザ名の設定

ユーザ名の設定です．
この名前がホームディレクトリのディレクトリ名等になります．

![ユーザ名の設定](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/6f680b7f-1309-a8f0-dad8-f6dd65dc1aa6.png)

### ユーザのパスワード設定

ユーザのパスワード設定です．
ログイン等で入力が必要になります．

![ユーザのパスワード設定](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/18aac378-a638-13b7-a501-59a690298d8c.png)

### ディスクのパーティション

Debian12をインストールするディスクを選択します．

:::note alert
同じディスク上にWindowsとLinuxを共存させる場合，ディスクの領域を分割する作業が必要になります．
Linux用に割り当てた領域のデータは上書きさせるため，Windows用のデータが上書きされないように注意が必要です．万が一失敗すると，Windowsが起動できなくなります．
:::

私は，Linux用にSSDを増設したため，そのSSD全体をDebian12用の領域に設定します．
そのため`Guided - use entire disk`を選択します．

![パーティション方法の選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/1e74da09-7abf-2476-6820-a17fc1e5b44f.png)

私のPCの物理構成は以下のようになっています．

| 記憶装置 | 容量　 | 役割 |
| :-: | -: | :-: |
| SSD | 1.0TB | WindowsのCドライブ |
| SSD | 500.0GB | Linux用 |
| HDD | 6.0TB | データ保存用 |

そのため，今回の場合は500GBの`/dev/nvme1n1`を選択しました．

![ディスクの選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/11d5ca5f-0db7-2940-03cb-cc7d4e9f328e.png)

全てのファイルを一つのパーティションにまとめるため，`All files in one partition`を選択しました．

![パーティション方法の選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/d5614c7d-013d-b69a-52bb-82fb9a5125be.png)

確認画面が表示されます．
問題がないことを確認し，`Continue`を選択します．

さらに，ディスクの上書きに関する確認画面が表示されました．
今回インストールするディスクには，以前のLinuxが残っていたため，上書きします．
`Yes`を選択し，`Continue`をクリックします．

![パーティションの確認](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/13cb0686-fcc1-7f3c-c150-62f6928abd69.png)

ディスクのパーティションとシステムのインストールが自動で行われます．

### パッケージマネージャのミラーサイト

パッケージマネージャを用いる時の参照先を選択します．
近い位置の方が良いため，`Japan`を選択します．

![ミラーサイトの地域の選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/6262ad35-8708-1137-1831-6c1a82e78e91.png)

日本にあるミラーサイトを選択したいため，`.jp`のものを選択してください．

![ミラーサイトの選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/a47e445a-f1d8-9fac-8e6c-2a1266f6f2ba.png)

### プロキシに関する設定

必要であれば入力してください．
今回は何も入力せず`Continue`をクリックしました．

![プロキシに関する設定](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/10a71631-139d-1620-dd1d-fe83cf7125ec.png)

### 情報提供に関する設定

レポートなどが自動的にデベロッパーに送信されるかどうかの設定です．
今回は`No`を選択しました．

![情報提供に関する設定](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/772f0079-80b8-d444-b623-a68c778f5c0e.png)

### ソフトウェアの選択

インストールするソフトウェア・デスクトップ環境を選択します．
デスクトップ環境はGNOME派なので，デフォルトのまま次に進みました．

![ソフトウェアの選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/c0b4b42e-b1da-bf12-a6cf-287c26e04411.png)

選択後，ソフトウェアのインストールとGRUBブートローダのインストールが始まります．

### インストール完了

インストールが完了しました．
`Continue`をクリックし，PCを再起動します．

![インストール完了](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/775f45c1-4205-79fa-162a-a65aab711561.png)

# Debian12の起動

GRUBのブートローダーが起動します．
本来はGUIが起動するはずなのですが，なぜかCUIが起動しました．
`exit`と入力することでGUIに移ることができました．

矢印キーでDebian12を選択し，Enterで起動します．

起動すると，ログイン画面になります．
先ほど設定したパスワードを入力してログインします．

# Debian12の初期設定

Debian12に初めてログインすると，初期設定画面が表示されます．
それぞれ任意に設定してください．

### 言語

`English`を選択しました．

![Screenshot from 2023-10-26 11-12-15.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/f46d620c-193e-df95-24ad-545ec453a446.png)

### キーボードレイアウト

USキーボードを使用しているため，`English (US)`を選択しました．

![Screenshot from 2023-10-26 11-14-24.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/75f40b41-20c7-8ca3-b7e9-2efe2e4b6038.png)

### 位置情報

位置情報を使う予定がないため，Offにしました．

![Screenshot from 2023-10-26 11-14-33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/bca683a4-5705-7744-72b0-0f951a6e03f1.png)

### アカウント連携

アカウントの連携です．
今回は必要ないためスキップしました．

![Screenshot from 2023-10-26 11-15-20.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/4205acdc-ac96-0fe7-be6c-85612de11199.png)

### 初期設定完了

初期設定が終了しました．

![Screenshot from 2023-10-26 11-15-55.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/0c5d8b38-c9ba-154b-813d-66db314396bd.png)

# 最後に

これでDebian12のインストールが完了しました．
その他の設定に関してもも別記事で記載しているので，ぜひご覧ください．

# 参考サイト
* [Debianの公式HP](https://www.debian.org/)
* [Rufusの公式HP](https://rufus.ie/ja/)
