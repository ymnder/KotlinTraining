# Kotlin Training
<!-- スネークまずはプログラミングの基本を思い出して -->
## 勉強を進めるにあたり
まず試しにKotlinを書いてみたい場合は[Kotlin公式サイト](https://try.kotlinlang.org/)で試すことができます。
IDEはIntelliJやAndroid Studioを使えます。
Javaを書かれていた経験のある方は、一度Javaでコードを書いてからIDEの機能でconvert to KotlinをすることでKotlinでの書き方を学べます。

## Basic Syntax
### 変数の定義
まずはKotlinの変数宣言について見てみましょう。
```
val hoge: String = "momiji"
不変 変数名: 型 = 値
```
Kotlinは静的型付け言語です。
しかし型を最初に書く必要はありません。
行頭には不変を示す`val`が書かれ、型はコロンのあとに宣言されています。
型推論を行えるので、型を省略することもできます。
```
val hoge = "momiji"
不変 変数名 = 値
```
primitiveはJavaと異なります。
Int、Long、Float、Double、Short、Byte
文字列はChar、Stringが使えます。

valとvarにより変数を定義することができます
valはimmutableな変数です。Javaのfinalに相当します。
varはmutableな変数です。
```
val hoge: String = "momiji"
hoge = "kaede" //this is error because hoge is immutable
var fuga: String = "keyaki"
fuga = "akebi"
```
`hoge`は再代入できないimmutableな変数として定義できます。

//TODO: アクセサについて

### クラスの定義
```
class Hoge(val hoge: String)

public final class Hoge {
	public final String hoge
}
```
クラスはDefaultでfinalです。
```
class Hoge(val hoge: String)
class Hoge(hoge: String)
```
このクラスは意味が異なります。
前者はフィールド変数としてコンストラクタ内で変数を宣言しています。
//TODO: 表現を要確認
後者は引数として宣言されているため、コンストラクタの外から変数を呼び出せません。
なお、Kotlinはクラス名とファイル名が一致しなくて大丈夫です。

classのコンストラクタは書き方が３通りあります。
```
class Hoge()
class Hoge {
	constructor()
}
class Hoge {
	init()
}

```
DIを使う場合は次のように宣言します。
```
class Hoge @Inject constructor() {

}
```

継承も簡単です。
```
class Hoge() : Fuga()

```

dataを保持したいクラスは簡単に作れます。
```
data class Hoge(val hoge: String)
```
変数はvalで宣言した場合は、getterが自動的に追加されるためボイラープレートを書く必要がありません。
varの場合はsetterも生えます。

ちなみにdataクラスにもメソッドを持たせられます。
```
data class Hoge(val hoge: String) {
	fun hoge() {
		//something cool
	}
}
```

<!--
//ボイラープレートとは、お決まりで書かなければならないコードを指す。
//生えるとは、クラスにメソッドが定義されていることを指す。
//TODO: アクセサについて
-->

### Null Safety
Kotlinはnull安全な言語です。
変数がnullか否かはJavaでは`@NonNull`や`@Nullable`のアノテーションをつけて判別してきました。
Kotlinでは型に`?`を付与してnullを表現できます。
```
var hoge: String = "momiji"
var fuga: String? = null
```
`?`を付けることでnullを代入することができます。

### 初期化
変数は宣言時に初期化が求められます。
```
var hoge: String = "" //this is OK
var fuga: String // this is error
```
classのコンストラクタのどこかで初期化を行う必要があります。
しかし、DIなど初期化が後ほど行いたい場合も存在します。
```
lateinit var hoge: String
```
lateinitにより初期化を後回しにできます。
初期化をする前に変数にアクセスした場合は例外が発生します。
lateinitにはvalは存在しません。

### static
Kotlinにはstaticの予約語はありません。
変数を宣言したい場合は、companion objectもしくはtop-levelに置く方法があります。
```
companion object {
	const val HOGE = "ume"
}
const val HOGE = ""
```
constはprimitiveとStringにしかつけられませんが、定数として定義できます。

### スコープ関数
* let
* also
* apply
* run
* with

[Kotlinアンチパターン
](https://www.slideshare.net/RecruitLifestyle/kotlin-87339759)

### ifとwhenと三項演算子
Kotlinはnull安全ですので、`if(hoge != null)`が必要となる場面が減ります。
nullableな変数をunwrapするときはifでも良いですが次の書き方は如何でしょうか。
```
var hoge: String? = "hoge"
hoge?.let{
	println(it)
}
```
`?.let`はnullでない場合に実行されるの意です。
let自体がnullをunwrapするわけではなく、あくまでも`?`でnullでない場合に実行しています。
`let`により中身を取り出すことができます。
`it`は暗黙的に宣言される変数で、`let{name -> println(name)}` のように明示的に名前を付けられます。
`it`はスコープが絞られていますので、他のスレッドから値を書き換えられる心配はありません。
```
val article : Article? = null
var article2 : Article? = null
fun hoge() {
    //OK
    if (article != null) {
        article.kiji
    }
    //NG
    if (article2 != null) {
        article2.kiji
    }
}

fun fuga() {
		var str: String? = null
		if (str != null) {
				println(str.length) //OK
		}
}
```
なぜローカル変数ではOKになるかというとSmart Castという仕組みが関係しています。

### ラムダ式とSAM
### cast
### object
### companion
### interface
### custom getter / setter
### backing field
### ジェネリクス
## Kotlin Coroutine
## DSL
## Android KTX


## Referrence

* [Kotlin Getting Started](https://kotlinlang.org/docs/reference/basic-syntax.html)
* [Kotlin 入門までの助走読本](https://drive.google.com/file/d/0Bylpznm149-gTGRjOFRkWm9PODg/view)
* [Taking Advantage of Kotlin](https://codelabs.developers.google.com/codelabs/taking-advantage-of-kotlin/#0)
* [Kotlin Koans](https://kotlinlang.org/docs/tutorials/koans.html)
* [Introduction to Kotlin (Google I/O '17)](https://www.youtube.com/watch?v=X1RVYt2QKQE&vl=ja)
* [はじめてのKotlinハンズオン](https://speakerdeck.com/ntaro/hazimetefalsekotlinhanzuon-number-droidkaigi)
* [Kotlinスタートブック](https://www.amazon.co.jp/Kotlin%E3%82%B9%E3%82%BF%E3%83%BC%E3%83%88%E3%83%96%E3%83%83%E3%82%AF-%E6%96%B0%E3%81%97%E3%81%84Android%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0-%E9%95%B7%E6%BE%A4-%E5%A4%AA%E9%83%8E/dp/4865940391)
* [Kotlinイン・アクション](https://www.amazon.co.jp/Kotlin%E3%82%A4%E3%83%B3%E3%83%BB%E3%82%A2%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3-Dmitry-Jemerov/dp/4839961743)
* [Androidアプリ開発のためのKotlin実践プログラミング](https://www.amazon.co.jp/exec/obidos/ASIN/479805366X/numa08blog01-22/)
