+++
title = "スタンダード"
date = 2019-02-07T00:00:00+09:00
draft = false
weight = 411
description = "よく利用されるスタンダードなヘッダーを紹介します。ご自身で作りたいレイアウトを見つけて参考にしてみてください。"
keywords = "Flutter,アプリ,日本語,インストール,install,ヘッダー,AppBar"
+++


<span id="n_header"></span>
## ノーマルヘッダー
---

<img src="http://flutter.ctrnost.com/images/layout/header/standard/normal_header.png" style="min-width:300px" alt="normal header" />

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Standard AppBar',
      home: Scaffold(
          appBar:AppBar(
            title: Text('Standard AppBar'),
          )
      )
    );
  }
}

```

<span id="icon_header"></span>
## アイコン

---

<img src="http://flutter.ctrnost.com/images/layout/header/standard/icon_header.png" style="min-width:300px" alt="icon header" />

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
          appBar:AppBar(
            title: const Text('Standard AppBar'),
            actions: <Widget>[
              IconButton(
                icon: Icon(Icons.settings),
                onPressed: () {
                  // Pressed Action
                },
              ),
              IconButton(
                icon: Icon(Icons.menu),
                onPressed: () {
                  // Pressed Action
                },
              ),
            ],
          )
      )
    );
  }
}
```

<span id="popup_header"></span>
## ポップアップメニュー

---


<img src="http://flutter.ctrnost.com/images/layout/header/standard/popup_header.png" style="min-width:300px" alt="popup header" />

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
          appBar:AppBar(
            title: const Text('Standard AppBar'),
            actions: <Widget>[
              // overflow menu
              PopupMenuButton<Choice>(
                onSelected: (Choice choice) {
                  // Selected Action
                },
                itemBuilder: (BuildContext context) {
                  return choices.map((Choice choice) {
                    return PopupMenuItem<Choice>(
                      value: choice,
                      child: Text(choice.title),
                    );
                  }).toList();
                },
              ),
            ],
          )
      )
    );
  }
}

class Choice {
  const Choice({this.title, this.icon});
  final String title;
  final IconData icon;
}


const List<Choice> choices = const <Choice>[
  const Choice(title: 'Settings', icon: Icons.settings),
  const Choice(title: 'My Location', icon: Icons.my_location),
];
```

<span id="logo_header"></span>
## センターロゴヘッダー

---

<img src="http://flutter.ctrnost.com/images/layout/header/standard/logo_01.png" style="min-width:300px" alt="Logo1" />

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Standard AppBar',
      home: Scaffold(
        appBar:AppBar(
          title: Row(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Image.asset(
                'assets/logo.png',
                fit: BoxFit.contain,
                height: 32,
              ),
              Container(
               padding: const EdgeInsets.all(8.0),
                child: Text('YourAppTitle')
              )
            ],
          ),
        )
      )
    );
  }
}
```
※イメージの読み込み設定をしていない場合は[こちら](/settings/)

<span id="logo2_header"></span>
## 左寄せロゴヘッダー

---

<img src="http://flutter.ctrnost.com/images/layout/header/standard/logo_02.png" style="min-width:300px" alt="Logo2" />

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Standard AppBar',
      home: Scaffold(
        appBar:AppBar(
          centerTitle: true,
          title: const Text('YourAppTitle'),
          leading: Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Image.asset(
                  'assets/logo.png',
                  fit: BoxFit.contain,
                  height: 32,
                ),
              ]
          ),
        )
      )
    );
  }
}
```
※イメージの読み込み設定をしていない場合は[こちら](/settings/)
