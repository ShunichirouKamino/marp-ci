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

- なぜ Rust を学ぶべきか？
- とりあえず Hello World をやってみよう！
- Rust が流行りそうな理由
- ソースコードを読んでみよう

---

<!-- _class: subtitle -->

## なぜ Rust を学ぶべきか？

---

<!-- _class: subsubtitle -->

Rust のいいところ

---

<!-- _class: box -->

Rust は、メモリ安全な言語です

Rust は、関数型言語の仕組みを多く取り入れています

Rust は、リッチな型システムがあります

Rust は、健全なコミュニティの有るエコシステムです

---

<!-- _class: subsubtitle -->

Rust の難しいところ

---

<!-- _class: box -->

Rust は、学習コストが高いです

Rust は、オブジェクト指向言語ではありません

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
