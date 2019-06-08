## コールバック関数

ある関数の引数として渡され、その関数の中で実行される関数のことを**_コールバック関数_**と言います。
以下の例では関数呼び出し時に無名関数を定義して引数として渡しています。関数funcの処理文の中で仮引数`callback`が**_コールバック関数_**として実行されています。

```js
function func(callback) {
  callback();  // コールバック関数の実行
  console.log('Second!');
}

// 関数呼び出し時に引数に関数を渡す
func(function() {console.log('First!');});
```

出力結果:

```js
'First!'
'Second!'
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/vnLpwrfh/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

このように**_コールバック関数_**として定義される関数は、関数呼び出し時の実引数としてそのまま定義されることがよくあります。特に簡潔な記述が特徴である**_アロー関数_**はこのような使われ方に向いているのでよく利用されます。

## コールバック関数を使ったタイマー処理

例えばブラウザ上でスライドーショーを実装させたい場合、一定間隔で処理を行うための仕組みが必要になります。JavaScriptにはタイマー処理を行うための関数`setTimeout`と`setInterval`が用意されています。これらの関数に**_コールバック関数_**を引数として渡すことで、タイマー制御された処理を簡単に実装することができます。

### setTimeout

`setTimeout(コールバック関数、時間(ミリ秒))`の構文を持つ`setTimeout`は、引数として指定した時間が経過した時に、ある特定の処理を1回だけ行う場合に使います。この特定の処理は**_コールバック関数_**として引数で渡すことができます。

### setInterval

`setInterval(コールバック関数、時間(ミリ秒))`の構文を持つは`setInterval`は、一定時間ごとにある特定の処理を繰り返す場合に使います。`setTimeout`同様、この特定の処理は**_コールバック関数_**として引数で渡すことができます。

例えば以下のコードを実行すると1秒ごとにログが表示されます。

```js
setInterval(function() { console.log('Hello'); }, 1000);
```

## クロージャー

関数の中に関数が存在する場合を考えます。この時内部の関数からは、外側の関数で定義された変数に対し直接参照先にアクセスできます。**_クロージャー(closure)_**はこのように内外2つの関数を使い、内側の関数が外側の関数のローカル変数を参照する仕組みを使った特殊なオブジェクトです。
説明だけではイメージが湧きにくいので実際に**_クロージャー_**の例を見てみましょう。

```js
function externalFunction() {
  let planet = 'The Sun'; // アクセスされるローカル変数
  return function innerFunction() {
    console.log(planet); // 内部関数から外部のローカル変数にアクセス
  };
};
let closure = externalFunction(); // グローバル変数

closure(); // 'The Sun'
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/vx28nmoa/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

上記の例では関数`innerFunction`が、自身を内包している`externalFunction`のローカル変数`planet`を参照しています。もちろんこのローカル変数はプライベートな関数スコープを持つので外部からアクセスすることはできません。

そして`externalFunction`がグローバル変数`closure`に格納されることで、これが**_クロージャー_**としてはたらきます。
**_クロージャー_**となっているこの変数`closure`を実行すると、戻り値として関数`innerFunction`が返りますが、この時参照された`planet`はその値が破棄されることなく保持され続けます。通常の関数内で定義されたローカル変数が、関数が呼ばれるたびに生成されるのとは対照的であり、これが**_クロージャー_**の最も重要な特徴です。


### クロージャーの利用例

**_クロージャー(closure)_**の特徴は、すでに述べたようにクロージャー外からアクセスできない変数に、内部の関数が参照できそれを保持することができるということです。よくあげられるクロージャーの実用例として、**「値を呼び出すたびに数をカウントしてくれる関数」**があります。
まず次のようなクロージャーでない関数を呼び出した場合、呼び出すごとにローカル変数の値が新たに生成されるためカウントされることはありません。

```js
// クロージャーでない場合
function func() {
  let counter = 1;
  console.log(counter);
  counter++;
}

// カウントされない
func(); // 1
func(); // 1
func(); // 1
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/zLa143uq/2/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

一方で**_クロージャー_**は内部関数で参照した変数の値が保持されるため呼び出すたびに数がカウントされます。

```js
// クロージャーの場合
function func() {
  let counter = 1;
  return function() {
    console.log(counter);
    counter++
  };
}
let closure = func();

// カウントされる
closure(); // 1
closure(); // 2
closure(); // 3
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/xqgty0k4/2/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

また次のように複数のグローバルオブジェクトに格納した場合、それぞれが独立したコンテキストをもつようになります。

```js
function func() {
  let counter = 1;
  return function() {
    console.log(counter);
    counter++
  };
}
let closure1 = func();

closure1(); // 1
closure1(); // 2
closure1(); // 3

let closure2 = func();

// closure1とは独立してカウントされる
closure2(); // 1
closure2(); // 2
closure2(); // 3
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/oshp598g/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

ただし、実は「数をカウントしてくれる関数」自体は次のようにグローバル変数を使うことでも実装できます。

```js
var counter = 1; // グローバル変数

function func() {
  console.log(counter)
  return counter++;
}
func(); // 1
func(); // 2
func(); // 3
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/uye41x8f/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

グローバル変数を使う場合と比較して**_クロージャー_**を使う利点をあげるとすれば、その名の通り外部から変数にアクセスできない仕組みになっていることが挙げられます。このような仕組みは特にオブジェクト指向型のプログラミングでは**カプセル化**と言われ、コードの安全性を高める上で重要なテクニックとなっています。どこからでもアクセスできるグローバル変数をむやみに増やしすぎるのは、意図しない変数の改変の危険性を考えるとあまり良いことではありません。
**_クロージャー_**のテクニックうまく利用することでより安全で保守性の高いプログラムにすることができるのです。

## 再帰関数

**_再帰関数_**はその関数の命令文の中で自分自身を呼び出している関数のことです。
再帰関数の具体例としてよく使われるのが**_階乗(factorial)_**の計算です。
階乗の計算は例えば`6!`であれば、

```js
6 * 5 * 4 * 3 * 2 * 1 = 720
```

のように指定した数から1を引いた数を、その数が1になるまで繰り返し掛け合わせていきます。
これを再帰関数で表現すると次のようになります。

```js
let factorial = (n) => {
  // nの値によって条件分岐
  if (n === 1) {
    return 1; // 1なら1を戻り値として終了
  } else {
    return n * factorial(n - 1); // (n - 1)の引数で自身の関数を呼ぶ
  }
}

console.log(factorial(6)); // 720
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/wuLbjcrq/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

上の例では**_アロー関数_**を使っていますがその他の定義方法でももちろん問題ありません。
この関数は戻り値の計算`n*factorial(n-1)`で自分自身が呼ばれ、これは`n`の値が1になるまで繰り返し行われます。
再帰関数は、**while文**と同様に**無限ループに注意**しなければなりません。意図せず無限回の処理を実行してPCに負荷を与えてしまわないよう、呼び出しを終了する条件を記述するなど注意する必要があります。

## 更に学ぼう

- [JavaScript入門 - ドットインストール](https://dotinstall.com/lessons/basic_javascript_v2)

### 本で学ぶ

- [Eloquent JavaScript 3rd Edition](http://eloquentjavascript.net/)
