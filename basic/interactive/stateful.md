+++
title = "Stateful"
date = 2019-03-02T00:00:00+09:00
draft = false
weight = 322
description = "StatefulWidgetについて説明していきます。慣れるためにStatefulクラスを使い値を保持するアプリケーションを作っていきましょう。  "
+++

## StatefulWidget

StatefulWidgetについて説明していきます。
FlutterにはStatelessとStatefulの2種類のWidgetがあります。

[StatefulWidget](/tutorial/tutorial05/)ではウィジェットの生存期間中に変更される値を維持することができます。  
StatelessWidgeとStatefulWidgetの違いについては[こちら](/tutorial/tutorial05/)を確認ください。

<img src="/images/basic/interactive/01/stateful_01.png" style="min-width:300px;max-width:600px;" alt="Stateful"/>

どのようなものかを慣れるためにStatefulクラスを使い値を保持するアプリケーションを作っていきましょう。  

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return
  }
}
```

これで``StatelessWidget``の準備ができました。  
これから、``StatefulWidget``と``State``を作っていきます。

```dart
class ClickGood extends StatefulWidget {
  @override
  _ClickGoodState createState() => _ClickGoodState();
}
class _ClickGoodState extends State<ClickGood> {
  Widget build(BuildContext context) {
    return
  }
}
```

次に``State``に状態を持つための変数を用意しましょう。

```dart
class _ClickGoodState extends State<ClickGood> {
  bool _active = false;
  
  Widget build(BuildContext context) {
    return
  }
}
```

次にクリック時の処理を作成していきます。    
今回は処理をわかりやすくするため、クリック時の動作をメソッド化しておきます。    
状態を保持する変数を変更する場合は必ず``setState``を実装し、その中で値を変更するようにしなければなりません。  

```dart
class _ClickGoodState extends State<ClickGood> {
  bool _active = false;
  
  void _handleTap() {
    setState(() {
      _active = !_active;
    });
  }
  
  Widget build(BuildContext context) {
    return
  }
}
```

クリックイベントとして、``_handleTap``を呼び出すことで、``_active``がクリックされるたびにtrue・falseと入れ替わるようになります。  
あとは画面に表示するレイアウトを作成してください。  
これでユーザの動作に応じて状態が変化するインタラクティブなアプリができました。

今回のプログラムの全文はこちら。  

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Stateful',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Stateful'),
        ),
        body: Center(
          child: ClickGood(),
        ),
      ),
    );
  }
}

class ClickGood extends StatefulWidget {
  @override
  _ClickGoodState createState() => _ClickGoodState();
}

class _ClickGoodState extends State<ClickGood> {
  bool _active = false;

  void _handleTap() {
    setState(() {
      _active = !_active;
    });
  }

  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: _handleTap,
      child: Container(
        child: Column(
          children: <Widget>[
            Container(
              child: Center(
                child: new Icon(
                  Icons.thumb_up,
                  color: _active ? Colors.orange[700] : Colors.grey[500],
                  size: 100.0,
                ),
              ),
              width: 200.0,
              height: 200.0,
            ),
            Container(
              child: Center(
                child: Text(
                _active ? 'Active' : 'Inactive',
                style: TextStyle(fontSize: 32.0, color: Colors.white),
                ),
              ),
              width: 200.0,
              height: 50.0,
              decoration: BoxDecoration(
                color: _active ? Colors.orange[700] : Colors.grey[600],
              ),
            ),
          ]
        ),
      )
    );
  }
}
```

<img src="/images/basic/interactive/01/stateful_02.gif" style="min-width:300px;max-width:600px;" alt="Stateful movie"/>
