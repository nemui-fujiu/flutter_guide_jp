+++
title = "レイアウト概要"
date = 2019-02-17T00:00:00+09:00
draft = false
weight = 311
description = "レイアウトの基本要素「Widget」について、どんなものがありどのように利用するのかを解説していきます。"
+++

## Widgetの基本的な種類と役割

Flutterの基本構成はほとんど全てWidgetです。 Flutterアプリで使われている画像、アイコン、テキストは全てウィジェットでできています。

行やカラム、グリッド、配置、制約、整列などレイアウトするために必要なウィジェットが揃っています。  

ウィジェットはツリー構造をイメージするとわかりやすいです。  
人によってはHTMLやXMLのようなものとしても理解しやすいかもしれません。

<img src="/images/basic/layout/01/icons.png" style="min-width:300px;border:1px solid gray" alt="icons"/>
<span id="debugPaintSizeEnabled"></span>
また、レイアウト構成を確認しながら作業をしたい場合は、「debugPaintSizeEnabled」を有効にすると以下のようになります。  
もともとアプリエンジニアであれば皆さんよく行う手法ですが、アイテムそれぞれの背景色を変更して見た目を確認する作業に似ていますね。

{{< highlight dart >}}
// パッケージを追加
import 'package:flutter/rendering.dart';

void main() {
  debugPaintSizeEnabled = true;
  runApp(MyApp());
}
{{< /highlight >}}

<img src="/images/basic/layout/01/icons_debug.png" style="min-width:300px;border:1px solid gray" alt="icons debug"/>

このように視覚的にわかりやすくなりましたが、どのようなウィジェットで構成されているのかをわかりやすくすると以下のような構造になります。

<img src="/images/basic/layout/01/icon_wire.svg" style="min-width:300px;max-width:600px;border:1px solid gray" alt="icon wire"/>

さらにツリー構造に直すと以下のようになります。

<img src="/images/basic/layout/01/tree.svg" style="min-width:300px;max-width:600px;border:1px solid gray" alt="tree"/>

### Container

さてここであえてピンクに色付けしてあるContainerについて説明します。  
Containerは内包する子ウィジェットをカスタマイズするために使うウィジェットクラスです。 機能としては、PaddingやMargin、Border、背景色の変更などを行うことができます。  
上記の例では、Rowに対してPaddingを追加し、Textに対してMarginを追加するために配置されています。

### Column & Row
ColumnとRowにはそれぞれ子要素の縦または横方向の配置方法、子の位置調整を設定するプロパティがあります。

### Text & Icon
後の制御はそれぞれのウィジェットで制御しており、Iconの色は「color」プロパティを。Textのフォント、色、太さなどの見た目は「style」プロパティで設定します。  


このようにいくつものウィジェットを組み合わせることで、レイアウトを作ることができます。


## レイアウトする。

レイアウトで使う個々のウィジェットの詳細を説明する前に、Flutterが元から提供しているデザインについても触れておきます。  
FlutterはGoogleが提供しており、Googleはデザイン面で[Material design](https://material.io/design/)を推奨しています。  
そのため、Flutterではデフォルトのデザインとしてマテリアルデザインが適用されます。


作り方は簡単で、「MaterialApp」クラス「Scaffold」クラスなどを利用して作成すると作成できます。
以下がそのソースです。

{{< highlight dart >}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Material Design',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Material Design Layout'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/layout/01/material_desgin.png" style="min-width:300px;max-width:600px;" alt="material desgin"/>

また、マテリアルデザインを利用せずに自由なレイアウトで作成したい場合は、以下のように一つづつの要素を組み合わせて、配置することで自由にデザイン可能
です。

{{< highlight dart >}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(color: Colors.white),
      child: Center(
        child: Text(
          'Hello World',
          textDirection: TextDirection.ltr,
          style: TextStyle(
            fontSize: 32,
            color: Colors.black,
          ),
        ),
      ),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/layout/01/non_material_desgin.png" style="min-width:300px;max-width:600px;" alt="non material desgin"/>

デザインが適用されている状態と言うのがどう言うことかと言うと、例えば上記の例だとTextクラスに着目するとわかりやすいです。  
マテリアルデザインを利用せずレイアウトする場合は、Textに必要な表示の詳細を設定しないと、正しく表示ができなくなってしまいます。    
以下のように変更して確認してみてください。

{{< highlight dart >}}
child: Text(
  'Hello World',
  textDirection: TextDirection.ltr,
),
{{< /highlight >}}

MaterialAppクラスを利用している時にはデフォルトで設定されていた文字色やサイズが当たらないため、文字が表示されていない状態になっているかと思います。  
このように、通常では細かく設定する必要があるデザインのデフォルト値をいろんなところで設定してくれます。  
