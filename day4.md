# 第4回

## JavaScript のオブジェクト

ここでは

1. オブジェクト

を学ぶ。

### オブジェクト

オブジェクト（object） とは、JavaScript における値の 1 つ。次のような書き方をする。

```js
var student = {
  name: '太郎',
  age: 15
};
```

オブジェクトは、プロパティという、名前と値のセットを複数持つことができる。

オブジェクトは、 `{ }` で宣言し、その中にプロパティを `プロパティ名: 値` の形式で宣言する。

プロパティが複数ある場合には , で区切る。

#### オブジェクトのプロパティの取得

プロパティの値は取得できる。Console で以下を入力してみる。

```js
var student = {
    name: '太郎',
    age: 15
};
console.log(student.name);
console.log(student.age);
```

このように、オブジェクトのプロパティは、 `オブジェクト.プロパティ名` で取得できる。

#### オブジェクトのプロパティの代入、更新

また、プロパティは変数と同じく、任意の値を代入、更新できる。

先程の続きで Console に以下を入力していく。

```js
student.age = 16;
console.log(student.age);
```

すると `16` と、更新されたプロパティが表示されるはず。（さっきまでは15歳だった）

#### オブジェクトのプロパティに関数を設定

また、プロパティには関数を設定することもできる。

```js
var counter = {
  number: 0,
  print: function() {
    counter.number++;
    console.log(counter.number);
  }
};
```

このコードに続けて、

`counter.print()`

と記述すると、オブジェクトのプロパティ `print` に定義した関数が呼び出される。

この関数は、オブジェクト自身のプロパティである `number` をインクリメント（ 1 だけ増加）して、コンソールに表示する関数なので、 `print()` を呼び出すたびに、`counter.number` の値が 1 ずつ大きくなって表示されることになる。

Console に以下を入力していく。

```js
var counter = {
  number: 0,
  print: function() {
    counter.number++;
    console.log(counter.number);
  }
};
counter.print()
counter.print()
counter.print()
```

すると、

`1`
`2`
`3`

と表示される。

### オブジェクトを使ったプログラム

時間あてゲームを作る。

js-object.html に以下を追加

```html
<!DOCTYPE html>
<html lang="ja">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>JavaScript のオブジェクト</title>
</head>

<body>
  <h1 id="display-area"></h1>
  <script src="object.js"></script>
</body>

</html>
```

body タグ内に display-area というIDをつけた h1 タグを書いている。これが時間あてゲームの表示領域になる。

以下のような時間あてゲームを作る。

- 「10 秒だと思ったら何かキーを押して下さい」というダイアログを表示する
- ダイアログで OK を押すと、時間あてゲームがスタートする
- 何かキーを押すと、時間あてゲームが停止する
- 9.5 秒から 10.5 秒の間なら「すごい」、そうでないなら「残念」と表示する

これらを、時間あてゲームの**要件**という。

1つ目の要件から順に実装していく。

#### 1つ目の要件の実装

「10 秒だと思ったら何かキーを押して下さい」というダイアログを表示する、という要件を実装する。

object.js に以下を追加

```js
if (confirm('OKを押して10秒だと思ったら何かキーを押して下さい')) {
  // TODO スタート処理
  console.log('スタートしました');
}
```

なお、

`confirm()`

は、引数で渡した文字列をダイアログで表示する関数。ボタンが OK とキャンセルの 2 つ出てくる。

さらに `confirm()` では戻り値が意味を持ち、 `OK` ボタンが押されると `true` 、`キャンセル`ボタンが押されると `false` が返る。

ちなみに、似ているものに `alert()` があるが、 `alert()` は、警告用の表示（アラートダイアログ）を使ってカッコの中身を表示させる。

```js
var isConfirmed = confirm('こんにちは');
document.write(isConfirmed);
```

というコードを実行すると、真偽値が返ることが分かる。つまりif文に応用できる。

また、途中に

`// TODO やることの内容`

というのがあるが、慣例として、 `// TODO やることの内容` というコメント文を書くと、ここに後で実装する（今は未完成の）コードが入る、という意味になる。

#### 2つ目の要件の実装

ダイアログで OK を押すと時間あてゲームがスタートする、という要件を実装する。

object.js を以下のように変更

```js
var startTime = null;
function start() {
  startTime = Date.now();
  console.log('スタートしました');
}
if (confirm('OKを押して10秒だと思ったら何かキーを押して下さい')) {
  start(); // さっきTODOとしていた部分
}
```

`Date.now()`

を使うと現在時刻のミリ秒が取得できる。つまり、前回使った`new Date()`と似ているけど少し違う。

（`Date.now()`を使えば前回`.getTime()`を使う必要無かったのでは、とか言わないで～）

この時点で実行すると Console に

`スタートしました`

と表示される。

#### 3つ目の要件の実装

何かキーを押すと時間あてゲームが停止する、という要件を実装する。

object.js を以下のように変更

```js
var startTime = null;
function start() {
  startTime = Date.now();
  document.body.onkeydown = function() {
    console.log('ストップしました');
  };
  console.log('スタートしました');
}
if (confirm('OKを押して10秒だと思ったら何かキーを押して下さい')) {
  start();
}
```

「何かキーが押されたらコンソールに ストップしました と表示する」という処理を追加した。

`onkeydown` プロパティには、キーボードのキーが押された時の関数を指定する。

ここでは `function() { ... }` のように書いて名前のない関数を表現し、そのまま onkeydown プロパティに指定しているが、 stop 関数として切り出した方が読みやすいコードになるので書き直す。

object.js を以下のように変更

```js
var startTime = null;
function start() {
  startTime = Date.now();
  document.body.onkeydown = stop;
  console.log('スタートしました');
}
function stop() {
  console.log('stop 関数が実行されました');
}
if (confirm('OKを押して10秒だと思ったら何かキーを押して下さい')) {
  start();
}
```

`document.body.onkeydown = stop;`

は、「何かキーが押されたら stop 関数を実行する」というコード。

`stop()`ではなく`stop`と書く理由は、stop() と書くと、キーが押された時ではなく、このファイルが読み込まれた時点で stop 関数が実行され、そして、その結果が onkeydown プロパティに代入されてしまうから。

（こういう書き方なんだと覚えてしまっても良いかも？）

スタートしてキーを何回も押してみると、キーを押すたびに

`ストップしました`

と表示される。

#### 4つ目の要件の実装

9.5 秒から 10.5 秒の間なら「すごい」、そうでないなら「残念」と表示する、という要件を実装する。

object.js を以下のように変更

```js
var startTime = null;
var displayArea = document.getElementById('display-area');
function start() {
  startTime = Date.now();
  document.body.onkeydown = stop;
}
function stop() {
  var currentTime = Date.now();
  var seconds = (currentTime - startTime) / 1000;
  if (9.5 <= seconds && seconds <= 10.5) {
    displayArea.innerText = `${seconds}秒でした。すごい。`;
  } else {
    displayArea.innerText = `${seconds}秒でした。残念。`;
  }
}
if (confirm('OKを押して10秒だと思ったら何かキーを押して下さい')) {
  start();
}
```

#### オブジェクトとして表現

最後に、オブジェクトとして表現することで、コードをわかりやすくする。（ここにきてやっとオブジェクトが登場）

```js
var startTime = null;
var displayArea = document.getElementById('display-area');
```

- 開始時間
- 表示エリア
  
という2つの情報を、 game というオブジェクトにまとめると、

```js
var game = {
  startTime: null,
  displayArea: document.getElementById('display-area')
};
```

となる。他の箇所も同様にしてオブジェクトを使うと object.js は最終的に以下のようになる。

```js
var game = {
  startTime: null,
  displayArea: document.getElementById('display-area')
};
function start() {
  game.startTime = Date.now();
  document.body.onkeydown = stop;
}
function stop() {
  var currentTime = Date.now();
  var seconds = (currentTime - game.startTime) / 1000;
  if (9.5 <= seconds && seconds <= 10.5) {
    game.displayArea.innerText = `${seconds}秒でした。すごい。`;
  } else {
    game.displayArea.innerText = `${seconds}秒でした。残念。`;
  }
}
if (confirm('OKを押して10秒だと思ったら何かキーを押して下さい')) {
  start();
}
```

### 練習問題

先ほどの時間あてゲームにおいて、 start と stop という関数で書かれていた箇所も、時間あてゲームの振る舞いの一部として game オブジェクト内に表現してみよう。

<!-- ### 練習問題の答え

```js
var game = {
  startTime: null,
  displayArea: document.getElementById('display-area'),
  start: function() {
    game.startTime = Date.now();
    document.body.onkeydown = game.stop;
  },
  stop: function() {
    var currentTime = Date.now();
    var seconds = (currentTime - game.startTime) / 1000;
    if (9.5 <= seconds && seconds <= 10.5) {
      game.displayArea.innerText = seconds + '秒でした。すごい。';
    } else {
      game.displayArea.innerText = seconds + '秒でした。残念。';
    }
  }
};
if (confirm('OKを押して10秒だと思ったら何かキーを押して下さい')) {
  game.start();
}
``` -->
