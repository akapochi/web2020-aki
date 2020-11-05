# 第1回

## HTML

- `html:5`
  
  と打ってTABキーを押すと、HTMLのひな形が作られる
- h タグ は見出しを作る（h1からh6まで）
- br タグ は改行を作る
- p タグ は段落を作る
- a タグ はリンクを作る

  ```html
  <a href="リンク先のURL">表示させたい文字</a>
  ```
  
- ul タグ はリストを作る

  ```html
  ul>li*6
  ```

- table タグ は表を作る
  
  ```html
  table>(tr>th*2)+(tr>td*2)*6
  ```

- 全角スペースと半角スペースの違いに注意

## JavaScript

- HTMLファイルの中に直接書くこともできるし、別ファイルとしてJavaScriptファイルを作ってそこに書くこともできる
- HTMLファイルのbodyタグの中に

  ```html
  <body>
  　<script src="myFirst.js（←ここは作ったJavaScriptファイルの名前）"></script>
  </body>
  ```

  のように書くとJavaScriptファイルを読み込める

- `document.write(表示したいもの)`
  
  で文字を表示させることができる
- JavaScriptでは文末のセミコロン（;）を忘れてはいけない
- エラーを見る方法は、Google Chromeで「右クリック → 検証 → console」
- `var 変数名`
  
  で変数を設定できる
- 変数名は何を表すか分かりやすいように英単語などを使う
- `**`
  
  で累乗を表す
- `//`
  
  の後ろに文字を書くとコメントになる

## HTML, JavaScript共通

- TABキーによる補完を上手く使おう！（スペルミス防止）
