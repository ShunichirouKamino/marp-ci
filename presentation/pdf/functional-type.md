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
@import url('https://fonts.googleapis.com/css2?family=Kosugi&display=swap');
</style>

<style>
h1 {
    font-size: 70px;
    font-weight: bold;
    color: #008080;
}
section {
    font-family: 'Kosugi', sans-serif;
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
section.subsubtitle p {
    padding: 0.25em 0.5em;
    font-size: 35px;
    color: #008080;
    border-top: solid 5px #008080;
    border-bottom: solid 5px #008080;
}
</style>

<!-- _class: title -->

# 関数型プログラミング

---

<!-- _class: box -->

何となく、「ラムダ式を使え」と言っていないですか？

適切な箇所で関数型プログラミングができていますか？

---

<!-- _class: text -->

## ゴール

- 関数型プログラミングがざっくりと分かり、必要な理由が説明できる
- コードレビューをする際に、何となく「ラムダ式を使え」ではなく、「こういった理由でここは関数型にすべき」という指摘ができる
- 自身が実装する際に、関数型の使いどころが分かる

---

<!-- _class: box -->

## 目次

関数型プログラミングとは？

なぜ関数型プログラミングが必要なのか？

実際どこで利用するのか？

---

<!-- _class: subtitle -->

## 関数型プログラミングとは？

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

<!-- _class: text -->

---

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

<!-- _class: text -->

## 変数管理（手続き型プログラミング）

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

## 変数管理（関数型プログラミング）

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

<!-- _class: box -->

## 関数型プログラミングの基本概念

イミュータブル

純粋関数

高階関数

---

<!-- _class: subsubtitle -->

イミュータブル

---

<!-- _class: text -->

#### イミュータブルとは？

- ⇒ 変異しないこと

#### 何が、いつ変異しないの？

- ⇒ 関数に与えた引数が、関数の実行後も値が変異しない

#### なぜ関数型プログラミングに重要なの？

- ⇒ 処理を関数に隠ぺいして抽象化することで、ルールが必要
- ⇒ 都度関数の中を追って何をしているか見るのは手間

#### その他何かある？

- ⇒ 直接渡しの言語で特に気を付ける。Java は参照渡し、JavaScript は直接渡し

---

<!-- _class: text -->

## 破壊的なメソッド

- JavaScript での関数の引数は`参照渡し`となるため、破壊的な操作（ここでは`push`）を行った場合は muteble となる。

```javascript
const addColor = function (add, list) {
  list.push(add);
  return list;
};
let colors = [{ color: "red" }, { color: "blue" }, { color: "green" }];
let add = { color: "white" };

console.log(colors);
addColor(add, colors);
console.log(colors);
```

```console
[ { color: 'red' }, { color: 'blue' }, { color: 'green' } ]
[ { color: 'red' }, { color: 'blue' }, { color: 'green' }, { color: 'white' } ]
```

---

<!-- _class: text -->

## Immutable にするには

- push を concat にすることで、破壊的な変更を防ぐことができる。

```javascript
const addColor = function (add, list) {
  list.concat(add);
  return list;
};

let colors = [{ color: "red" }, { color: "blue" }, { color: "green" }];
let add = { color: "white" };

console.log(colors);
addColor(add, colors);
console.log(colors);
```

```console
[ { color: 'red' }, { color: 'blue' }, { color: 'green' } ]
[ { color: 'red' }, { color: 'blue' }, { color: 'green' } ]
```

---

<!-- _class: subsubtitle -->

純粋関数

---

<!-- _class: text -->

#### 純粋関数とは？

- ⇒ 関数は、少なくとも一つの引数を受け取らなければいけない
- ⇒ 関数は、値もしくは他の関数を戻り値として返却しなければならない
- ⇒ 関数は、引数・引数外のスコープの変数に直接変更を加えてはいけない

#### なぜ関数型プログラミングに重要なの？

- ⇒ 関数外のアプリケーションに影響を与えることを「副作用」と言い、副作用が有る関数は関数型プログラミングには適していない

#### その他何かある？

- ⇒ 副作用が無いことは、テストが簡単になることと同義
- ⇒ React コンポーネントは全て純粋関数

---

<!-- _class: text -->

## 純粋関数の例

- Welcome は、`<h1>Hello, {arg}</h1>`を表示する関数コンポーネント
- 引数として props を受け取り、関数スコープ外の変数に影響は与えない
- 値を戻り値として返却する

```typescript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(element, document.getElementById("root"));
```

---

<!-- _class: subsubtitle -->

データの変換

---

<!-- _class: subsubtitle -->

高階関数

---

<!-- _class: text -->

#### 高階関数とは？

- 他の関数を引数に取る（コールバック関数）
- 戻り値として関数を返す
- もしくは上記両方

---

<!-- _class: text -->

## 関数を引数に取る例

- よく登場するのは、map, filter, reduce

```java
var numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// 奇数を3乗して足す
var result = numbers.stream()
  .filter(number -> number % 2 != 0) // 奇数の抽出
  .map(number -> number * number) // 二乗する
  .reduce(0, (x, y) -> x + y); // 初期値0として、配列の前後要素を足す
```

```console
> 165
```

---

<!-- _class: text -->

## 関数を引数に取る例

- さらに関数型プログラミングっぽくした例
  - Java8 より導入された`Functional Interface`（後述）を利用する

```java
var numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

Function<Integer, Integer> toSquare = x -> x * x;
Predicate<Integer> isOdd = x -> x % 2 != 0;

// 奇数を3乗して足す
var result = numbers.stream()
  .filter(isOdd)
  .map(toSquare)
  .reduce(0, Integer::sum);
```

```console
> 165
```

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
