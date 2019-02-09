---
title: "リスト表示の作成"
date: 2019-02-08T00:00:00+09:00
draft: false
weight: 23
---

もっとアプリを作っている感じを出していきましょう。
リストの表示にチャレンジです。

## Step1

1.最初に```lib/main.dart```を以下のように書き換えましょう。

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp()); // ①

class MyApp extends StatelessWidget {// ②
  @override
  Widget build(BuildContext context) { // ③
    return MaterialApp(  // ④
      title: 'Welcome to Flutter',
      home: Scaffold( // ⑤
        appBar: AppBar( // ⑥　
          title: Text('Welcome to Flutter'),
        ),
        body: Center( // ⑦
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

起動するとこんな感じになるかと思います。

<img src="http://flutter.ctrnost.com/images/tutorial/03/01_base.png" width="600px"  alt="Base Program">

1.mainメソッドの箇所に書かれている「=>」はワンライナーで書くときの記述方式です。
通常の書き方だと以下のようになります。
```dart
void main()  {
  runApp(MyApp());
} 
```
2.「StatelessWidget」を継承することでアプリ自体がWidgetになります。  
Flutterでは、配置やパディング、レイアウトに関するほとんどすべてがウィジェットでできています。  
[Wifgets](https://flutter.io/docs/development/ui/widgets)

3.ウィジェットの「build()」メソッドでウィジェットのUIを作成します。
[build](https://docs.flutter.io/flutter/widgets/State/build.html)

4.今回のアプリは、マテリアルデザインのアプリを作成します。
マテリアルデザインは、Googleが提唱、推進しているデザインで、Webやモバイルアプリなどの標準的なビジュアルデザインを提供しています。  
Flutterでは豊富なマテリアルデザインのウィジェットが提供されています。
[Material design](https://material.io/design/)

5.Scaffoldウィジェットは、アプリケーションバーやタイトル、ホーム画面のウィジェットツリーを保持する、bodyプロパティを提供します。
ウィジェットのサブツリーはかなり複雑になる可能性があります。
[Scaffold](https://docs.flutter.io/flutter/material/Scaffold-class.html)

6.appBarにはWidgetのヘッダーに表示するアプリケーションバーを表現することができます。
[AppBar](https://docs.flutter.io/flutter/material/AppBar-class.html)

7.bodyではさまさまな表現が可能です、今回は「Center」で中央配置にし「Text」で文字列を配置しています。
[Layout](https://flutter.io/docs/development/ui/layout)


---

参考

[Write Your First Flutter App, part1](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt1/index.html?index=..%2F..index#0)
