---
marp: true
page_number: true
theme: default
paginate: true
class: lead
header: Rust-Buginner
footer:
---

<!-- フォントのimport -->
<style>
@import url('https://fonts.googleapis.com/css2?family=Kosugi&display=swap');
</style>

<style>
h1 {
    font-size: 53px;
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

# Rust Beginner

---

<!-- _class: text -->

## ゴール

- Rust で`Hello World!`ができるようになる。
- なんとなく Rust の面白さがわかる。
- なんとなく Rust のむずかしさが分かる。

---

<!-- _class: text -->

## 目次

- とりあえず Hello World をやってみよう！
- なぜ Rust を学ぶべきか？
- Rust が流行りそうな理由
- ソースコードを読んでみよう

---

<!-- _class: subtitle -->

## とりあえず Hello World をやってみよう！

---

<!-- _class: text -->

### 環境構築手順

- C++ Build Tools のインストール
  https://visualstudio.microsoft.com/ja/visual-cpp-build-tools/
- Rust のインストール
  https://www.rust-lang.org/tools/install
- VSCode に拡張プラグインのインストール
  - "matklad.rust-analyzer"
  - "vadimcn.vscode-lldb"
- プラグインの設定
  - rustup component add rust-src
  - rustup component add rust-analysis
  - rustup component add rls

---

<!-- _class: text -->

### 環境構築手順（おすすめ）

- 事前準備

  - Docker Desktop
  - VSCode

- 手順
  - `$ git clone https://github.com/ShunichirouKamino/rust-graphql.git -b main rust-graphql`完了後、VSCode でプロジェクトルートを開く
  - `Extensions`の`RECOMMENDED`にある`Remote - Containers`を install
  - VSCode 左下の緑の部分(`Open a Remote Winndow`)をクリックすることで、自動でリモートコンテナ上で VSCode を開く
    - 初回はビルドに多少時間がかかる

---

<!-- _class: text -->

### Hello World

- `$ cd hello_world`
- `$ cargo run`
  - cargo により、コンパイルから実行までをワンライナーで行う
  - > Hello, world!
- `$ cd src`
- `$ rustc main.rs`
  - コンパイルする
- `$ ./main`
  - コンパイルして実行形式となったモジュールを実行する
  - > Hello, world!

---

<!-- _class: text -->

### Gussing Game

- `$ cd guessing_game`
- `$ cargo run`

```bash
    Finished dev [unoptimized + debuginfo] target(s) in 0.18s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 5
Please input your guess.
100
You guessed: 100
Too big!
Please input your guess.
5
You guessed: 5
You win!
```

---

<!-- _class: subtitle -->

## なぜ Rust を学ぶべきか？

---

<!-- _class: subsubtitle -->

Rust のいいところ

---

<!-- _class: box -->

Rust はメモリ安全な言語です

Rust はリッチな型システムがあります

Rust は関数型言語の仕組みを多く取り入れています

Rust は健全なコミュニティの有るエコシステムです

---

<!-- _class: subsubtitle -->

Rust はメモリ安全な言語です

---

<!-- _class: text -->

### Rust はメモリ安全な言語です

プログラミング言語によるメモリ管理には、これまで 2 種類の方法が有りました。

- プログラマが全責任をもって管理する
  - 例）C 言語

```c
    char *str;
    int length = 100; // 100byte（半角文字100文字分）確保
    str = (char*)malloc(sizeof(char) * length);
    if (str == NULL)
    {
      .. // メモリの確保に失敗した場合の処理
    }
    free(str); // メモリの解放
```

- システムが GC によって自動で不要なメモリをかき集める
  - 例）Java, Python

---

<!-- _class: text -->

### Rust はメモリ安全な言語です

Rust は、第三の方法でメモリ管理を行っております。
それが、「所有権」という考え方で、以下のルールで成り立ちます。

- ① 値は、変数が束縛しており、変数のことを所有者と呼ぶ
- ② 値の所有者は、その瞬間は 1 つの変数のみ
- ③ 所有者である変数のスコープを抜けた際に、その値は利用不可能になる
- ④ 借用という考え方により、所有権を貸し出すことができます

Rust では、このルールがあることで、プログラマ自身がメモリの管理をすることなく、かつ GC が無いにも関わらずメモリを利用することができます。

---

<!-- _class: text -->

### Rust はメモリ安全な言語です

以下の例で、"hello"の所有者は`s`です。
`s`のスコープは`f()`内のため、ルール ③ により、`f()`の外では`s`にアクセスできません。

これは、スコープの概念を持つ言語、Java でも同様の動きをします。

```rust
fn f() {
    let s = "hello";
    // sを使った処理
}
// ここではsにアクセスすることはできない
```

---

<!-- _class: text -->

### Rust はメモリ安全な言語です

以下の例で、`let s = "hello";`の時点で"hello"の所有者は`s`です。
`get_length`に`s`を渡すと、s の所有者は get_length 内の`s`に移ります。
ルール ② により、`get_length`以降`f()`では`s`にアクセスすることはできません。

```rust
fn get_length(s: String) -> usize {
    s.len()
}

fn f() {
    let s = "hello";
    let len = get_length(s);
    // ここではsにアクセスすることはできない
}

```

---

<!-- _class: text -->

### Rust はメモリ安全な言語です

別の変数に代入した場合も、ルール ② により"hello"の所有者が移ります。これを、Rust では`move`と呼びます。

```rust
fn f() {
    let s = "hello";
    let s2 = s;
    // ここではsにアクセスすることはできない
}
```

---

<!-- _class: text -->

### Rust はメモリ安全な言語です

ルール ④ の借用により、所有権を read-only でレンタルすることができます。
借用は、`s`が immutable の場合のみ（Rust ではデフォルトで変数は final）可能です。

```rust
fn get_length(s: String) -> usize {
    s.len()
}

fn f() {
    let s = "hello";
    let s2 = &s;
    // sにアクセスすることが可能
    let len = get_length(&s);
    // sにアクセスすることが可能
}
```

---

<!-- _class: text -->

### 所有権の仕組みの何がすばらしいのか？

以下の例では、println の引数として`greet`を参照していますが、その前の`let greet2 = greet`の時点で所有権は`move`しています。
これにより、

- 実行する前にコンパイル時点でエラーを出力してくれるため、メモリ解放忘れによる実行中メモリリークのようなことは起きづらくなります。
- プログラマが手出しできない GC が無いため、全てのメモリリークの要因はコード上に存在します。

```rust
fn f() {
    let greet = vec!["Hello", "What's up?", "How is everything?"];
    let greet2 = greet;

    println!("{:?}", greet); // ここで静的コンパイルエラー
}
```

---

<!-- _class: subsubtitle -->

Rust はリッチな型システムがあります

---

<!-- _class: text -->

### Rust はリッチな型システムがあります

---

<!-- _class: subsubtitle -->

Rust の難しいところ

---

<!-- _class: box -->

Rust は、学習コストが高いです

Rust は、オブジェクト指向言語ではありません

```

```
