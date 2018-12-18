# 関数コンストラクタ

関数宣言、関数式の他にもうひとつ**_関数コンストラクタ_**という方法を使って関数を定義することができます。


## 関数コンストラクタの構文

**_関数コンストラクタ_**は本来オブジェクトを生成する方法です。**_関数コンストラクタ_**はオブジェクトを生成する際使われる宣言キーワード`new`を使って生成することができます。
この`new`を伴って**_コンストラクタ関数_**を呼び出すことで特別な役割を果たすことが出来ます。
**_関数コンストラクタ_**ではFunctionクラスのオブジェクトが生成できます。

```js
let 変数名 = new Function('引数1', '引数2', ..., '引数N', '処理文');
```

Functionに続く括弧「()」の中に引数を記述していきます。ただし、最後尾は引数ではなく関数の処理文を記述します。引数および処理文はダブルクオーテーションで囲みます。
以下は**_関数コンストラクタ_**を使ったサンプルコードです。

```js
function displayLog()
{
  total = func(2, 5, 8); // 関数コンストラクタに引数を格納して実行
  console.log('The total number is '+total);
}
let func = new Function(
  'x',
  'y',
  'z',
  'return x + y + z;'
);

displayLog(); // 'The total number is 15'
```

# 関数宣言と関数式と関数コントラクタの違い
以下の3種類の関数はそれぞれ同じ結果を出力します。

```js
// 関数宣言
function func1(a, b) {
  return a*b;
}
// 関数式
let func2 = function(a, b) {
  return a*b;
}
// 関数コントラクタ
let func3 = new Function('a', 'b', 'return a * b;');

console.log(func1(4,8)); // 32
console.log(func2(4,8)); // 32
console.log(func3(4,8)); // 32
```

ただし関数式と**_関数コンストラクタ_**で定義されたものとは違い、関数宣言で定義された関数は定義式より前で呼び出すことができます。
```js
// 関数宣言
message(); // 'Hello!'
function message() {
   console.log('Hello');
}
```
```js
// 関数式
message(); // TypeErrorとなり出力されない
var message = function(){
   console.log('Hello');
}
```
```js
// 関数コントラクタ
message(); // TypeErrorとなり出力されない
var message = new Function(console.log('Hello'));
```