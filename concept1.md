## 関数とは

**_関数_**は再利用が可能なコードの塊です。英語名を使って**ファンクション**とも呼ばれることもあります。**_関数_**はプログラミングの主役と言っても良いほどに頻繁に使われています。以下が関数の基本的なシンタックスです。

```js
function myFunc() {
  //関数の処理文
}

// 関数の呼び出し
myFunc();
```

関数はブロック「{}」で囲まれた構造を持ちます。このブロックの中に処理を書いておき、`myFunc();`の一行でこの関数を呼び出すだけで関数のコードを実行することができます。同じコードを何度も記述するような手間が省け、また関数をうまく使うことでプログラムの可読性、メンテナンス性を高めることができます。


上記のような構文を使って関数を定義することを**_関数宣言(function文)_**と言います。
`myFunc`と書かれた部分がこの関数の名前になります。冒頭の`function`はこれが関数であることを明示するための**予約語**なので必ず記述しなければなりません。
関数名は任意ですがその機能に合わせた適切な名前にすると良いでしょう。また以下のようなルールがあるので覚えておきましょう。

- JavaScriptの予約語は使うことができません。(例. `if`、`for`、`var`、`let`、`const`など)
- 関数を定義したスコープ上に同じ関数名や変数名がある場合、重複して使うことはできません。
- 大文字と小文字は区別されます。(**ケースセンシティブ**といいます)
- 名前の1文字目で使えるのは半角の英字、ドル記号、アンダースコアのみ。2文字目以降はこれに加えて半角数字を使用可能です。
- Unicodeが認められているので、日本語文字も関数名として使うことができます。(非推奨)

## 関数宣言の構文

プログラミング言語では関数を使うことを**関数を呼び出す**と表現します。関数を呼び出すには、利用したい箇所で次のように`関数名();`と記述します。

```js
myFunc();
```

次の簡単な例ではメッセージをログとして出力する処理を記述した関数を呼び出しています。

```js
function displayMessage() {
  console.log('Message');
}

// 関数の呼び出し
displayMessage(); // 'Message'
```

この例のように処理が一行しかない場合は、わざわざ関数にして呼び出す意味がないように思えます。しかし処理内容が複雑になるにつれ関数は威力を発揮するようになります。

## 戻り値を持つ関数

関数は呼び出すだけではなく、何らかのデータを結果として返すこともできます。
この返されたデータは**_戻り値_**といいます。
次の例では、関数の中で宣言した変数に3を掛けた計算結果を戻り値として取得しています。

```js
function multiplyNum() {
  let number = 5;
  return number * 3; // 関数の結果を返す記述
}

// 関数の呼び出し
console.log(multiplyNum()); // 15
```

この例のように関数を使った出力結果が欲しい場合は、その戻り値を`return 戻り値;`のように記述しておきます。この`return`を使った文は関数処理の最後に記述しておく必要があります。上記の例では関数の呼び出しを`console.log()`の中で実行すると、出力結果である戻り値がそのまま`console.log`へ格納されるようになります。`return`を使った関数の一回の呼び出しで返すことができるのは一つの値のみです。複数の値を一度に返すことはできません。

また関数はブロックの中に**スコープ(関数スコープ)**を持ちます。従って関数の中で宣言された値に外部から直接アクセスすることはできません。

```js
function multiplyNum() {
  let number = 5; // 関数外部から直接アクセスはできない
  return number * 3;
}
console.log(number); // 出力結果は「undefinded」になる。
```

それでは例えば、関数の中で宣言した変数の値を別の値に変更したい場合はどうすれば良いのでしょうか。これは次で述べる**_引数_**を使うことで実現できます。

## 関数の引数

関数は、**_引数_**を利用することで呼び出し時に値を渡すことができます。N個の**_引数_**を持つ関数は次のように定義できます。

```js
function funcName(引数1, 引数2 , ..., 引数N) {
  // 関数の処理文
}
```

関数を呼び出す際は次のようにして**_引数_**を渡すことができます。

```js
funcName(引数1, 引数2 ,...,引数N); // 関数呼び出し
```

関数呼び出し時の引数は**_実引数_**と呼ばれ、関数定義で値が渡される側の引数のことを**_仮引数_**と呼びます。**_実引数_**は記述された順番で仮引数に格納されていきます。**_仮引数_**は**関数スコープ**を持ち、関数ブロック内で変数として利用することができます。

具体例で確認しましょう。次の例では、関数の呼び出し時に2つの数値を渡して、関数は渡された2つの値の和を求めてその結果を返すという処理を行なっています。

```js
function addNumber(num1, num2) {
  let sum = num1 + num2; // ２つの値の和を格納する変数
  return sum;
}

console.log(addNumber(4,5)); // 9
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/4xad35yg/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

この例ではまず、関数呼び出す際に関数に渡したい値を`addNumber(4,5)`のように記述します。この時実引数4は仮引数`num1`へ格納され、実引数5は仮引数`num2`へと格納されます。

## 初期値を持つ仮引数

ES6では仮引数に初期値を与えることができます。

```js
function addNumber(num1, num2 = 1) {
  let sum = num1 + num2; // num2は1
  return sum;
}

console.log(addNumber(4)); // 5
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/go0fqm9y/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

上記の場合、実引数の4は仮引数で定義された順番で格納されていくので、`num1`に格納されます。この時、本来であれば仮引数に対して実引数が一つ足りないのでエラーが起きそうな気がしますがそうはなりません。もし引数が与えられなかったとしても`num2 = 1`とあるように、デフォルトで初期値1が与えられた状態で処理が実行できます。もちろんこれは初期値なので、実引数を与えれば上書きされます。

```js
function addNumber(num1, num2 = 1) {
  let sum = num1 + num2;
  return sum;
}

console.log(addNumber(4, 6)); // 10
```

## arguments

JavaScriptでは、関数に任意の個数渡された引数は内部で自動的に`arguments`という名前の**配列**として格納されるので、関数内でこれを取り扱うことができます。

```js
let func = function() {
  for(let i = 0; i < arguments.length; ++i) {
    console.log(arguments[i]);
  }
};

func('a','b','c','d','e');
```

出力結果：

```
'a'
'b'
'c'
'd'
'e'
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/dtn2zfej/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

## 残余引数(Rest Parameters)

関数定義に記述する最後尾の引数に 「...」の接頭辞を付与すると、この引数は呼び出し側の**残りの実引数全てを 要素にもつ配列**になります。この時配列となった引数のことを**_残余引数(Rest Parameters)_**と呼びます。仮引数がひとつしか定義されていない場合はそれ自身が**_残余引数_**になります。

```js
function sample(...args) {
  console.log(args.length);
}

sample(); // 引数を与えない
sample(100); // 引数を1つ
sample(100, 200, 300); // 引数を3つ
```

出力結果:

```
0
1
3
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/39604ds1/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

この時の`args`は配列なので、通常の配列と同じように要素数を返す**lengthプロパティ**を使うことができているのがわかります。

次の例のように引数が複数ある場合は、すでに説明した通り残りの実引数が各要素となって配列に格納されます。

```js
function sample(a, b, c, d, ...args) {
  // 繰り返し処理で配列の中の要素を取り出す
  for (let i = 0; i < args.length; i++) {
    console.log(args[i]);
  }
}

sample(1,2,3,4,5,6,7,8,9); // 5〜9までが配列になる
```

出力結果:

```js
5
6
7
8
9
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/1mx9ybu7/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

## 関数式

ここまでは関数宣言として関数を単体で定義する記述について見てきましたが、ここでは別の関数の定義の仕方を説明します。

## 関数式の構文

次のように宣言された変数に格納するような形で関数を定義する式を**_関数式(function演算子)_**と呼びます。

```js
let func = function function_name(引数1,引数2 ,...,引数N) {
// 関数の処理文
}
```

ここでは関数名をあえて指定していますがこれは省略することができます。実際には次に説明する関数名を省略した無名関数を使うことが一般的です。

## 無名関数の構文

```js
let func = function(引数1,引数2 ,...,引数N) {
// 関数の処理文
}
```

関数は関数名をつけずに定義することができます。この関数のことを**_無名関数_**あるいは**_匿名関数_**と言います。
関数式で定義した関数を呼び出す際は関数式で宣言された変数を使います。

```js
let func = function() { return 'Message!'; }
console.log(func()); // 'Message!'
```

**_無名関数_**も関数宣言された関数と同じように引数を使って入出力が実装できます。

```js
function displayLog() {
  total = func(2, 5, 8); // 無名関数に引数を格納して実行
  console.log(`The total number is ${total}`);
}

let func = function(x, y, z) {
  return x + y + z;
}

displayLog(); // 'The total number is 15'
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/ha4r3euL/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>