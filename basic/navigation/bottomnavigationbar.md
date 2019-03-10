+++
title = "BottomNavigationBar"
date = 2019-03-09T00:00:00+09:00
draft = false
weight = 352
description = "画面の下部にナビゲーションメニューを配置する時に利用するのが「BottomNavigationBar」です。今回は、「BottomNavigationBar」を使ったサンプルを説明していきます。"
+++

## BottomNavigationBar

画面の下部にナビゲーションメニューを配置する時に利用するのが「BottomNavigationBar」です。  
今回は、「BottomNavigationBar」を使ったサンプルを説明していきます。  

```dart
class _MainPageState extends State<MainPage> {

  int _currentIndex = 0;
  final _pageWidgets = [
    PageWidget(color:Colors.white, title:'Home'),
    PageWidget(color:Colors.blue, title:'Album'),
    PageWidget(color:Colors.orange, title:'Chat'),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('BottomNavigationBar'),
      ),
      body: _pageWidgets.elementAt(_currentIndex),
      bottomNavigationBar: BottomNavigationBar(
        items: <BottomNavigationBarItem>[
          BottomNavigationBarItem(icon: Icon(Icons.home), title: Text('Home')),
          BottomNavigationBarItem(icon: Icon(Icons.photo_album), title: Text('Album')),
          BottomNavigationBarItem(icon: Icon(Icons.chat), title: Text('Chat')),
        ],
        currentIndex: _currentIndex,
        fixedColor: Colors.blueAccent,
        onTap: _onItemTapped,
        type: BottomNavigationBarType.fixed,
      ),
    );
  }

  void _onItemTapped(int index) => setState(() => _currentIndex = index );
}

class PageWidget extends StatelessWidget {
  final Color color;
  final String title;

  PageWidget({Key key, this.color, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: color,
      child: Center(
        child: Text(
          title,
          style: TextStyle(
            fontSize: 25,
          ),
        ),
      ),
    );
  }
}
```

<img src="/images/basic/navigation/02/bottomnavicationbar_01.gif" style="min-width:300px;max-width:600px;" alt="BottomNavigationBar"/>

- ``items``はメニューボタンとなるウィジェットの一覧を設定します。
- ``currentIndex``は選択されているIndexです。
- ``fixedColor``は選択状態の時のアイコンとテキストの色を指定します。
- ``type``は選択状態の見た目を変更することができます。
- ``onTap``はメニューをタップした時のイベントを設定します。

まず、メニュー部分を「BottomNavigationBar」で作成します。

```dart
  bottomNavigationBar: BottomNavigationBar(
    items: <BottomNavigationBarItem>[
      BottomNavigationBarItem(icon: Icon(Icons.home), title: Text('Home')),
      BottomNavigationBarItem(icon: Icon(Icons.photo_album), title: Text('Album')),
      BottomNavigationBarItem(icon: Icon(Icons.chat), title: Text('Chat')),
    ],
    currentIndex: _currentIndex,
    fixedColor: Colors.blueAccent,
    onTap: _onItemTapped,
    type: BottomNavigationBarType.fixed,
  ),
```

BottomNavigationBarには表示するナビゲーションメニューを``items``に「BottomNavigationBarItem」のリストを設定します。  
「BottomNavigationBarItem」はメニューとして以下のような要素を定義します。  

```dart
items: <BottomNavigationBarItem>[
  BottomNavigationBarItem(icon: Icon(Icons.home), title: Text('Home')),
  BottomNavigationBarItem(icon: Icon(Icons.photo_album), title: Text('Album')),
  BottomNavigationBarItem(icon: Icon(Icons.chat), title: Text('Chat')),
],
```

- ``icon``はメニューの表示アイコンです。
- ``activeIcon``は選択状態のアイコンを指定します。  
これにより、未選択状態のアイコンと選択状態のアイコンを変えることができます。  
- ``title``はタイトルを表示したい時に記載してください。  

次に``onTap``でアイコンを押された時の動作を作成します。 

```dart
        onTap: _onItemTapped,
        type: BottomNavigationBarType.fixed,
      ),
    );
  }

  void _onItemTapped(int index) => setState(() => _currentIndex = index );
```

今回は、``_pageWidgets``変数にページの配列を準備しているので、``_onItemTapped``では``_currentIndex``を更新のみをしています。

```dart
  final _pageWidgets = [
    PageWidget(color:Colors.white, title:'Home'),
    PageWidget(color:Colors.blue, title:'Album'),
    PageWidget(color:Colors.orange, title:'Chat'),
  ];
```

これにより、``body``に設定されている``_pageWidgets``の対象ページを取得して、画面に表示さ流ようになるので、これで完成です。

```dart
body: _pageWidgets.elementAt(_currentIndex),
```

「BottomNavigationBar」は選択時の見た目を``type``によって変えることが可能です。  
``BottomNavigationBar.type``には以下の2種類があり、それぞれ以下のような見た目になるので、使い分けてみてください。

- BottomNavigationBarType.fixed,
<img src="/images/basic/navigation/02/bottomnavicationbar_02.gif" style="min-width:300px;max-width:600px;" alt="BottomNavigationBarType.fixed"/>

- BottomNavigationBarType.shifting
<img src="/images/basic/navigation/02/bottomnavicationbar_03.gif" style="min-width:300px;max-width:600px;" alt="BottomNavigationBarType.shifting"/>

この時、「BottomNavigationBarItem」の背景色が指定されていないと、真っ白になってしまうので、色指定を忘れずに行いましょう。
```dart
items: <BottomNavigationBarItem>[
  BottomNavigationBarItem(icon: Icon(Icons.home), backgroundColor: Colors.blueAccent, title: Text('Home')),
  BottomNavigationBarItem(icon: Icon(Icons.photo_album), backgroundColor: Colors.blueAccent, title: Text('Album')),
  BottomNavigationBarItem(icon: Icon(Icons.chat), backgroundColor: Colors.blueAccent, title: Text('Chat')),
],
```
