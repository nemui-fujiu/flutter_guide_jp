+++
title = "SnackBar"
date = 2019-03-21T00:00:00+09:00
draft = false
weight = 363
description = "SnackBarを使う場合いくつかの方法があります。一番単純なのはStatelessWidgetを使った方法です。"
+++

## SnackBar

SnackBarを使う場合いくつかの方法があります。
一番単純なのはStatelessWidgetを使った方法です。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('SnackBar'),
        ),
        body: SnackBarPage(),
      ),
    );
  }
}

class SnackBarPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Center(
      child: RaisedButton(
        onPressed: () {
          final snackBar = SnackBar(
            content: Text('お知らせ！'),
            action: SnackBarAction(
              label: 'とじる',
              onPressed: () {
                Scaffold.of(context).removeCurrentSnackBar();
              },
            ),
            duration: Duration(seconds: 3),
          );
          Scaffold.of(context).showSnackBar(snackBar);
        }, 
        child: Text('スナックバーを開く'),
      ),
    );
  }
}
```

<img src="/images/basic/dialog/03/snack_bar.gif" style="min-width:300px;max-width:600px;" alt="SnackBar"/>


スナックバーを表示するにあたり、まずは表示したい情報をSnackBarクラスで作成します。

```dart
  final snackBar = SnackBar(
    content: Text('お知らせ！'),
  ),
```

作成したSnackBarを``Scaffold.of``の``showSnackBar``に渡すことで、Scaffoldで作成されている画面の下部からスナックバーが表示されます。

```dart
  Scaffold.of(context).showSnackBar(snackBar);
```

スナックバーには、``action``によるボタン追加や``duration``による表示時間の制御が可能です。

さらに詳細に説明すると以下のようになっています。

- ``Scaffold.of``は、ScaffoldStateを取得しスナックバーの表示とアニメーションを管理します。
- ``ScaffoldState.showSnackBar``を表示するの時に利用します。
- ``ScaffoldState.removeCurrentSnackBar``は現在表示されている、スナックバーがある場合非表示にします。
- ``SnackBarAction``は、スナックバー内で表示するボタンを作成できます。


StatefulWidgetでスナックバーを表示したい場合は以下のようにします。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('SnackBar'),
        ),
        body: MainPage()
      )
    );
  }
}

class MainPage extends StatefulWidget {
  @override
  _MainPageState createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {

  @override
  Widget build(BuildContext context) {
    return Center(
        child: RaisedButton(
          onPressed: (){
            final snackBar = SnackBar(
              content: Text('お知らせ！'),
            );
            Scaffold.of(context).showSnackBar(snackBar);
          },
          child: Text('スナックバーを開く'),
        )
    );
  }
}
```

この時、_MainPageStateクラス内に``home: Scaffold(``が定義されていると正しく動作しないため注意が必要です。

どうしても_MainPageStateクラス内に``Scaffold``を定義したい場合は以下のように``GlobalKey``により``ScaffoldState``を持つこと、作成することが可能です。

```dart
class _MainPageState extends State<MainPage> {

  final GlobalKey<ScaffoldState> _scaffoldstate = new GlobalKey<ScaffoldState>();

  void _showBar(){
    _scaffoldstate.currentState.showSnackBar(new SnackBar(content: new Text('お知らせ！')));
  }
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      key: _scaffoldstate,
      appBar: AppBar(
        title: Text('SnackBar'),
      ),
      body: Center(
        child: RaisedButton(
          onPressed: _showBar,
          child: Text('スナックバーを開く'),
        )
      ),
    );
  }
}
```
