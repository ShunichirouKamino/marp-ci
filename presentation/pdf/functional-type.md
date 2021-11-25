---
marp: true
page_number: true
theme: default
paginate: true
class: lead
header: header
footer: footter
---

<!-- フォントのimport -->
<style>
@import url('https://fonts.googleapis.com/css?family=Noto+Serif+JP&display=swap');
</style>

<style>
h1 {
    font-size: 70px;
    font-weight: bold;
    color: #008080;
}
section {
    font-family: 'Noto Serif JP', serif;
}
section.box p {
    padding: 0.5em;
    margin: 0.5em;
    color: #ffffff;
    background: #008080;
    text-align: center;
}
section.agenda, section.text {
    justify-content: start;
}
section.subtitle {
    font-size: 50px;
    color: #008080;
}
</style>

<!-- _class: title -->

# 関数型プログラミングを理解する

---

<!-- _class: agenda -->

## 目次

- 関数型プログラミングとは
- なぜ今関数型プログラミングか？

---

<!-- _class: subtitle -->

## 関数型プログラミングとは

---

<!-- _class: text -->

## 手続き型プログラミング

手続き型プログラミングは、「順に処理を書いていく」プログラミング手法。

- 年齢が格納されたリストから 30 代の人数をカウントする Java のソースコード。

```java
Integer[] age = {39, 40, 33, 36, 25};
List<Integer> list = Arrays.asList(age);

// カウントする初期値
int count = 0;
// ageの中から、30～40の数をカウントする
for (int i = 0; i <> list.size(); i++) {
    int individualAge = list.get(i);
    if (individualAge >= 30 && individualAge <= 39) {
        count++;
    }
System.out.println(count);
```

---

<!-- _class: text -->

## 手続き型プログラミング

手続き型プログラミングのメリット

- コードが追いやすい
  - 上から順に読めばいいだけなので
- 学習難易度が低い

手続き型プログラミングのデメリット

- 再利用がし辛い
- 現実の物体と紐づかない

---

<!-- _class: text -->

## 関数型プログラミング

- 年齢が格納されたリストから 30 代の人数をカウントする Java のソースコード。
  - 本当にやりたい処理は filter, count の中に隠蔽されている
  - コメントが不要

```java
Integer[] age = {39, 40, 33, 36, 25};
List<Integer> list = Arrays.asList(age);

System.out.println(list.stream()
                        .filter(x -> x >= 30 && x <= 39)
                        .count());

```

---

<!-- _class: text -->

## 関数型プログラミング

関数型プログラミングのメリット

- コードが追いやすい
  - 人間の思想、動詞とリンクした関数名称
  - コメントで書くようなことを、関数の名称にする
  - ユーザが定義すべき変数が少なくなる
- 純粋関数化により、副作用を抑えることができる

関数型プログラミングのデメリット

- 学習難易度は高い
- センスが必要
  - 一目でわかる関数名称、抽象化された再利用性のある関数を定義する

---

<!-- _class: box -->

## 関数型プログラミングの基本概念

イミュータブルなデータ

純粋関数

データの変換

高階関数

---

<!-- _class: subtitle -->

## なぜ今関数型プログラミングか？

---

<!-- _class: text -->

## 豆知識

### ラムダの語源

- 数学における関数の表記
  - $f(x)=2x+1$
- ラムダ式を用いた関数の表記
  - $λx.2x+1$

後者は、プログラミングのラムダ式と近しい表記になっている。

```java
Function<Integer, Integer> func = x -> 2 * x + 1
```

$λ$を使う理由は、$f(x)$という表記ではこれが関数なのか、$x$に関数を適用したものなのか判別できないため。

---
