## アロー関数

ES6から新しい関数の定義方法として、**_アロー関数_**が追加されました。**_アロー関数_**を定義するときは次のように**矢印記号「=>」+**を使って関数を定義するのが特徴です。
**_アロー関数_**の基本的な書き方は以下のようになります。

```js
// アロー関数の宣言の基本構文
(引数1, 引数2, ...) => { /*関数の処理*/ }

// アロー関数を変数に代入した形
let arrowFunction = (引数1, 引数2, ...) => {/*関数の処理*/}
```

**_アロー関数_**は無名関数に近い書き方になっています。実際、**_アロー関数_**は無名関数の記法を簡略化するために考案されたものです。
引数がある場合は()の中に`(引数1, 引数2, ...) `のようにカンマを使って並べていきます。これは他の関数の定義のやり方と同様です。
引数を持たない場合はその記述を省略することができます。

もし関数の引数が一つだけであれば引数の()を省略できます。

```js
let arrowFunc = x => { console.log(x); }
```

ただし引数がない場合は()そのものを省略することはできません。

```js
// 引数を持たないアロー関数
let arrowFunc = () => console.log('this is an arrow function');
```

もし**_アロー関数_**の処理文が戻り値`return`を使った文一行のみであれば、この`return`を省略して記述することができます。この省略記法は見た目も簡潔で頻繁に使われますので覚えておきましょう。

```js
let arrowFunc1 = (a, b) => { return a * b; };

// 上記のアロー関数は次と同じ
let arrowFunc2 = (a, b) => a * b;
```

ただし戻り値がオブジェクトの場合は、関数ブロックの「{}」でないことを明示するために「()」で囲む決まりになっています。

```js
// オブジェクトを返すアロー関数
let arrowFunc2 = (x, y) => ({
  name: x,
  color: y
});
```

## アロー関数の特徴

**_アロー関数_**は旧来の関数の定義の記述を簡潔に書くために導入されたものなのですが、必ずしも他の方法で定義された関数と同じ挙動をする訳ではないので注意が必要です。以下で**_アロー関数_**の特徴について触れていきます。

### アロー関数とコンストラクタ

**_アロー関数_**は**コンストラクタ**として振舞うことができません。**コンストラクタ**というのは関数を使って定義された一種のオブジェクトです。**コンストラクタ**は予約語`new`を使って、雛形として定義されたオブジェクトをその都度生成することができます。

```js
// コンストラクタの定義
let Fruit = function (name, color) {
  // 引数をプロパティに格納
  this.name = name;
  this.color = color;
};

// コンストラクタを生成
let myFruit = new Fruit('apple','red');

console.log(myFruit.color); // red
```

**_アロー関数_**で同様のことをしようとするとエラーになります。
```js
let Fruit = (name, color) => {
  this.name = name;
  this.color = color;
};
let myFruit = new Fruit('apple', 'red'); // エラーになる
```

### アロー関数は配列argumentsを持たない

**_アロー関数_**は引数を格納した配列argumentsを持つことができないので使用するとエラーとなります。

```js
// 無名関数
let func = function(){
  for(let i = 0; i < arguments.length; ++i){
    console.log(arguments[i]); // 配列要素が出力される
  }
};

func('a','b','c','d','e');

// アロー関数
let func = () => {
  for(let i = 0; i < arguments.length; ++i){
    console.log(arguments[i]); // エラー
  }
};

func('a','b','c','d','e');
```

### アロー関数とthis

**_アロー関数_**を使う際に注意しないといけないのが、通常の関数と**_アロー関数_**では`this`の扱いが異なる点です。
レッスン8でもすでに触れたように、通常の関数ではthisの意味は使われるスコープや状況によって変化しますが、**_アロー関数_**を使った場合のthisは、**_アロー関数_**の定義時に定められた対象のまま変わることがありません。このような**_アロー関数_**の性質を**「thisをレキシカルに束縛する」**とも表現したりします。下記の例では、オブジェクトのプロパティとして設定したメソッドとして呼び出した場合は、そのthisは呼び出し元のオブジェクトを指しています。しかし関数呼び出しした際のthisはグローバルオブジェクトを指します。

```js
// 関数定義のthis - 無名関数定義の場合

window.name = 'Everyone'; // グローバル変数としてnameを定義

let obj1 = {
  name: 'Michael',
  sayHello: function(){
    // ここでのthisはobj1を指します。
    console.log(`Hello, ${this.name}!`);
    let sayHello = function(){
      // ここでのthisはwindowを指します。
      console.log(`Hello, ${this.name}!`);
    }
    sayHello(); // 2.関数呼び出し： 'Hello, Everyone!'
  }
}

obj1.sayHello(); // 1.メソッド呼び出し : 'Hello, Michael!'
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/j1uxaq2n/2/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

無名関数を使う代わりに**_アロー関数_**で置き換えて定義すると、関数呼び出しした際のthisであっても定義元のオブジェクトを指します。

```js
// アロー関数定義のthis
let name = 'Everyone';
let obj1 = {
  name: 'Michael',
  sayHello: function() {
    console.log(`Hello, ${this.name}!`);
    // アロー関数で定義
    let sayHello = () => {
      console.log(`Hello, ${this.name}!`);
    }
    sayHello(); // 関数呼び出し： 'Hello, Michael!'
  }
}
obj1.sayHello(); //メソッド呼び出し : 'Hello, Michael!'
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/wqybL9r3/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

ES5以前は、thisの指す内容が状況によって変わるのを回避して束縛しておくために**bindメソッド**や**that(self)を使ったthisの退避**などを駆使していましたが**_アロー関数_**によりその必要がなくなりました。

- [参考：アロー関数 - MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/arrow_functions)
