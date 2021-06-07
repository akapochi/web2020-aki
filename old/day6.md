# 第6回

## 診断機能の組み込み

### 診断結果の表示部分の HTML の作成

今、HTML には入力を受け取る部分はあるが、表示をする部分がないので、まずはそれを作っていく。

assessment.html を以下のように変更。

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="assessment.css" />
    <title>あなたのいいところ診断</title>
  </head>
  <body>
    <h1>あなたのいいところは?</h1>
    <p>診断したい名前を入れて下さい</p>
    <input type="text" id="user-name" size="40" maxlength="20" />
    <button id="assessment">診断する</button>
    <div id="result-area"></div>
    <div id="tweet-area"></div>
    <script src="assessment.js"></script>
  </body>
</html>
```

div タグという何の意味も持たないタグを利用して　ID を付与している。

div 要素の中身はまだ空であるときは、 Chrome で表示した際の見た目は変わらない。

### JavaScript でボタンをクリックした時の動作を作成

まず、 assessment.js 冒頭 `'use strict';` の下に、以下のようなコードを追記する。

```js
'use strict';
const userNameInput = document.getElementById('user-name');
const assessmentButton = document.getElementById('assessment');
const resultDivided = document.getElementById('result-area');
const tweetDivided = document.getElementById('tweet-area');
```

各行で、設定した id を使って要素の取得を行っている。

次に、ボタンを押した時に何かしらの反応をするようにしてみる。

```js
'use strict';
const userNameInput = document.getElementById('user-name');
const assessmentButton = document.getElementById('assessment');
const resultDivided = document.getElementById('result-area');
const tweetDivided = document.getElementById('tweet-area');

assessmentButton.onclick = function() {
  console.log('ボタンが押されました');
  // TODO 診断結果表示エリアの作成
  // TODO ツイートエリアの作成
};
```

assessmentButton.onclick に関数を設定している。

この関数の書き方は、無名関数といわれる名前を持たない関数の記述法で、 それを assessmentButton というオブジェクトの onclick という プロパティに設定することで、ボタンがクリックされた時に動くようにできる。

コンソールのログに

`ボタンが押されました`

と表示されていればOK。

またこの無名関数は、`=>`を使ってさらに簡単に書くことができる。

```js
const tweetDivided = document.getElementById('tweet-area');
assessmentButton.onclick = () => {
  console.log('ボタンが押されました');
  // TODO 診断結果表示エリアの作成
  // TODO ツイートエリアの作成
};
```

この書き方をアロー関数と呼ぶ。

### 入力された名前を受け取る

userNameInput オブジェクトの value プロパティから、 テキストエリアに入力された文字列を受け取ることができる。

```js
assessmentButton.onclick = () => {
  const userName = userNameInput.value;
  console.log(userName);
  // TODO 診断結果表示エリアの作成
  // TODO ツイートエリアの作成
};
```

入力欄に「太郎」と入力して「診断する」ボタンを押したらコンソールのログに

`太郎`

と表示される。

何も入力しなかった場合は、動かしたくないのでその処理を追加してみる。

```js
assessmentButton.onclick = () => {
  const userName = userNameInput.value;
  if (userName.length === 0) {
    // 名前が空の時は処理を終了する
    return;
  }
  console.log(userName);
  // TODO 診断結果表示エリアの作成
  // TODO ツイートエリアの作成
};
```

関数の処理の中で、 return; は、戻り値なしにそこで処理を終了するという意味。

### 診断結果を表示させる

```js
  // 診断結果表示エリアの作成
  const header = document.createElement('h3');
  header.innerText = '診断結果';
  resultDivided.appendChild(header);

  const paragraph = document.createElement('p');
  const result = assessment(userName);
  paragraph.innerText = result;
  resultDivided.appendChild(paragraph);

  // TODO ツイートエリアの作成
```

document.createElement を使って `<p></p>` や `<h3></h3>` を作成し、 innerText プロパティを用いてタグの中身を設定している。

最終的には次のような HTML になる。

```html
<div id="result-area">
  <h3>診断結果</h3>
  <p>あなたのいいところは声です。あなたの特徴的な声は皆を惹きつけ、心に残ります。</p>
</div>
```

ちなみに、この HTML の場合、div が親要素、 h3 と p が子要素となっている。

### 連続して診断結果が出ないようにする

何度も「診断する」ボタンを押すと、どんどん見出しと診断結果の段落が追加されてしまう。

この問題に対応するため、診断ボタンが押されたら一度、診断結果の div 要素の子どもの要素を全て削除する処理を入れる。

```js
// 診断結果表示エリアの作成
while (resultDivided.firstChild) {
  // 子どもの要素があるかぎり削除
  resultDivided.removeChild(resultDivided.firstChild);
}
const header = document.createElement('h3');
```

while 文は、与えられた論理式が true である場合に実行し続けるというもの。

removeChild は指定された子要素を削除する関数。

つまり、この処理は診断結果表示エリアに、最初の子どもの要素が存在する限り、その最初の子どもの要素を削除し続ける、すなわち、子どもの要素を全削除する、という動作をする。

> truthy な値と falsy な値
>  
> JavaScript で if や while で受け取る値は、true 以外の値でもほとんどの場合 true と評価されるが。true にならないような値には、
>  
> - false
> - null
> - undefined
> - 空文字列 ''
> - 数値の 0
> - 数値の NaN (非数という、数値にできないことを意味する特殊な値)
>  
> というものがある。
> 
> このように if や while の条件式に与えた時、 true になる値のことを truthy な値、 false になる値のことを falsy な値と呼ぶ。

### ツイートエリアの子どもの要素を全て削除する

今、ボタンが押された時に、診断結果エリアの子どもの要素を全て削除する機能は追加したが、同じように、ボタンを押したときにツイートエリアの子どもの要素を全て削除する機能も追加する。

```js
// ツイートエリアの作成
while (tweetDivided.firstChild) {
  // 子どもの要素があるかぎり削除
  tweetDivided.removeChild(tweetDivided.firstChild);
}
```

assessment.html の tweet-area という ID がついた div タグの中に

```html
<div id="tweet-area">
  <p>テスト</p>
</div>
```

のように p タグを追加すると「テスト」という文字が表示されるが、名前を入力してボタンを押すと消えるはず。

### 練習問題

resultDivided と tweetDivided で同じ処理（子どもの要素を全消去）を2回書いてしまっているので、これを関数に切り出そう。

### 練習問題の答え

```js
/**
 * 指定した要素の子どもを全て削除する
 * @param {HTMLElement} element HTMLの要素
 */
function removeAllChildren(element) {
  while (element.firstChild) {
    // 子どもの要素があるかぎり削除
    element.removeChild(element.firstChild);
  }
}

assessmentButton.onclick = () => {
  const userName = userNameInput.value;
  if (userName.length === 0) {
    // 名前が空の時は処理を終了する
    return;
  }

  // 診断結果表示エリアの作成
  removeAllChildren(resultDivided);
  const header = document.createElement('h3');

  ...（中略）...

  // ツイートエリアの作成
  removeAllChildren(tweetDivided);
```
