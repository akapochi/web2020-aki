# 第6回

## 診断機能の開発（続き）

### 関数の実装の続き

#### 正規表現で {userName} をユーザーの名前に置き換える

最後に、 {userName} をユーザーの名前に置き換える、という機能を追加する。

まず、

```js
const result = answers[index];
```

となっている部分を

```js
let result = answers[index];
```

に書き換える。

続いて、

```js
// TODO {userName} をユーザーの名前に置き換える
```

というコメント文を消し、

```js
result = result.replace(/\{userName\}/g, userName);
```

に書き換える。

`/\{userName\}/g` と書かれている部分が正規表現で、正規表現とは、様々な文字列のパターンを表現するための記述方法のこと。

また、replace 関数は、`文字列.replace(置換前の部分文字列, 置換後の部分文字列)` の形で、文字列の一部を置き換える。

`/ ... /g` が正規表現の本体で、これは `/ ... /` で囲まれた文字列と一致する部分を全て選択することを意味する。

なお、中身の `\{` の部分は、 `{` を正規表現の中に書くための特別な記号（エスケープシーケンス）。`\}` の部分も同様。

```js
result = result.replace(/\{userName\}/g, userName);
```

つまりこの記述は、result 内の `{userName}` という文字列のパターンを全て選択し、それら全てを変数 userName の表す部分文字列に置き換えている。

`太郎のいいところは決断力です。太郎がする決断にいつも助けられる人がいます。`

`次郎のいいところは自制心です。やばいと思ったときにしっかりと衝動を抑えられる次郎が皆から評価されています。`

`太郎のいいところは決断力です。太郎がする決断にいつも助けられる人がいます。`

とログに表示されればOK。これで診断機能の実装は終わり。

### 実装のテストの追加

これまで、機能の確認には `console.log` を使い、そのログ出力を目視で確認してきたが、この方法だと人間が確認している以上、間違う可能性がある。

そこで JavaScript にもともと用意されているテストを行う機能を使う。

それが `console.assert` という関数で、以下のように書く。

```js
console.assert(
  assessment('太郎') ===
    '太郎のいいところは決断力です。太郎がする決断にいつも助けられる人がいます。',
  '診断結果の文言の特定の部分を名前に置き換える処理が正しくありません。'
);
```

第一引数には正しい時に true となるテストしたい式を記入し、  
第二引数にはテストの結果が正しくなかった時に出したいメッセージを書く。

### 練習問題

「入力が同じ名前なら、同じ診断結果を出力する」処理が正しいか、というテストを書いてみよう。

### 練習問題の答え

```js
console.assert(
  assessment('太郎') === assessment('太郎'),
  '入力が同じ名前なら同じ診断結果を出力する処理が正しくありません。'
);
```

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

<!-- ### 練習問題の答え

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
``` -->

## ツイート機能の開発

### Tweet ボタンを作る

1. [Twitter ボタン作成サイト](https://publish.twitter.com/#)へ GO ！

1. 開いたら、入力欄に`#あなたのいいところ`と入力する。

1. 次に、ボタンをカスタマイズするため、`set customization options` というリンクをクリックする。

1. カスタマイズする画面が開いたら、`Do you want to prefill the Tweet text?` という一番上の欄に 診断結果の文章などと入力する。

1. 入力が完了したら右下の `Update` という水色のボタンをクリックする。

1. そしたら、元の画面に戻るので、 Copy Code ボタンをクリックする。

1. コピーした文字列を assessment.html の
  
   ```html
   <div id="tweet-area"> </div>
   ```

    の中に貼り付ける。

### Tweet ボタンをプログラムから扱ってみよう

今コピペしてきたものを JavaScript を用いて再現するということをする。

a タグは

```html
<a
  href="https://twitter.com/intent/tweet?button_hashtag=あなたのいいところ&ref_src=twsrc%5Etfw"
  class="twitter-hashtag-button"
  data-text="診断結果の文章"
  data-show-count="false"
  >Tweet #あなたのいいところ</a>
```

のように、script タグは

```html
<script
  async
  src="https://platform.twitter.com/widgets.js"
  charset="utf-8"
></script>
```

のようになっているはず。

これらをプログラムに書いてみる。

まずは、 assessment.js を以下のように書き換える。

```js
// ツイートエリアの作成
removeAllChildren(tweetDivided);
const anchor = document.createElement('a');
const hrefValue =
  'https://twitter.com/intent/tweet?button_hashtag=あなたのいいところ&ref_src=twsrc%5Etfw';

anchor.setAttribute('href', hrefValue);
anchor.className = 'twitter-hashtag-button';
anchor.setAttribute('data-text', '診断結果の文章');
anchor.innerText = 'Tweet #あなたのいいところ';

tweetDivided.appendChild(anchor);

const script = document.createElement('script');
script.setAttribute('src', 'https://platform.twitter.com/widgets.js');
tweetDivided.appendChild(script);
```

そして、先ほど assessment.html に貼り付けた内容から、内側にある a タグと script タグを削除し、assessment.html を以下のようにする。

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

これで再現できたことになる。

### ハッシュタグの設定

先ほどの a タグの href 属性の URI をよく見ると、 `?button_hashtag=あなたのいいところ` というのが含まれているのがわかる。

#### URI とは

URI （ユーアールアイ）とは、インターネット上などにある情報やサービスを一意に識別するためのデータ形式で、 Uniform Resource Identifier の略称。

なおインターネット上の場所に限定したものとして、URL (Uniform Resource Locator の略称)と呼ぶこともある。

#### URI の各箇所の名前

`https://twitter.com/intent/tweet?button_hashtag=あなたのいいところ&ref_src=twsrc%5Etfw`

- `https` の部分は、URI のスキーム
- `twitter.com` の部分は、ホスト名
- `/intent/tweet` の部分は、リソース名
- ? 以降の部分は、クエリ

と呼ぶ。

この URI のクエリに日本語などの 半角英数以外 の文字を含めるには、URI エンコード を使ったほうがよい。

#### URI エンコードとは

URI エンコードとは、URI のクエリに含めることのできない文字のために、 それらの文字を %（パーセント） という記号から始まる 16 進数で表現して含めるようにするための変換方法のこと。

URI エンコードを、URL エンコード や パーセントエンコーディングなどと言うこともある。

JavaScript では、

- encodeURIComponent 関数で 文字列を URI エンコードされたものへ変換
- decodeURIComponent 関数で URI エンコードされた文字列から元のものへ復元

を行える。

```js
encodeURIComponent('あ');
> "%E3%81%82"
decodeURIComponent('%E3%81%82');
> "あ"
```

---

URI エンコードを使ってハッシュタグを `#あなたのいいところ` になるようにコードを下記のように変えよう。

```js
const hrefValue =
  'https://twitter.com/intent/tweet?button_hashtag=' +
  encodeURIComponent('あなたのいいところ') +
  '&ref_src=twsrc%5Etfw';
```

### ツイートの文章に診断結果をのせる

ツイートの文章は data-text 属性で設定している。今は

```js
anchor.setAttribute('data-text', '診断結果の文章');
```

となっているが、これを

```js
anchor.setAttribute('data-text', result);
```

に変えると、ツイートの文章が診断結果に変わる。

### 最終的なコード

最終的に assessment.js は以下のようになる。

```js
'use strict';
const userNameInput = document.getElementById('user-name');
const assessmentButton = document.getElementById('assessment');
const resultDivided = document.getElementById('result-area');
const tweetDivided = document.getElementById('tweet-area');

/**
 * 指定した要素の子どもを全て除去する
 * @param {HTMLElement} element HTMLの要素
 */
function removeAllChildren(element) {
  while (element.firstChild) {
    // 子どもの要素があるかぎり除去
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
  header.innerText = '診断結果';
  resultDivided.appendChild(header);

  const paragraph = document.createElement('p');
  const result = assessment(userName);
  paragraph.innerText = result;
  resultDivided.appendChild(paragraph);

  // ツイートエリアの作成
  removeAllChildren(tweetDivided);
  const anchor = document.createElement('a');
  const hrefValue =
    'https://twitter.com/intent/tweet?button_hashtag=' +
    encodeURIComponent('あなたのいいところ') +
    '&ref_src=twsrc%5Etfw';
  anchor.setAttribute('href', hrefValue);
  anchor.className = 'twitter-hashtag-button';
  anchor.setAttribute('data-text', result);
  anchor.innerText = 'Tweet #あなたのいいところ';
  tweetDivided.appendChild(anchor);

  // widgets.js の設定
  const script = document.createElement('script');
  script.setAttribute('src', 'https://platform.twitter.com/widgets.js');
  tweetDivided.appendChild(script);
};

const answers = [
  '{userName}のいいところは声です。{userName}の特徴的な声はみなを惹きつけ、心に残ります。',
  '{userName}のいいところはまなざしです。{userName}に見つめられた人は、気になって仕方がないでしょう。',
  '{userName}のいいところは情熱です。{userName}の情熱に周りの人は感化されます。',
  '{userName}のいいところは厳しさです。{userName}の厳しさがものごとをいつも成功に導きます。',
  '{userName}のいいところは知識です。博識な{userName}を多くの人が頼りにしています。',
  '{userName}のいいところはユニークさです。{userName}だけのその特徴が皆を楽しくさせます。',
  '{userName}のいいところは用心深さです。{userName}の洞察に、多くの人が助けられます。',
  '{userName}のいいところは見た目です。内側から溢れ出る{userName}の良さに皆が気を惹かれます。',
  '{userName}のいいところは決断力です。{userName}がする決断にいつも助けられる人がいます。',
  '{userName}のいいところは思いやりです。{userName}に気をかけてもらった多くの人が感謝しています。',
  '{userName}のいいところは感受性です。{userName}が感じたことに皆が共感し、わかりあうことができます。',
  '{userName}のいいところは節度です。強引すぎない{userName}の考えに皆が感謝しています。',
  '{userName}のいいところは好奇心です。新しいことに向かっていく{userName}の心構えが多くの人に魅力的に映ります。',
  '{userName}のいいところは気配りです。{userName}の配慮が多くの人を救っています。',
  '{userName}のいいところはその全てです。ありのままの{userName}自身がいいところなのです。',
  '{userName}のいいところは自制心です。やばいと思ったときにしっかりと衝動を抑えられる{userName}が皆から評価されています。'
];

/**
 * 名前の文字列を渡すと診断結果を返す関数
 * @param {string} userName ユーザーの名前
 * @return {string} 診断結果
 */
function assessment(userName) {
  // 全文字のコード番号を取得してそれを足し合わせる
  let sumOfCharCode = 0;
  for (let i = 0; i < userName.length; i++) {
    sumOfCharCode = sumOfCharCode + userName.charCodeAt(i);
  }

  // 文字のコード番号の合計を回答の数で割って添字の数値を求める
  const index = sumOfCharCode % answers.length;
  let result = answers[index];

  result = result.replace(/{userName}/g, userName);
  return result;
}

// テストコード
console.assert(
  assessment('太郎') ===
    '太郎のいいところは決断力です。太郎がする決断にいつも助けられる人がいます。',
  '診断結果の文言の特定の部分を名前に置き換える処理が正しくありません。'
);
console.assert(
  assessment('太郎') === assessment('太郎'),
  '入力が同じ名前なら同じ診断結果を出力する処理が正しくありません。'
);
```

### 練習問題

あなたのいいところ診断で、テキストフィールド上で Enter が押された際にも診断をしてくれるように改良しよう。

テキストフィールドの要素の onkeydown プロパティに、

```js
userNameInput.onkeydown = event => {
  if (event.key === 'Enter') {
    // TODO ボタンのonclick() 処理を呼び出す
  }
};
```

のように無名関数を代入することでキー入力時の処理が実装できる。

<!-- ### 練習問題の答え

```js
userNameInput.onkeydown = (event) => {
  if (event.key === 'Enter') {
    assessmentButton.onclick();
  }
};
``` -->