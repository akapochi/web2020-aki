# JavaScript のループ

ここでは

1. for文

を学ぶ。

## for文

```js
for (初期化式; 条件式; 変化式) {
  // 繰り返しを行いたい処理
}
```

## for文を使ったプログラム

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

## 練習問題

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

<!-- ## 練習問題の答え

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
``` -->
