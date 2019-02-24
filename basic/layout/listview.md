+++
title = "ListView"
date = 2019-02-24T00:00:00+09:00
draft = false
weight = 315
description = ""
+++

## ListView

ListViewは直線的に配置されたスクロール可能なウィジェットのリストです。
一覧作成を行う場合は一番一般的なスクロールウィジェットとなります。  

それでは細かいところを一つずつみていきましょう。

ListViewを構築するためには4つの方法があります。

1. デフォルトコンストラクタ(ListView())でリストを構築することが可能です。ただしインスタンス生成時に明示的に「List<Widget>」を受け取るため、表示するリストが少ない場合に利用するのがいいでしょう。
2. 「ListView.builder」で作成すると、実際に表示されている子要素のみビルダーが呼び出されるため随時読み込みなど、多数(または無限)のリストを表示する場合に利用する作成方法です。
3. 「ListView.separated」で作成すると、表示する項目と項目の間に区切りをつけることが可能になります。ただしリストの件数が固定の場合にのみ利用できる作成方法です。
4. 「ListView.custom」で作成すると、「SliverChildDelegate」を追加してカスタマイズできます。

今回は、1~3について説明していきます。

### 1. デフォルトコンストラクタ

主に、表示する要素が事前にわかっている場合に利用する書き方です。  
一番簡単にリスト表示が可能な書き方になります。  
読み込みと同時に全てのリストを描画するためあまり量の多いリストには向いていません。  
メニューの一覧や設定画面などで利用すると良いかと思います。  

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    const data = [
      Text("item0"),Text("item1"),Text("item2"),Text("item3"),Text("item4"),
    ];
    return MaterialApp(
      home: Scaffold(
        body: ListView(
          children: data
        ),
      ),
    );
  }
}
```
<img src="/images/basic/layout/05/listview_01.png" style="min-width:300px;max-width:600px;" alt="ListView"/>

デフォルトコンストラクタの場合は``children``のみが必須項目です。  
他の作成方法と違い、``builder``がないため、事前にウィジェットの一覧を作成しておく必要があります。  

以下のように記載すれば設定画面ぽくなります。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('ListView'),
        ),
        body: ListView(
          children: [
            _menuItem("メニュー1", Icon(Icons.settings)),
            _menuItem("メニュー2", Icon(Icons.map)),
            _menuItem("メニュー3", Icon(Icons.room)),
            _menuItem("メニュー4", Icon(Icons.local_shipping)),
            _menuItem("メニュー5", Icon(Icons.airplanemode_active)),
          ]
        ),
      ),
    );
  }

  Widget _menuItem(String title, Icon icon) {
    return GestureDetector(
      child:Container(
        padding: EdgeInsets.all(8.0),
        decoration: new BoxDecoration(
          border: new Border(bottom: BorderSide(width: 1.0, color: Colors.grey))
        ),
        child: Row(
          children: <Widget>[
            Container(
              margin: EdgeInsets.all(10.0),
              child:icon,
            ),
            Text(
              title,
              style: TextStyle(
                color:Colors.black,
                fontSize: 18.0
              ),
            ),
          ],
        )
      ),
      onTap: () {
        print("onTap called.");
      },
    );
  }
}
```

<img src="/images/basic/layout/05/listview_02.png" style="min-width:300px;max-width:600px;" alt="ListView"/>

このように事前にウィジャット一覧を作成することでメニュー一覧を作ることができました。  
ですが、もう少し簡単な方法もあります。   
``_menuItem``を以下のように書き換えてみましょう。  

```dart
  Widget _menuItem(String title, Icon icon) {
    return Container(
      decoration: new BoxDecoration(
        border: new Border(bottom: BorderSide(width: 1.0, color: Colors.grey))
      ),
      child:ListTile(
        leading: icon,
        title: Text(
          title,
          style: TextStyle(
            color:Colors.black,
            fontSize: 18.0
          ),
        ),
        onTap: () {
          print("onTap called.");
        }, // タップ
        onLongPress: () {
          print("onLongPress called.");
        }, // 長押し
      ),
    );
  }
```

<img src="/images/basic/layout/05/listview_03.png" style="min-width:300px;max-width:600px;" alt="ListView ListTile"/>

このように見た目はほとんど同じですが、記述量が減ったのがわかると思います。  
「ListTile」クラスを使うことによって、一般的によくあるリスト表示の表現を使うことができるので、特に見た目を変更する必要がなければ、「ListTile」を使うことをお勧めします。

### 2. ListView.builder

主に、表示する要素が事前にわからない場合に利用する書き方です。  
``itemBuilder``は画面表示時に実行されるため、無限にリストを作成することが可能です。  
チャットやメッセージ、検索結果の表示などに利用すると良いかと思います。 

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var list = ["メッセージ", "メッセージ", "メッセージ", "メッセージ", "メッセージ",];
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('ListView'),
        ),
        body: ListView.builder(
          itemBuilder: (BuildContext context, int index) {
            if (index >= list.length) {
              list.addAll(["メッセージ","メッセージ","メッセージ","メッセージ",]);
            }
            return _messageItem(list[index]);
          },
        )
      )
    );
  }

  Widget _messageItem(String title) {
    return Container(
      decoration: new BoxDecoration(
        border: new Border(bottom: BorderSide(width: 1.0, color: Colors.grey))
      ),
      child:ListTile(
        title: Text(
          title,
          style: TextStyle(
            color:Colors.black,
            fontSize: 18.0
          ),
        ),
        onTap: () {
          print("onTap called.");
        }, // タップ
        onLongPress: () {
          print("onLongTap called.");
        }, // 長押し
      ),
    );
  }
}
```

<img src="/images/basic/layout/05/listview_04.png" style="min-width:300px;max-width:600px;" alt="ListView.builder"/>

上記の例では``itemBuilder``でメッセージを表示するウィジェットを作成しています。  
また、listの件数が表示件数を上回る場合に追加でメッセージを生成しています。  
このようにすることで無限にリストが生成される状態になりました。

### 3. ListView.separated

表示する要素が事前にわかっており、一行ごとに何か要素を差し込みたい場合に利用する書き方です。  
indexは``itemBuilder``、``separatorBuilder``ともに0から始まります。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var list = ["メッセージ", "メッセージ", "メッセージ", "メッセージ", "メッセージ","メッセージ","メッセージ","メッセージ",];
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('ListView'),
        ),
        body: ListView.separated(
          itemBuilder: (BuildContext context, int index) {
            return _messageItem(list[index]);
          },
          separatorBuilder: (BuildContext context, int index) {
            return separatorItem();
          },
          itemCount: list.length,
        )
      )
    );
  }
  Widget separatorItem() {
    return Container(
      height: 10,
      color: Colors.orange,
    );
  }

  Widget _messageItem(String title) {
    return Container(
      decoration: new BoxDecoration(
        border: new Border(bottom: BorderSide(width: 1.0, color: Colors.grey))
      ),
      child:ListTile(
        title: Text(
          title,
          style: TextStyle(
            color:Colors.black,
            fontSize: 18.0
          ),
        ),
        onTap: () {
          print("onTap called.");
        }, // タップ
        onLongPress: () {
          print("onLongTap called.");
        }, // 長押し
      ),
    );
  }
}
```

<img src="/images/basic/layout/05/listview_05.png" style="min-width:300px;max-width:600px;" alt="ListView.separated"/>


## 参考
---

[ListView](https://docs.flutter.io/flutter/widgets/ListView-class.html)
