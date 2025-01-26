---
title: Linuxが "SGX disabled by BIOS" のエラーで起動しないときの対処法
tags:
  - Linux
  - BIOS
  - 備忘録
private: false
updated_at: '2024-07-16T15:03:09+09:00'
id: 5aad489b008ada763101
organization_url_name: null
slide: false
ignorePublish: false
---
# TL;DR
グラボのドライバがインストールされていなかったのでエラーが出ていた．
`nvidia-driver` をインストールすることで解決．

# はじめに
次の構成で検証を行いました．

| 項目名 | 内容 |
| :-- | :-- |
| OS | Debian 12 |
| CPU | インテル® Core™ i7-10700K |
| GPU | NVIDIA GeForce RTX 3060 |
| マザーボード チップセット | H570 |

※ SSD を2枚マウントして Windows 11 とのデュアルブートを行っています．

# エラー概要

Windows 11 と Debian 12 をデュアルブートして利用していましたが，
何らかの原因で Debian 12 が起動しなくなりました．
BIOS のエラーを見ると，以下のようなエラーが出力されていました．

```
SGX disabled by BIOS
```

# 原因と解決策

映像出力に NVIDIA のグラフィックボードを使用しており，NVIDIA のドライバがインストールされていないことが原因でした．
映像出力を グラフィックボードではなくマザーボードから直接行うことで起動できました．

ただこのままだとせっかくのグラフィックボードが使用でできないため，
Linux 上のターミナルで以下のコマンドを実行し，ドライバをインストールしました．

```
sudo apt-add-repository contrib
sudo apt-add-repository non-free
sudo apt update
sudo apt -y install nvidia-driver
sudo reboot
```

コマンドの詳細については[こちら](https://qiita.com/Hietan/items/b6123590bb5cf500b59e)のページを参考にしてください．

https://qiita.com/Hietan/items/b6123590bb5cf500b59e

PC を再起動し，映像出力をグラフィックボードに戻すことで，エラーなく起動できました．

# おまけ

## エラー解決のために試したこと

エラー解決のために，以下の解決策を試してみました．

### BIOS の設定変更

エラー文で検索して最も多かった情報が BIOS の設定変更でした．
BIOS の設定で，`Intel Software Guard Extension (SGX)` を有効にすると治るそうです．
( 参考：[ask Ubuntu, SGX disabled by BIOS in Ubuntu 22.04](https://askubuntu.com/questions/1406760/sgx-disabled-by-bios-in-ubuntu-22-04) )

しかし，私の PC の BIOS には，SGX に関する設定項目がありませんでした．
また，H570 チップセットは SGX をサポートしていないという情報もありました．
( 参考：[パソコン工房 NEXMAG, Z590・H570・H510・B560 チップセットの機能をスペックから徹底比較！](https://www.pc-koubou.jp/magazine/49036#section04-03))

### Linux の再インストール

もともと OS を再インストールする予定だったため，何度か OS の再インストールを試してみました．
Ubuntu 24 や Debian 12 をインストールしてみましたが，変わらず同じエラーが発生しました．
