---
title: Linux で sudo コマンドを実行する方法
tags:
  - Linux
  - Debian
private: false
updated_at: '2024-07-16T15:42:21+09:00'
id: 19d555600142b3eff627
organization_url_name: null
slide: false
ignorePublish: false
---
Linux をインストールした直後，`sudo` コマンドを実行しようとすると，以下の写真のようなエラーが発生します．

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/1cd7b026-0019-872b-ee80-42fb22753b56.png)

# 解決策

まずは，`su` コマンドで rootユーザーにログインします．

```
su -
```

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/a1586a0d-3409-708b-e047-68ba3efa1b9a.png)

次に，`usermod` コマンドで，`sudo` コマンドを実行したいユーザーを `sudo` グループに追加します．

```
usermod -G sudo <USERNAME>
```

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/a1147837-12a8-fa03-8c53-071dfcdb667e.png)

PC を再起動し，もう一度 `sudo` コマンドを実行すると，問題なく実行できるようになっています．
