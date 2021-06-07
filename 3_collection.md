# JavaScript の配列

ここでは

1. 配列

を学ぶ。

## 配列

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

## 配列を使ったプログラム

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

## 練習問題

ペットの名前を考えるのに、

`ああ, あい, ...,あこ, いあ, ..., ここ`

という、あ行とか行の文字を使った 2 文字で良い名前を考える。

すべての名前を HTML に書き出すプログラムを作ってみよう。

<!-- ## 練習問題の答え

```js
var chars = ['あ', 'い', 'う', 'え', 'お', 'か', 'き', 'く', 'け', 'こ'];
for (var i = 0; i < chars.length; i++) {
  for (var j = 0; j < chars.length; j++) {
    document.write('<p>' + chars[i] + chars[j] + '</p>');
  }
}
``` -->
