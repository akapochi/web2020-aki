# 第5回

## CSS を使ったプログラミング

### CSS の適用

css-programming.html に以下を追加

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>CSS を使ったプログラミング</title>
    <link rel="stylesheet" href="css-programming.css" />
  </head>
  <body>
    <h1 id="header">CSS を使ったプログラミング</h1>
    <script src="animation.js"></script>
  </body>
</html>
```

CSS を適用するにあたり、新しくクラス (class) という機能を使ってみる。今書いた

```html
<h1 id="header">CSS を使ったプログラミング</h1>
```

を

```html
<h1 id="header" class="face">CSS を使ったプログラミング</h1>
```

のように class を追加してみよう。

class 属性は、その名前にもとづいて HTML タグを分類する機能。特定の class を対象にし、CSS で見た目を指定したりできる。

class に対してCSSを適用するためには、.（ドット）を書いてからクラス名を書いたものをセレクタにする。

css-programming.css に以下を追加

```css
.face {
  color: darkblue;
}
```

続いて、css-programming.css を以下のように変更することで、x 軸方向（横を貫く軸）に 60 度回転させる。

```css
.face {
  color: darkblue;
  transform: rotateX(60deg);
}
```

回転していることが確認できたら、CSS から今追加した transform: rotateX(60deg); を削除する。

```css
.face {
  color: darkblue;
}
```

### class 属性と id 属性

今回は class というものを使っている。この class は同じクラス名を複数の要素に指定でき、同じクラス名の要素全てにスタイルを設定できる。

一方、 id 属性は、1 つの要素を特定するために設定するもので、文章全体で 1 つの要素にしか設定できない。

### JavaScript から CSS を操作する

animation.js に以下を追加

```js
var header = document.getElementById('header');
header.style.transform = 'rotateX(60deg)';
```

2行目は、CSS に 'rotateX(60deg)' という文字列を書き加えるイメージ。

> ### そのほかの transform
> 
> rotateX （x 軸方向に回転）のほかにも、 rotateY や rotateZ もある。また、scale のようにサイズを変えるものもある。
> 
> なお、複数の変形を指定する場合は以下のように半角スペースを挟んで書く。
> 
> `header.style.transform = 'rotateX(60deg) rotateZ(10deg) scale(2)';`

### アニメーションさせよう

パラパラ漫画のようにしてアニメーションさせてみよう。

ここでは、 20 ミリ秒に一度、表示の更新をして、更新させる度に、回転の角度を 6 度ずつ増やす。

animation.js を以下のように変更

```js
var header = document.getElementById('header');
var degree = 0;
function rotateHeader() {
  degree = degree + 6;
  header.style.transform = 'rotateX(' + degree + 'deg)';
}
setInterval(rotateHeader, 20);
```

setInterval 関数を使って、 rotateHeader 関数を、20 ミリ秒ごとに繰り返し実行している。

### 色が変わるアニメーションをさせてみよう

回転して裏になった時、色が変わるようにしてみる。

CSS ファイルに新しい back というクラスを用意しよう。 css-programming.css を以下のように変更。

```css
.face {
  color: darkblue;
}
.back {
  color: lightgray;
}
```

次に JavaScript コードで、回転して裏を向いた瞬間に class を変更するコードを書く。 animation.js を以下のように変更。

```js
var header = document.getElementById('header');
var degree = 0;
function rotateHeader() {
  degree = degree + 6;
  degree = degree % 360;
  if ((0 <= degree && degree < 90) || (270 <= degree && degree < 360)) {
    header.className = 'face';
  } else {
    header.className = 'back';
  }
  header.style.transform = 'rotateX(' + degree + 'deg)';
}
setInterval(rotateHeader, 20);
```

現在の角度に応じて、見出しの class 名を、表の時には face、裏の時には back と設定している。

先ほどは JavaScript から直接 CSS を編集したが、このようにあらかじめ用意しておいた HTML 要素のクラスを、JavaScript で動的に付け替えることによって表示を切り替える方法もある。

### 練習問題

先ほどの回転する見出しを、

1. 裏表を赤色 (red) に設定して、
2. 裏側を向いている時は少し透明にしてみよう。

### 練習問題の答え

css-programming.css を以下のように変更

```css
.face {
  color: red;
}
.back {
  color: red;
  opacity: 0.4;
}
```

## 「あなたの良いところ診断」の見た目部分の作成

「あなたのいいところ診断」を、以下のような要件を持つページとする。

- いいところを診断できる
  1. 名前を入力すると診断できる
  2. 同じ名前なら、必ず同じ診断結果が出る
  3. 診断後に、自分の名前入りの診断結果が表示される
- 診断結果をツイートボタンでツイートできる

### 実装の準備

workspace フォルダに assessment というプロジェクトフォルダを作成しよう。そしてその assessment フォルダの中に、

- assessment.html
- assessment.js
- assessment.css

という 3 つのファイルを作成する。

assessment.html に以下を追加。

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>あなたのいいところ診断</title>
    <link rel="stylesheet" href="assessment.css" />
  </head>
  <body>
    <script src="assessment.js"></script>
  </body>
</html>
```

これでひな形ができたので、ここから HTML に中身を追加する。

### HTML の実装

assessment.html を以下のように変更。

```html
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>あなたのいいところ診断</title>
    <link rel="stylesheet" href="assessment.css" />
  </head>
  <body>
    <h1>あなたのいいところは?</h1>
    <p>診断したい名前を入れて下さい</p>
    <input type="text" id="user-name" size="40" maxlength="20" />
    <button id="assessment">診断する</button>
    <script src="assessment.js"></script>
  </body>
</html>
```

input タグは、入力欄を作るためのタグ。

- **type 属性**では、入力の形式（テキストやパスワード、チェックボックス、ラジオボタンなど）を指定している。
- **size 属性**では、入力欄（テキストフィールド）の大きさを設定している。
- **maxlength 属性**では、この入力欄に入力できる最大の文字数を設定している。

### CSS の実装

#### 背景色と文字色の設定

assessment.css に以下を追加

```css
body {
  background-color: #04a6eb;
  color: #fdffff;
}
```

16 進数カラーコードという書き方を使っている。

#### 横幅の太さと余白の設定

assessment.css を以下のように変更

```css
body {
  background-color: #04a6eb;
  color: #fdffff;
  width: 500px;
  margin-right: auto;
  margin-left: auto;
}
```

- **width: 500px;** で、幅を 500px に設定している
- **margin-right: auto;** で、右側のマージンを自動調節にしている
- **margin-left: auto;** で、左側のマージンを自動調節にしている

#### ボタンの CSS を設定

assessment.css を以下のように変更

```css
body {
  background-color: #04a6eb;
  color: #fdffff;
  width: 500px;
  margin-right: auto;
  margin-left: auto;
}
button {
  padding: 5px 20px;
  background-color: #337ab7;
  border-style: none;
}
```

padding （パディング）のプロパティでは、要素の内側の余白を設定でき、ここでは、上下を 5px、左右を 20px に設定している。

- margin は border より外側の余白
- padding は border より内側の余白

#### 入力欄の CSS を設定

入力欄の高さをボタンの高さと合うように設定する。

assessment.css を以下のように変更

```css
body {
  background-color: #04a6eb;
  color: #fdffff;
  width: 500px;
  margin-right: auto;
  margin-left: auto;
}
button {
  padding: 5px 20px;
  background-color: #337ab7;
  border-style: none;
}
input {
  height: 20px;
}
```

### 練習問題

もっと見栄えを良くするために、先ほどのボタンの文字色を、body の文字色（#fdffff）と一緒にしよう。

### 練習問題の答え

```css
body {
  background-color: #04a6eb;
  color: #fdffff;
  width: 500px;
  margin-right: auto;
  margin-left: auto;
}
button {
  padding: 5px 20px;
  background-color: #337ab7;
  color: #fdffff;
  border-style: none;
}
input {
  height: 20px;
}
```

## 診断機能の開発

### 診断機能の要件

1. 診断結果のパターンのデータが取得できる
1. 名前を入力すると診断結果が出力される関数
     1. 入力が同じ名前なら、同じ診断結果を出力する処理
     2. 診断結果の文章のうち名前の部分を、入力された名前に置き換える処理

### 診断結果のパターンを作る

名前に置き換えられる予定の部分を {userName} として、以下のようなパターンにする。

> {userName}のいいところは声です。{userName}の特徴的な声は皆を惹きつけ、心に残ります。  
> {userName}のいいところはまなざしです。{userName}に見つめられた人は、気になって仕方がないでしょう。  
> {userName}のいいところは情熱です。{userName}の情熱に周りの人は感化されます。  
> {userName}のいいところは厳しさです。{userName}の厳しさがものごとをいつも成功に導きます。  
> {userName}のいいところは知識です。博識な{userName}を多くの人が頼りにしています。  
> {userName}のいいところはユニークさです。{userName}だけのその特徴が皆を楽しくさせます。  
> {userName}のいいところは用心深さです。{userName}の洞察に、多くの人が助けられます。  
> {userName}のいいところは見た目です。内側から溢れ出る{userName}の良さに皆が気を惹かれます。  
> {userName}のいいところは決断力です。{userName}がする決断にいつも助けられる人がいます。  
> {userName}のいいところは思いやりです。{userName}に気をかけてもらった多くの人が感謝しています。  
> {userName}のいいところは感受性です。{userName}が感じたことに皆が共感し、わかりあうことができます。  
> {userName}のいいところは節度です。強引すぎない{userName}の考えに皆が感謝しています。  
> {userName}のいいところは好奇心です。新しいことに向かっていく{userName}の心構えが多くの人に魅力的に映ります。  
> {userName}のいいところは気配りです。{userName}の配慮が多くの人を救っています。  
> {userName}のいいところはその全てです。ありのままの{userName}自身がいいところなのです。  
> {userName}のいいところは自制心です。やばいと思ったときにしっかりと衝動を抑えられる{userName}が皆から評価されています。

これを JavaScript の文字列の配列として実装する。

assessment.js に以下を追加（文字列部分はコピペ推奨）

```js
'use strict';
const answers = [
  '{userName}のいいところは声です。{userName}の特徴的な声は皆を惹きつけ、心に残ります。',
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
```

#### マルチカーソル機能の使い方

すべての行を `'` で囲み、`,` で配列の要素を区切ればいいのだが、全て手動で行っていると面倒なので、マルチカーソル機能を使おう。

Mac ならば、

- Command + Option + ↓
- Alt + Shift + ドラッグ

でマルチカーソル機能を使える（らしい）。

#### const について

今回は、 var ではなく const という宣言を利用している。 const は、一度代入すると再代入できない（変数の値を後から変更できない）変数を宣言するときに用いられる。

ところで、ここまでは変数の宣言に var を利用してきたが、現バージョンの JavaScript では var は使わないことが推奨されている。

何を使うかというと const と let を使う。

- const は、再代入することのできない変数の宣言
- let は、再代入することのできる変数の宣言

というような使い分けをする。

ちなみに const は、定数という意味の英語 constant（コンスタント） の略。

また、余談だが、 const と let を使って、{ ～ }の**中**で宣言された変数は、{ ～ }の中でしか有効ではない。こういった変数の有効な範囲をスコープ（scope）という。

#### use strict について

今回のコードの1行目にある `'use strict';` は、宣言後の記述ミスをエラーとして表示してくれる機能を呼び出すための記述。日本語では「厳格モードを使う」という意味。

### 関数の実装

要件は、

1. 名前を入力すると診断結果が出力される関数
    1. 入力が同じ名前なら、同じ診断結果を出力する処理
    1. 診断結果の文章のうち名前の部分を、入力された名前に置き換える処理

assessment.js を以下のように変更

```js
'use strict';
const answers = [
  '{userName}のいいところは声です。{userName}の特徴的な声は皆を惹きつけ、心に残ります。',
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
  // TODO 診断処理を実装する
  return '';
}
```

`/*` で始まり `*/` で終わる文字列の中身は、以前出てきた `//` と同じように、コメントとして扱われ、ソースコードとしては実行されない。

今回のように、関数の上に書かれているコメントは、 **JSDoc** と呼ばれる形式のコメントの書き方で、直下の `assessment(userName)` という関数を説明している。

- 最初の行は、「関数の処理内容」を説明している。
- 2 行目の `@param` は関数の「引数（parameter）」のことで、`userName` は「引数の名前」。
- 3 行目の `@return` は関数の「戻り値（return value）」のこと。
- 2 行目と 3 行目の `{string}` は値の**型**が「文字列（string）型」であることを意味している。

型 とは、値の種類のことで、JavaScript の型には数値、文字列、真偽値などがある。

#### 入力が同じ名前なら同じ診断結果を出力する処理

名前の文字列の 1 文字が実は「ただの整数である」という事実をうまく利用して、入力が同じ名前なら同じ診断結果を出力する処理を実装しよう。

デベロッパーツールの Console で、

`'A'.charCodeAt(0);`

と入力すると、

`65`

と表示されるはず。

この仕組みをつかって

1. 名前のすべての文字のコードの整数値を足す
    1. 足した結果を、診断結果のパターンの数で割った余りを取得する
    1. 余りを診断結果の配列の添え字として、診断結果の文字列を取得する

という処理を実装する。

assessment.js を以下のように変更

```js
'use strict';
const answers = [
  '{userName}のいいところは声です。{userName}の特徴的な声は皆を惹きつけ、心に残ります。',
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
  const result = answers[index];

  // TODO {userName} をユーザーの名前に置き換える
  return result;
}
console.log(assessment('太郎'));
console.log(assessment('次郎'));
console.log(assessment('太郎'));
```

for 文を使って、名前の文字すべての文字のコードを足し合わせている。

Console に

`{userName}のいいところは決断力です。{userName}がする決断にいつも助けられる人がいます。`

`{userName}のいいところは自制心です。やばいと思ったときにしっかりと衝動を抑えられる{userName}が皆から評価されています。`

`{userName}のいいところは決断力です。{userName}がする決断にいつも助けられる人がいます。`

と表示されていればOK。