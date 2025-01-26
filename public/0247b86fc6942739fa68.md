---
title: C++で２次元配列をもっと楽に扱いたい！
tags:
  - C++
  - AtCoder
  - 競技プログラミング
  - vector
private: false
updated_at: '2024-12-02T21:17:04+09:00'
id: 0247b86fc6942739fa68
organization_url_name: null
slide: false
ignorePublish: false
---
## 🎄 Qiita Advent Calendar 2024 🎄

この記事は，[Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024) の [ひとりアドベントカレンダー by Hietan](https://qiita.com/advent-calendar/2024/hietan) というカレンダーの２日目の記事です．

https://qiita.com/advent-calendar/2024/hietan

アウトプット力の強化を目的に，25日分すべて１人で書ききるというチャレンジをしています．是非他の記事もご覧ください．

---

:::note warn
注：以下では，`using namespace std;` の記述がある前提でソースコードを記載しています．
:::

みなさん，競技プログラミングはやっていますか？
競技プログラミングでは速度の速さから C++ が使われることが多いですが，vector の記述量が多くて大変です．

特に２次元配列を vector で作成するとき，デフォルトだと次のような記述になります．

```cpp
vector<vector<int>> arr(i, vector<int>(j, 0));
```

なんとも長い...
`vector` と3回も記述しています．

そこで，以下のクラスを作成して記述量を削減します．

```c++
template <typename T>
class V2 : public vector<vector<T>> {
    public:
        V2(int i, int j): vector<vector<T>>(i, vector<T>(j)) {}
        V2(int i, int j, T x): vector<vector<T>>(i, vector<T>(j, x)) {}
};
```

これで，2次元配列を宣言するときは，

```c++
V2<int> arr(i, j, 0);
```

とてもシンプルになりました！

仕組みもコンストラクタを作成しているだけで非常にシンプルです．
これで `vector` と大量に打つ手間を省けますね．
