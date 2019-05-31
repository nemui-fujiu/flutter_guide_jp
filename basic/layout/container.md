+++
title = "Container"
date = 2019-02-21T00:00:00+09:00
draft = false
weight = 312
description = "Containerの基本について、どんなものがありどのように利用するのかを解説していきます。合わせてColorやPadding、Marginなども解説します。"
+++

## Containerの基本

「Container」クラスは内包する子ウィジェットをカスタマイズするために利用するウィジェットです。
どんなことができるのかを一つずつみていきましょう。

{{< highlight dart >}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Material Design',
      home: Scaffold(
        body: Center(
          child: Container(
            color: Colors.blue,
            width: 300.0,
            height: 300.0,
          ),
        ),
      ),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/layout/02/container_01.png" style="min-width:300px;max-width:600px;" alt="bule box"/>

このように背景色の指定、幅と高さを指定して青い四角を書いてみました。  
widthやheightを変更することで自由にサイズを変更することができます。  

### 色

色味についてもう少しみていきましょう。

#### Color

色の指定の仕方はColorクラスを使うことで可能です。  
Colorは以下のように指定できます。  

{{< highlight dart >}}
Color(0xFF42A5F5)
Color.fromARGB(0xFF, 0x42, 0xA5, 0xF5)
Color.fromARGB(255, 66, 165, 245)
Color.fromRGBO(66, 165, 245, 1.0)
{{< /highlight >}}
指定方法としてはWebやネイティブアプリでは一般的なRGBまたは16進数で指定することができます。

#### Colors

色指定のもう一つの方法としては、Colorsクラスを使うと簡単に色指定ができます。  
いくつもの色があるので、必要な色を探して利用するだけで済むでしょう。  

{{< highlight dart >}}
Colors.green
Colors.bule
Colors.red
{{< /highlight >}}

また、探した色が目的の色とそぐわない場合でも、濃淡を変更することが可能です。  
指定方法は以下の2種類があります。  

{{< highlight dart >}}
Colors.blue[900],
Colors.blue.shade900
{{< /highlight >}}

濃淡で指定できる数値はどちらの方法でも以下の通りです。

| 数値 |
| --- |
| 50 |
|100|
|200|
|300|
|400|
|500|
|600|
|700|
|800|
|900|

これ以外の数値は指定できません。  
そのため``Colors.blue[101]``などとしても色は表示できません。

### テキスト


ここに文字を表示してみましょう。

{{< highlight dart >}}
  child: Container(
    color: Colors.blue,
    width: 300.0,
    height: 300.0,
    child: Text('word')
  ),
{{< /highlight >}}

<img src="/images/basic/layout/02/container_02.png" style="min-width:300px;max-width:600px;" alt="text box"/>

小さくて見辛いと思いますがこれで文字が表示されます。

### 配置

#### Padding

次にPaddingをあててもう少し中心によってもらいましょう。

{{< highlight dart >}}
  child: Container(
    color: Colors.blue,
    width: 300.0,
    height: 300.0,
    child: Text('word'),
    padding: const EdgeInsets.all(50.0),
  ),
{{< /highlight >}}

<img src="/images/basic/layout/02/container_03.png" style="min-width:300px;max-width:600px;" alt="padding box"/>

Paddingは、EdgeInsetsクラスによって設定します。  
全周囲にかける場合は``EdgeInsets.all``特定の辺にのみPaddingを付ける場合は``EdgeInsets.only``を使いましょう。

{{< highlight dart >}}
    padding: const EdgeInsets.only(top:50.0),
{{< /highlight >}}
<img src="/images/basic/layout/02/container_04.png" style="min-width:300px;max-width:600px;" alt="padding top box"/>

#### Margin

Paddingを当てたので今度はMarginを指定してみましょう。

{{< highlight dart >}}
  child: Container(
    color: Colors.blue,
    width: 300.0,
    height: 300.0,
    child: Text('word'),
    margin: const EdgeInsets.all(100.0),
  ),
{{< /highlight >}}

<img src="/images/basic/layout/02/container_05.png" style="min-width:300px;max-width:600px;" alt="margin box"/>


横幅がMarginで入りきらなくなったため、押しつぶされてしまいましたが、このようにMarginもかけることが可能です。  
Paddingと同じく全周囲にかける場合は``EdgeInsets.all``特定の辺にのみMarginを付ける場合は``EdgeInsets.only``を使いましょう。

{{< highlight dart >}}
    margin: const EdgeInsets.only(left:50.0),
{{< /highlight >}}
<img src="/images/basic/layout/02/container_06.png" style="min-width:300px;max-width:600px;" alt="margin left box"/>


### 揃え

次に文字列の配置を調整してみましょう。  
Aligmentを使って中央に表示してみます。  

{{< highlight dart >}}
  child: Container(
    color: Colors.blue,
    width: 300.0,
    height: 300.0,
    child: Text('word'),
    alignment: Alignment.center,
  ),
{{< /highlight >}}

<img src="/images/basic/layout/02/container_07.png" style="min-width:300px;max-width:600px;" alt="alignment center"/>

Alignmentクラスには他にも以下のようなものがあります。

||||
|---|---|---|
|topLeft|topCenter|topRight|
|centerLeft|center|centerRight|
|bottomLeft|bottomCenter|bottomRight|

このように、子要素の配置箇所を揃えることが可能です。

### 変化

また、transformを使うことによって、形状を変化させることが可能です。  

{{< highlight dart >}}
  child: Container(
    color: Colors.blue,
    width: 300.0,
    height: 300.0,
    child: Text('word'),
    transform: Matrix4.rotationZ(0.1),
  ),
{{< /highlight >}}

<img src="/images/basic/layout/02/container_08.png" style="min-width:300px;max-width:600px;" alt="transform center"/>

TransformはX,Y,Z軸の回転やサイズ(Scale)の変更、子要素の一の変更(translate)などができます。


