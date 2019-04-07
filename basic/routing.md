+++
title = "画面遷移(Navigator)"
date = 2019-02-21T00:00:00+09:00
draft = false
weight = 359
description = "画面遷移(Navigator)の移動(遷移)の仕組み、使い方を紹介します。必ず使う機能なので確実に押さえておきましょう。"
+++

## Navigator

画面遷移(Navigator)の移動(遷移)の仕組み、使い方を紹介します。必ず使う機能なので確実に押さえておきましょう。

アプリの画面を移動(遷移)の仕組みから説明しますと、まずすると画面を移動(遷移)をおこなうと、前の画面の上に1つ画面がかぶさる(オーバーレイ)されている状態になります。
これにより積み重なったウィジェットはユーザの画面移動の履歴となります。

この状態から、前の画面に戻るのであれば、一番上にあるウィジェットを取り除けば前の画面に戻ることが可能です。

もちろん常にウィジェットの新規作成と破棄を繰り返すことで画面を表現することも可能ですが、そうすると画面の再作成が頻発するため、処理が重くなってしまいます。

<img src="/images/basic/navigation/06/navi_push_pop.svg" style="min-width:300px;max-width:800px;" alt="PushとPop">

このようにアプリでは画面をプッシュ、ポップして遷移を行なっています。   
効率よく画面を作成、破棄して画面を遷移できるようにしましょう。


見た目上わかりやすい例とするなら、ダイアログの表示がわかりやすいかと思います。
ダイアログが呼び出され、元の画面の上にダイアログが被さります。
ダイアログを閉じると、前の画面に戻ります。

画面遷移の方法は2種類あり、事前に``routes``を登録して遷移先を決める方法と、必要に応じて``routes``を登録するやり方があります。

## 画面遷移(Navigator.of(context).pushNamed)

事前に定義した状態での画面遷移から説明していきます。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MainPage(),
      routes: <String, WidgetBuilder> {
        '/home': (BuildContext context) => new MainPage(),
        '/subpage': (BuildContext context) => new SubPage()
      },
    );
  }
}
class MainPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Navigator'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget>[
              Text('Main'),
              RaisedButton(onPressed: () => Navigator.of(context).pushNamed("/subpage"), child: new Text('Subページへ'),)
            ],
          ),
        ),
      ),
    );
  }
}
class SubPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Navigator'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget>[
              Text('Sub'),
              RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る'),)
            ],
          ),
        ),
      ),
    );
  }
}
```

事前に``routes``を定義する場合は以下のように定義します。
```dart
return MaterialApp(
  home: MainPage(),
  routes: <String, WidgetBuilder> {
    '/home': (BuildContext context) => new MainPage(),
    '/subpage': (BuildContext context) => new SubPage()
  },
);
```
``/home``のようなルーティング名称に対して、表示されるページを作成しウィジェットを設定します。

```dart
'/home': (BuildContext context) => new MainPage(),
```

遷移するには``Navigator.of(context).pushNamed``に対して遷移先の名称を渡すことで、対象のウィジェットを呼び出します。

```dart
RaisedButton(onPressed: () => Navigator.of(context).pushNamed("/subpage"), child: new Text('Subページへ'),)
```

呼び出されたウィジェットは``MainPage``の上に``SubPage``になりナビゲーションヘッダーに戻るボタンが表示されているのがわかると思います。

```dart
class SubPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Navigator'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget>[
              Text('Sub'),
              RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る'),)
            ],
          ),
        ),
      ),
    );
  }
}
```

``SubPage``から遷移元の画面へ戻るには、ナビゲーションヘッダーの戻るボタンを押下するか、``Navigator.of(context).pop()``を行い、``SubPage``をポップして取り除くことで、元のページへ戻ります。

```dart
RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る'),)
```

## 画面遷移(Navigator.of(context).pushReplacementNamed)

先ほど説明した``pushName``以外にも遷移方法があります。  
``pushReplacementNamed``を使うことで、前の画面に戻ることができない遷移を作成することが可能です。

```dart
class MainPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Navigator'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget>[
              Text('Main'),
              RaisedButton(onPressed: () => Navigator.of(context).pushReplacementNamed("/subpage"), child: new Text('Subページへ'),)
            ],
          ),
        ),
      ),
    );
  }
}

class SubPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Navigator'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget>[
              Text('Sub'),
            ],
          ),
        ),
      ),
    );
  }
}
```

<img>

このように``SubPage``へ遷移しましたが、ナビゲーションヘッダーに戻るボタンが表示されます。  
かりに、``Navigator.of(context).pop()``を使って戻ろうとした場合にどうなるかというと、現在の画面(SubPage)をポップしてしまうので、全ての画面ウィジェットがなくなった状態になり、真っ黒な画面が表示されるかと思われます。

<img src="/images/basic/navigation/06/navi_replace.svg" style="min-width:300px;max-width:600px;" alt="PushとPop">


## 画面遷移(Navigator.popUntil(context, ModalRoute.withName("/home")))

``Navigator.popUntil``は指定した条件(``ModalRoute.withName("/sub1page")``)の対象をポップするまで間の画面をポップし続けます。   
この時、``/home``(最初の画面)を指定すると、``/home``もポップしようとしてエラーになるため注意してください。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MainPage(),
      routes: <String, WidgetBuilder> {
        '/home': (BuildContext context) => new MainPage(),
        '/sub1page': (BuildContext context) => new SubPage(page: Pages.PAGE1),
        '/sub2page': (BuildContext context) => new SubPage(page: Pages.PAGE2),
        '/sub3page': (BuildContext context) => new SubPage(page: Pages.PAGE3)
      },
    );
  }
}

class MainPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Navigator'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget>[
              Text('Main'),
              RaisedButton(onPressed: () => Navigator.of(context).pushNamed("/sub1page"), child: new Text('Subページへ'),)
            ],
          ),
        ),
      ),
    );
  }
}

enum Pages{
  PAGE1,
  PAGE2,
  PAGE3,
}
class SubPage extends StatelessWidget {
  final Pages page;
  SubPage({this.page});

  @override
  Widget build(BuildContext context) {
    List<Widget> widget;
    switch(page) {
      case Pages.PAGE1:
        widget =  <Widget>[
          Text('Sub1'),
          RaisedButton(onPressed: () =>  Navigator.of(context).pushNamed("/sub2page"), child: new Text('次へ'),),
          RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る')),
        ];
        break;
      case Pages.PAGE2:
        widget =  <Widget>[
          Text('Sub2'),
          RaisedButton(onPressed: () =>  Navigator.of(context).pushNamed("/sub3page"), child: new Text('次へ'),),
          RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る')),
        ];
        break;
      case Pages.PAGE3:
        widget =  <Widget>[
          Text('Sub3'),
          RaisedButton(onPressed: () => Navigator.of(context).popUntil(ModalRoute.withName("/sub1page")), child: new Text('ホームへ')),
          RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る')),
        ];
        break;

    }
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Navigator'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: widget
          ),
        ),
      ),
    );
  }
}
```


<img src="/images/basic/navigation/06/navi_until.svg" style="min-width:300px;max-width:500px;" alt="PushとPop">


##  画面遷移(Navigator.pushNamedAndRemoveUntil)

``pushNamedAndRemoveUntil``は第2引数で指定した画面まで画面をポップして、第2引数の画面をプッシュします。

```dart
class SubPage extends StatelessWidget {
  final Pages page;
  SubPage({this.page});

  @override
  Widget build(BuildContext context) {
    List<Widget> widget;
    switch(page) {
      case Pages.PAGE1:
        widget =  <Widget>[
          Text('Sub1'),
          RaisedButton(onPressed: () =>  Navigator.of(context).pushNamed("/sub2page"), child: new Text('次へ'),),
          RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る')),
        ];
        break;
      case Pages.PAGE2:
        widget =  <Widget>[
          Text('Sub2'),
          RaisedButton(onPressed: () =>  Navigator.of(context).pushNamed("/sub3page"), child: new Text('次へ'),),
          RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る')),
        ];
        break;
      case Pages.PAGE3:
        widget =  <Widget>[
          Text('Sub3'),
          RaisedButton(onPressed: () => Navigator.of(context).pushNamedAndRemoveUntil("/sub1page", ModalRoute.withName("/home")), child: new Text('サブ1へ')),
          RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る')),
        ];
        break;

    }
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Navigator'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: widget
          ),
        ),
      ),
    );
  }
}
```

初期画面(スプラッシュやホーム)に戻す時に便利です。
``(_) => false``とすることで、画面を新規にプッシュせずに終わることができるので、初期画面(スプラッシュやホーム)に戻す時に便利です。

```dart
RaisedButton(onPressed: () => Navigator.pushNamedAndRemoveUntil(context, "/home", (_) => false), child: new Text('ホームへ')),
```


## ルーティング書かない遷移

最初にルーティングを書かないで画面遷移することも可能です。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MainPage(),
    );
  }
}

class MainPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Navigator'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget>[
              Text('Main'),
              RaisedButton(onPressed: () => Navigator.of(context).push(MaterialPageRoute(builder: (context) {
                return SubPage();
              })), child: new Text('Subページへ'),)
            ],
          ),
        ),
      ),
    );
  }
}

class SubPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Navigator'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget> [
              Text('Sub1'),
              RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る')),
            ]
          ),
        ),
      ),
    );
  }
}
```

以下のように``Navigator.of(context).push``と``MaterialPageRoute``を利用して画面を呼び出します。

```dart
RaisedButton(onPressed: () => Navigator.of(context).push(MaterialPageRoute(builder: (context) {
  return SubPage();
})), child: new Text('Subページへ'),)
```

この形で呼び出した画面であっても、プッシュされていることには変わりないため、ポップすることで前の画面へ戻る動作が可能です。

```dart
RaisedButton(onPressed: () => Navigator.of(context).pop(), child: new Text('戻る')),
```

また、ルーティング名をつけることで、今まで紹介した画面遷移を利用することが可能になります。

```dart
MaterialPageRoute(
  settings: const RouteSettings(name: "/sub1page"),
  builder: (context) {
    return SubPage();
  }
),
```

他にも今まで紹介した方法と同じ画面遷移が提供されています。

### 画面の置き換えをして前画面への遷移をできなくする処理

```dart
RaisedButton(onPressed: () => Navigator.of(context).pushReplacement(
  MaterialPageRoute(
    settings: const RouteSettings(name: "/detail"),
    builder: (context) {
      return SubPage();
    }
  ),
), child: new Text('Subページへ'),)
```

### 特定の画面まで、画面をポップして新規に画面を作成する処理

```dart
RaisedButton(onPressed: () => Navigator.of(context).pushAndRemoveUntil(
  MaterialPageRoute(
    settings: const RouteSettings(name: "/subpage"),
    builder: (context) {
      return SubPage();
    }
  ),
  ModalRoute.withName("/home"),
), child: new Text('Subページへ'),)
```

