---
marp: true
page_number: true
theme: default
paginate: true
class: lead
header: 関数型プログラミング
footer:
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

- 関数型プログラミング
- 純粋関数

---

<!-- _class: subtitle -->

## 関数型プログラミング

---

<!-- _class: text -->

## 手続き型プログラミング

手続き型プログラミングは、「順に処理を書いていく」プログラミング手法。

- 30 ～ 39 の数字の数をカウントする。

```java
List<Integer> age = Arrays.asList(39, 40, 33, 36, 25);


// カウントする初期値
var count = 0;
// ageの中から、30～40の数をカウントする
for (int i = 0; i < list.size(); i++) {
    int individualAge = list.get(i);
    if (individualAge >= 30 && individualAge <= 39) {
        count++;
    }
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
- 処理が長くなると、変数初期化など煩雑になる

---

<!-- _class: text -->

## 関数型プログラミング

- 30 ～ 39 の数字の数をカウントする。
  - 本当にやりたい処理は filter, count の中に隠蔽されている
  - コメントが不要

```java
List<Integer> age = Arrays.asList(39, 40, 33, 36, 25);

System.out.println(age.stream()
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
  - 開発者が定義すべき変数が少なくなる
- 純粋関数化により、副作用を抑えることができる

関数型プログラミングのデメリット

- 学習難易度は高い
- センスが必要
  - 一目でわかる関数名称、抽象化された再利用性のある関数を定義する

---

<!-- _class: box -->

## 関数型プログラミングの基本概念

管理変数の削減

イミュータブルなデータ

純粋関数

データの変換

高階関数

---

<!-- _class: subtitle -->

## 管理変数の削減

---

<!-- _class: text -->

## 手続き型プログラミングにおける変数管理

30 ～ 39 の数字の数をカウントし、List から`garbage`を取り除く。

```java
List<Integer> age = Arrays.asList(39, 40, 33, 36, 25);
List<String> words = Arrays.asList("a", "b", "a", "garbage", "a");

var count = 0;
for (int i = 0; i < age.size(); i++) {
    int individualAge = age.get(i);
    if (individualAge >= 30 && individualAge <= 39) {
        count++;
}
count = 0;
for (int i = 0; i < list.size(); i++) {
    String individualWord = list.get(i);
    if (!individualWord.equals("garbage")) {
        count++;
    }
}
```

---

<!-- _class: text -->

- `count` , `i`といった変数を、意識して管理しなければいけない
  - 初期化したか？再利用時前に再度初期化するのか？
  - 同一スコープ内で使っていない変数名か？

処理が複雑になり、変数が増えれば増えるほどデメリットになる。

---

<!-- _class: text -->

## 関数型プログラミングにおける変数管理

30 ～ 39 の数字の数をカウントし、List から`garbage`を取り除く。

```java
List<Integer> age = Arrays.asList(39, 40, 33, 36, 25);
List<String> words = Arrays.asList("a", "b", "a", "garbage", "a");

age.stream().filter(x -> x >= 30 && x <= 39)
            .count());
words.stream().filter(l -> !l.equals("garbage"))
              .count();
```

- 自分で管理しなければいけない変数は、あくまでデータが入っている age, words のみ。

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
