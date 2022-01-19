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

Rust はエラー処理が分かりやすい

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

- struct
  - 直積型です。C 言語でいう構造体と同じです。メソッドの無い class のイメージです。

```rust
#[derive(Debug)]
struct Person {
    name: String,
    age: u8,
}

fn f() {
    let taro = Person {
        name: String::from("taro"),
        age: 27,
    };
    println!("{:?}", taro); // Person { name: "taro", age: 27 }
}
```

---

<!-- _class: text -->

### Rust はリッチな型システムがあります

- enum
  - 列挙型です。Java の Enum と同じです。

```rust
#[derive(Debug)]
enum IpAddrKind {
    V4,
    V6,
}

fn f() {
    let v4 = IpAddrKind::V4;
    let v6 = IpAddrKind::V6;

    println!("{:?}, {:?}", v4, v6); // V4, V6
}
```

---

<!-- _class: text -->

### Rust はリッチな型システムがあります

- 直和型
  - 取りうるすべての型の羅列です。TypeScript では`a = number | string`のように表現され、Java では Java17 以降、`sealed`構文と`record`構文により実現されます。

```rust
/// Actionは、ToDoリストにおけるアクションを示す直和型です。
/// 複数のstructの列挙型で表現され、AddにもDoneにもListにもなれます。
pub enum Action {
    Add {
        text: String,
    },
    Done {
        position: usize,
    },
    List,
}
```

---

<!-- _class: text -->

### Rust はリッチな型システムがあります

- trait
  - 共通の振る舞いを定義します。struct に付与することで、クラスのような振る舞いが可能です。

```rust
struct Person {
    name: String,
    age: u8,
}

pub trait Judge {
    fn isOver30(&self) -> bool;
}

impl Judge for Person {
    fn isOver30(&self) -> bool {
        self.age > 30
    }
}
```

---

<!-- _class: text -->

### 型システムが豊富なことは何が素晴らしいのか？

ドメイン知識を実装する幅が広がります。例えば Java では列挙型に対して、以下の制約が有ります。

- 変数が持てない
- 列挙値毎に定数の数は固定

```java
enum IpAddr {
    V4("127.0.0.1"),
    V6("::1"),

    private final String loopBack;

    public String getLoopBack(){
        return this.loopBack;
    }
}
```

---

<!-- _class: text -->

### 型システムが豊富なことは何が素晴らしいのか？

Rust では、以下のように列挙値をそれぞれ別の型で表現できます。（直和型）
この仕様により、ドメイン知識を実装する幅が広がります。

```rust
enum IpAddr {
    V4 (u8, u8, u8, u8), // 8byte整数値を4つ持つタプル
    V6 { loopBack: String }, // Stringを1つもつstruct
}

fn f() {
    let v4LoopBack = IpAddr::V4(127, 0, 0, 1);
    let v6LoopBack = IpAddr::V6 {
        ip: "::1".to_string(),
    };
    println!("{:?}, {:?}", v4LoopBack, v6LoopBack); // V4(127, 0, 0, 1), V6 { ip: "::1" }
}
```

---

<!-- _class: text -->

### 型システムが豊富なことは何が素晴らしいのか？

パターンマッチングによる分岐処理が簡潔に書けます。

```rust
pub enum Action {
    Add {text: String, }, // 文字列変数を保持する構造体
    Done {position: usize, },  // usize型の数値変数を保持する構造体
    List,  // 列挙定数のみ
}

fn f() {
    let action = XXX::from_args(); // コマンドラインから何らかの値を取得

    match action {
        Add { text } => ..., // 文字列の場合の処理
        List => ... , // 何も指定されなかった場合の処理
        Done { position } => ... , // 数値の場合の処理
};

```

---

<!-- _class: subsubtitle -->

Rust はエラー処理が分かりやすい

---

<!-- _class: text -->

### Rust はエラー処理が分かりやすい

- Java におけるエラー処理
  - 検査例外の Exception により、上位レイヤでキャッチします。
  - 上位のモジュールでは、`try-catch`の記載が必須となります。

```java
public void any() {
    try {
        final var file = open("../input/input.json");
        // 何らかの処理
    } catch (IOException ex) {
        throw ex;
    }
}

public File open(String fileName) throws IOException {
    return new File(fileName);
}
```

---

<!-- _class: text -->

### Rust はエラー処理が分かりやすい

- Java におけるエラー処理
  - 非検査例外の Exception により、Runtime 時の Exception を定義します。
    - `NullPointerException`, `ArrayIndexOutOfBoundsException`等
  - 上位のモジュールでは、`try-catch`の記載が任意となります。

```java
public void any() {
    final var file = open("../input/input.json");
}

public InputStream open(String fileName) {
    try {
        return new FileInputStream(fileName);
    } catch (final FileNotFoundException e) {
        throw new UncheckedIOException(e);
    }
}
```

---

<!-- _class: text -->

### Rust はエラー処理が分かりやすい

Java におけるエラー処理の課題は？

- 検査例外と非検査例外の使い分けとしては、検査例外は、「呼び出し側で発生を避けられないもの」です。しかし境界が曖昧で、例えば`NullPointerException`は非検査例外（呼び出し側で発生を避けられるもの）の定義ですが、実際は実行するまで見逃されることがほとんどです。
- 結果として、大原則である「Exception を握りつぶしてはいけない」について、実装時に見逃すことになります。上記の例で非検査例外である`NullPointerException`は、コンパイル時に検査できません。
- Exception の実装が、他の Class の実装と異なるためある程度学習コストがかかります。実際に Java を書いている人でも、Exception を何となく実装したり、Exception そのものの実装をしたことが無い人がほとんどではないでしょうか。

---

<!-- _class: text -->

### Rust はエラー処理が分かりやすい

- VBA におけるエラー処理
  - VBA や C 言語のような例外機構が存在しない言語では、GoTo にてエラーハンドリングを行うことがあります。

```vb
Sub 実行()
    On Error GoTo Catch ' エラーが発生したら Catch の行へ処理を飛ばす
    Dim i As Integer
    i = "a"  ' エラー発生
    Exit Sub ' 正常に処理が行われたときに Catch: の処理を行わないように、ここで関数を抜ける
    Catch:
    ' エラー処理
End Sub
```

---

<!-- _class: text -->

### Rust はエラー処理が分かりやすい

VBA におけるエラー処理の課題は？

- GoTo 文は、何よりも先に解析されるため、非常に強力な構文です。結果として、処理がどこに飛ぶか分からないスパゲッティコードが生まれる原因となります。故に、GoTo 文を禁止する案件も少なくありません。
- そうはいっても、書き換えて関数呼び出しのネストが深い処理になってしまう場合や、メモリのクローズ忘れを防ぐ等、有用なシーンでの共通処理を定義しておくことができるため、完全には無くならないのが現状です。

---

<!-- _class: text -->

### Rust はエラー処理が分かりやすい

Rust の回復可能なエラー処理（検査例外）は、以下の 2 つの列挙型を返り値とすることで実現されます。

```rust
pub enum Result<T, E> {
    Ok(T),
    Err(E),
}

pub enum Option<T> {
    None,
    Some(T),
}
```

---

<!-- _class: text -->

### Rust はエラー処理が分かりやすい

Rust の回復不可能なエラー処理（非検査例外）は、`panic!`を発生させることで実現されます。panic!が発生すると、デフォルトではこれまで確保したメモリ等を自動的に開放していきます。

```rust
fn main() {
    panic!("crash and burn");  //クラッシュして炎上
}
```

---

<!-- _class: subsubtitle -->

Rust の難しいところ

---

<!-- _class: box -->

Rust は、学習コストが高いです

Rust は、オブジェクト指向言語ではありません

```

```

```

```

```

```
