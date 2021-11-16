# 1. Javascript の基本

## Node.js の環境について

Javascript(以下 JS)はブラウザでの動作を目的とした言語ですが、サーバーサイドでも実行をすることが可能です。
実行するためには Node.js と呼ばれる JS 実行環境が必要となります。

React のコーディングにはこの Node.js 環境が必須となるため、まずは Node.js をインストールしてみましょう。

### ツールを使用して Node.js をインストールする。

Node.js は公式から直接実行ファイルをインストールすることが可能ですが、バージョンマネージャを使用してインストールすることが一般的です。これは、Node.js のアップデートが比較的頻繁に行われていること、セキュリティ関連のアップデートがその中にはよく含まれていることが理由として挙げられます。

主に使われているバージョンマネージャは以下の二つです。

- n
- nvm

今回は、nvm を使用して最新版の Node.js をインストールします。

(環境を汚したくない場合は、同ディレクトリにある Dockerfile を使用してください。リポジトリの root にある `docker-compose.yml` の `build` を 1_base_syntax に書き換えれば、ubuntu 環境が起動します)

#### 1. nvm (node version manager) のインストール

まず、nvm の公式から nvm をインストールしましょう。
https://github.com/nvm-sh/nvm

Linux 環境の場合、以下のコマンドで nvm をインストールできます。
なお、コマンドのバージョン箇所は最新版のものに差し替えてください。

```
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
```

問題がなければ、以下のコマンドが実行できるはずです。

```
command -v nvm
```

#### 2. nvm で最新版 Node.js をインストール

(ターミナルに fish を使用している場合は動作しない場合があります。fisher で nvm.fish をインストールしてください)
ターミナルを再起動し、以下のコマンドを実行しましょう

```
nvm list-remote
```

このコマンドは、以下のページに存在する Node.js のバージョン一覧を出力するコマンドです
https://nodejs.org/dist/

このコマンドによって出力される値のうち、latest と表記されるバージョンが最新版のバージョンとなります。
本テキストを執筆時、バージョンは v17.1.0 でした。なので、v17.1.0 をインストールします。

```
nvm install 17.1.0
```

これにてインストールは完了です。
問題ないか確認するために、以下のコマンドで Node.js のバージョンを確認してみましょう。

```
node -v
```

ちなみに、今回のように最新版をインストールしたい場合は、バージョンを調べずに以下のコマンドでインストールが可能です。

```
nvm install node
```

以上で Node.js 環境の用意は終了です。

## 基本文法

基本となる文法一覧を記載します。
なお、Python などと同じく node コマンドで手軽に試すことができるので、試してみるとよいかもしれません。

### 変数宣言

var で宣言します。

```
var foo = 1
var bar = "bar"

console.log(foo)
# >> 1
console.log(bar)
# >> bar
```

### 関数

関数は以下のように使用できます。

```
function foo () {
  console.log(1);
};

function bar (text) {
  return text + 'bar';
};

foo()
# >> 1
baz = 'baz';
console.log(bar(baz));
# >> '2bar'
```

なお、上記の例では関数 bar に対して number 型の 2 を引数として与えました。にもかかわらず、 `text + 'bar'`にてエラーが発生していません。
これは、暗黙の型変換といって、明確に型変換が必要な場合勝手に JS がそれを判断して型を変換する仕組みが動作したためです。今回の場合、文字列に対して何かを+する場合は、される側は必ずテキストである必要があるため、number 型 2 が sting 型'2'へ型変換され、2bar といったテキストとなりました。
`+`の場合は別言語と同じような処理であるため問題はありませんが、`-`の場合は注意が必要です。気になる方は、ぜひ以下の項目についても試してみてください。

```
console.log('baz' - 2);
console.log('2' - 2);
console.log(true - 2);
```

#### 即時実行関数

即時実行関数 (即時関数) は、宣言したその場で関数を実行させる処理のことを指します。Python で言うところの lambda です。

具体的には、無名関数と呼ばれるものをグローバル化演算子`()`で囲い、末尾に()を記載することで機能します。

```
(function (arg){
  console.log(arg)
})('foo')
# >> foo

var bar = (function (arg){
  return arg
})('bar')
console.log(bar)
# >> bar
```

末尾の()は即時実行関数に与える引数です。
ちなみに、即時実行関数に返り値を持たせる必要がない場合、暗黙の型変換を利用して以下の方法で即時実行関数を記載することも可能です。

```
!function (arg){
  console.log(arg)
}('foo')
# >> foo

+function (arg){
  console.log(arg)
}('bar')
# >> bar

-function (arg){
  console.log(arg)
}('baz')
# >> baz
```

### if 文

if 文は以下の通りです。

```
function foo (bool) {
  if(bool){
    return 'A';
  } else {
    return 'B';
  };
};

console.log(foo(true))
# >> A
console.log(foo(false))
# >> B
console.log(foo(0))
# >> B
console.log(foo('false'))
# >> A
```

if 文を連続させて使用したい場合は、`else if`が使用できます。

### for 文

ループはいくつか種類がありますが、今回は`for`と`for in`について紹介します。

まずは for 文です。
for 文は、`for(宣言;終了条件;変化)`のように記載します。

```
function foo(){
  for (var i = 0; i < 9; i++) {
    str = str + i;
  }
  return str
}
console.log(foo())
# >> 0123456789
```

次に`for in`です。
`for in`は列挙可能プロパティを必要とします。
列挙可能プロパティには、配列の他、object 型が使用できます。
なお、環境によっては**順序保証がされていない**ことに注意してください。

```
for(item in ['foo', 'bar', 'baz']){
  console.log(item)
}
# >> 0
# >> 1
# >> 2

for(item in { 0: 'foo', 1: 'bar', 2: 'baz'}){
  console.log(item)
}
# >> 0
# >> 1
# >> 2
```

実行結果からわかるように、値ではなく index や key をループします。

### エラーハンドリング

エラーハンドリングには、try catch 文を使用します。

```
try {
  var foo = {'a': 1, 'b': 2}
  console.log(foo.c)
  console.log(foo.c.d)
} catch (e){
  console.error(e)
} finally {
  console.log('finally')
}
# >> undefined
# >> TypeError: Cannot read properties of undefined (reading 'd')
# >>     at REPL103:4:21
# >>     at Script.runInThisContext (node:vm:129:12)
# >>     at REPLServer.defaultEval (node:repl:562:29)
# >>     at bound (node:domain:421:15)
# >>     at REPLServer.runBound [as eval] (node:domain:432:12)
# >>     at REPLServer.onLine (node:repl:889:10)
# >>     at REPLServer.emit (node:events:402:35)
# >>     at REPLServer.emit (node:domain:475:12)
# >>     at REPLServer.Interface._onLine (node:readline:487:10)
# >>     at REPLServer.Interface._line (node:readline:864:8)
# >> finally
```

今回の場合は、undefined の値のプロパティ d にアクセスしようとしてエラーが発生しました。
エラーが発生した時点でその処理は中断され、catch 節に移動しその中の処理を実行します。
catch 節の引数(e)は発生したエラー内容が記載されており、エラーを区別したい場合は catch の中で if 文を使用して分岐させる必要があります。
finally は、try catch 文の処理がすべて実行された後に実行される処理です。

### モジュールのインポート/エクスポート

Node.js では、関数のモジュール化や別ファイルでのそのインポートが可能です。
具体的に確認してみましょう。

今回は、例としてexportされる関数が記載されたファイル`exporter.js`を用意しました。
では、空のファイル`importer.js`に`exporter.js`の関数`foo()`をインポートして実行するように書いてみましょう。

インポートには、`require`文を使用します。
require(`相対/絶対Path`)でインポートすることができます。
今回の場合は、以下のように記載します。

```importer.js
var bar = require('./exporter.js');
console.log(bar());
```

実行し、問題なく文字列が出力されれば成功です。
これで、`exporter.js`の関数`foo()`を`bar`として実行することができました。
なお、今回はインポート時にfooではなくbarという名前を当てましたが、別の名前を当てることができるということを知ってもらうために別の名前を割り当てているだけであり、**理由がない限りは同じ名前を当てることが一般的です**。

以上でのJavascriptの基本を終了します。
dockerを使用している場合は、忘れずに`docker-compose down --rmi all --volumes --remove-orphans`でコンテナを削除してください。

次は`2_ECMAScript`となります。
