---
title: Linuxでグラボを使いたい(備忘録)
tags:
  - Linux
  - Debian
  - GPU
  - 備忘録
private: false
updated_at: '2024-07-16T15:53:42+09:00'
id: b6123590bb5cf500b59e
organization_url_name: null
slide: false
ignorePublish: false
---

# TL;DR

LinuxでNVIDIAのグラフィックボードを利用したいとき，以下のコマンドを実行してドライバをインストールする

```
sudo apt-add-repository contrib non-free
sudo apt install nvidia-driver -y
sudo reboot
```
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/5a1027f3-fef3-af6b-3618-68a9c39473cb.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/8a54a6d2-c5e3-a216-ec99-4ca3438e4eaf.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1206897/80fa2b6e-579c-8b83-34ff-41a71e1cf8cd.png)
