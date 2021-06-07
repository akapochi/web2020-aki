# JavaScript の関数

ここでは

1. 関数

を学ぶ。

## 変数や関数の名前の付け方

変数名で複数の単語をつなげたいとき、3種類のつなげ方がある。

| もとの単語       | キャメルケース  | スネークケース   | ケバブケース     |
| ---------------- | --------------- | ---------------- | ---------------- |
| log date         | logDate         | log_date         | log-date         |
| user information | userInformation | user_information | user-information |

JavaScript ではキャメルケースが使われる。（JavaScript では変数名にハイフンが利用できないらしい）

キャメルケースでは、スペースを使わずに単語をつなげて各単語の先頭を大文字にする。ただし、最初の単語の先頭は小文字にする。

## 関数

関数とは、ひとかたまりの処理に名前をつけて、再利用できるようにしたもの。

実は、関数は今までにも出てきていて、

```js
console.log('ログ');
```

の `log` の部分、

```js
document.write('こんにちは');
```

の `write` の部分はそれぞれ関数の名前であり、上記はどれも関数を利用する記述だった。

これらの関数は「組み込み関数」と言って、最初からブラウザで使えるようにしてある関数。

つまりすでに関数を使っていたことになる。今日やろうとしているのは関数を「作る」こと。

たとえば次のような関数を作ることができる。

```js
function logDate() {
  console.log(new Date());
}
```

Console で以下を入力してみよう。改行には Shift + Enter を用いよう。

```js
function logDate() {
  console.log(new Date());
}
logDate();
logDate();
```

関数に値を渡して、処理をして、その結果をもらうこともできる。たとえば、

```js
function square(n) {
  return n * n;
}
```

これは、square という「与えられた数を二乗する」処理を行う関数。

Console に以下のように入力して実行すると

```js
function square(n) {
  return n * n;
}
console.log(square(3));
console.log(square(12));
```

`9`

`144`

と表示される。

ちなみにさっきの関数と違い、関数の定義のところに `console.log` が書かれていないので、関数を実行するときに `console.log` を付けている。

この例でいう ( ) の中に入っている n は、引数（ひきすう）と呼ばれている。
引数は、関数に渡すことができる値。

`return n * n;`

は、 n * n の結果を、この関数の結果として返すという記述。

この関数の結果のことを、戻り値（もどりち）という。

JavaScript では、戻り値を記述するために return（リターン）文を用いる。

## 関数を使ったプログラム

`自分が生まれてからの秒数をアニメーションで表示する`

ということをやってみる。

js-function.html に以下を追加。

```html
<!DOCTYPE html>
<html lang="ja">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>JavaScript の関数</title>
</head>

<body>
  <p id="birth-time"></p>
  <script src="function.js"></script>
</body>

</html>
```

```html
<p id="birth-time"></p>
```

で、id という属性を使い、中身がからっぽの p タグに専用の名前 birth-time をつけている。
名前をつけておくことで、JavaScript のプログラムの中からこのタグの内容を調べたり、書き換えたりできます。

function.js に以下を追加。

```js
var myBirthTime = new Date(1982, 11, 17, 12, 30);
function updateParagraph() {
  var now = new Date();
  var seconds = (now.getTime() - myBirthTime.getTime()) / 1000;
  document.getElementById('birth-time').innerText = `生まれてから${seconds}秒経過。`;
}
setInterval(updateParagraph, 50);
```

- （なぜか）月のみ 0 はじまりとなっているので注意！

- .getTime() を実行することで、 その日時の 1970 年 1 月 1 日からのミリ秒（＝ 1000 分の 1 秒）を取得できる。 （画像参照）

- document.getElementById は、先ほど HTML で設定した id 属性 に、'生まれてから' + seconds + '秒経過。' という文字列を設定させるための記述

- setInterval は、指定された関数を、指定されたミリ秒ごとに繰り返し実行するという関数

  ここでは、 updateParagraph 関数を、50 ミリ秒で繰り返し実行するようになる。

## 練習問題

areaOfCircle という名前の関数を使い、

```js
document.write('<p>半径 5cm の円の面積は ' + areaOfCircle(5) + 'です。</p>');
document.write('<p>半径 10cm の円の面積は ' + areaOfCircle(10) + 'です。</p>');
document.write('<p>半径 15cm の円の面積は ' + areaOfCircle(15) + 'です。</p>');
```

と入力したら、半径 5cm, 10cm, 15cm の円の面積を計算してくれる関数を作ろう。

<!-- ## 練習問題の答え

```js
function areaOfCircle(r) {
  var area = r * r * 3.14;
  return area;
}
document.write(`<p>半径 5cm の円の面積は${areaOfCircle(5)}です。</p>`);
document.write('<p>半径 10cm の円の面積は ' + areaOfCircle(10) + 'です。</p>');
document.write('<p>半径 15cm の円の面積は ' + areaOfCircle(15) + 'です。</p>');
``` -->
