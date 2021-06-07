# 第7回

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

### 練習問題の答え

```js
userNameInput.onkeydown = (event) => {
  if (event.key === 'Enter') {
    assessmentButton.onclick();
  }
};
```
