# 第3回

## JavaScript のループ

ここでは

1. for文

を学ぶ。

### for文

```js
for (初期化式; 条件式; 変化式) {
  // 繰り返しを行いたい処理
}
```

### for文を使ったプログラム

`1 から 10 万までの数を表示させる`

という問題を解く。

js-loop.html に以下を追加

```html
<!DOCTYPE html>
<html lang="ja">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>JavaScript のループ</title>
</head>

<body>
  <script src="loop.js"></script>
</body>

</html>
```

loop.js に以下を追加

```js
for (var i = 1; i <= 100000; i++) {
  document.write(i);
}
```

見づらいので、数字と数字の間に空白（半角スペース）を入れたい。  
→ loop.js を以下のように変更

```js
for (var i = 1; i <= 100000; i++) {
  document.write(i + ' ');
}
```

### 練習問題

for 文を使って、1から10万までの数で Fizz Buzz （フィズ・バズ）を実装してみよう。

>Fizz Buzz とは、以下のようなルールのもとで1から数字を数えていく遊び。
>
>- '3' で割り切れる場合は「Fizz」をその数の代わりに発言する。  
>- '5' で割り切れる場合は「Buzz」をその数の代わりに発言する。  
>- '3' と '5' の両方で割り切れる場合（＝すなわち '15' の倍数）は「FizzBuzz」をその数の代わりに発言する。  
>- 上記のいずれにも当てはまらない数の場合、その数をそのまま発言する。
>
>つまり、普通に数字を数えていくと
>
>`1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 ...`
>
>となるが、FizzBuzz のルールのもとで数字を数えていくと
>
>`1 2 Fizz 4 Buzz Fizz 7 8 Fizz Buzz 11 Fizz 13 14 FizzBuzz 16 17 Fizz 19 Buzz ...`
>
>となる。

また、前回学んだif文を使って '3' で割り切れる数の処理を書くことができる。

```js
if (i % 3 === 0) {
  // 3 で割り切れる場合の処理
}
```

ここで、 % は剰余演算子というもので、前の数を後ろの数で割った余りが計算できる。  
たとえば、 `10 % 5` の結果は 0 、 `10 % 3` の結果は 1 になる。

### 練習問題の答え

```js
for (var i = 1; i <= 100000; i++) {
  if (i % 15 === 0) {
    document.write('FizzBuzz ');
  } else if (i % 5 === 0) {
    document.write('Buzz ');
  } else if (i % 3 === 0) {
    document.write('Fizz ');
  } else {
    document.write(i + ' ');
  }
}
```

---

## JavaScript の配列

ここでは

1. 配列

を学ぶ。

### 配列

配列は、 JavaScript では次のように表現する。

```js
['A組', 'B組', 'C組', 'D組'];
```

この配列は、変数に代入することもできる。

```js
var classes = ['A組', 'B組', 'C組', 'D組'];
```

配列には数値を入れることもできる。

```js
var numbers = [342, 493, 532, 693];
```

Console で以下のコードを打ってみる。

```js
var classes = ['A組', 'B組', 'C組', 'D組'];
classes[0];
```

すると

`"A 組"`

と表示される。

次に、

```js
var classes = ['A組', 'B組', 'C組', 'D組'];
classes[4];
```

と打つと、

`undefined`

と表示される。

また配列は、

`console.log(配列)`

という記述を利用して Console で中身を確認できる。

以下のように Console に入力してみる。

```js
var classes = ['A組', 'B組', 'C組', 'D組'];
console.log(classes);
```

すると、

`["A 組", "B 組", "C 組", "D 組"]`

と、表示されます。

配列には、要素を足すこともできる。

```js
var a = [];
console.log(a); // [] と表示
console.log(a.length); // 0 と表示
a.push('X');
console.log(a); // ["X"] と表示
console.log(a.length); // 1 と表示
a.push('Y');
console.log(a); // ["X", "Y"] と表示
console.log(a.length); // 2 と表示
```

このように、配列の変数名に続けて

`.push('追加したい要素')`

と記述することで、要素を追加できる。

また、配列の変数名の後ろに

`.length`

と書くことで、配列に入っている要素の個数を整数値で得ることができる。
配列に入っている要素の個数のことを「配列の長さ」という。

### 配列を使ったプログラム

`各学年 A 組、B 組、C 組、D 組まである高校の、1~3 年の全クラスのリストを作る`

ということをやってみる。

js-collection.html に以下を追加

```html
<!DOCTYPE html>
<html lang="ja">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>JavaScript のコレクション</title>
</head>

<body>
  <script src="collection.js"></script>
</body>

</html>
```

collection.js に以下を追加

```js
var classes = ['A組', 'B組', 'C組', 'D組'];
for (var grade = 1; grade < 4; grade++) {
  for (var i = 0; i < classes.length; i++) {
    document.write(`<p>${grade}年${classes[i]}</p>`)
  }
}
```

### 練習問題

ペットの名前を考えるのに、

`ああ, あい, ...,あこ, いあ, ..., ここ`

という、あ行とか行の文字を使った 2 文字で良い名前を考える。

すべての名前を HTML に書き出すプログラムを作ってみよう。

### 練習問題の答え

```js
var chars = ['あ', 'い', 'う', 'え', 'お', 'か', 'き', 'く', 'け', 'こ'];
for (var i = 0; i < chars.length; i++) {
  for (var j = 0; j < chars.length; j++) {
    document.write('<p>' + chars[i] + chars[j] + '</p>');
  }
}
```

---

## JavaScript の関数

ここでは

1. 関数

を学ぶ。

### 変数や関数の名前の付け方

変数名で複数の単語をつなげたいとき、3種類のつなげ方がある。

| もとの単語       | キャメルケース  | スネークケース   | ケバブケース     |
| ---------------- | --------------- | ---------------- | ---------------- |
| log date         | logDate         | log_date         | log-date         |
| user information | userInformation | user_information | user-information |

JavaScript ではキャメルケースが使われる。（JavaScript では変数名にハイフンが利用できないらしい）

キャメルケースでは、スペースを使わずに単語をつなげて各単語の先頭を大文字にする。ただし、最初の単語の先頭は小文字にする。

### 関数

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

### 関数を使ったプログラム

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

### 練習問題

areaOfCircle という名前の関数を使い、

```js
document.write('<p>半径 5cm の円の面積は ' + areaOfCircle(5) + 'です。</p>');
document.write('<p>半径 10cm の円の面積は ' + areaOfCircle(10) + 'です。</p>');
document.write('<p>半径 15cm の円の面積は ' + areaOfCircle(15) + 'です。</p>');
```

と入力したら、半径 5cm, 10cm, 15cm の円の面積を計算してくれる関数を作ろう。

### 練習問題の答え

```js
function areaOfCircle(r) {
  var area = r * r * 3.14;
  return area;
}
document.write(`<p>半径 5cm の円の面積は${areaOfCircle(5)}です。</p>`);
document.write('<p>半径 10cm の円の面積は ' + areaOfCircle(10) + 'です。</p>');
document.write('<p>半径 15cm の円の面積は ' + areaOfCircle(15) + 'です。</p>');
```
