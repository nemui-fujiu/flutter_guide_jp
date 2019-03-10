+++
title = "Drawer"
date = 2019-03-09T00:00:00+09:00
draft = false
weight = 355
description = "「Drawer」はAndroidアプリでよく見るUIで、左からドロワーを表示し、メニューなどを配置するのに利用します。今回は「Drawer」について解説していきます。"
+++

## Drawer

「Drawer」はAndroidアプリでよく見るUIで、左からドロワーを表示し、メニューなどを配置するのに利用します。   
今回は「Drawer」について解説していきます。

```dart
class _MainPageState extends State<MainPage> {

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Drawer'),
      ),
      drawer: Drawer(
        child: ListView(
          children: <Widget>[
            DrawerHeader(
              child: Text('Drawer Header'),
              decoration: BoxDecoration(
                color: Colors.blue,
              ),
            ),
            ListTile(
              title: Text("Item 1"),
              trailing: Icon(Icons.arrow_forward),
            ),
            ListTile(
              title: Text("Item 2"),
              trailing: Icon(Icons.arrow_forward),
            ),
          ],
        ),
      ),
    );
  }
}
```

<img src="/images/basic/navigation/05/drawer_01.gif" style="min-width:300px;max-width:600px;" alt="Drawer"/>

「Drawer」を使うときは「AppBar」の``drawer``で作成します。  
子要素として「ListView」を持つようにします。  
必ずしも「ListView」でなければいけない訳ではないですが、通常のドロワー形式でメニューを作る場合は「ListView」で対応できるかと思います。

```dart
  drawer: Drawer(
    child: ListView(
    ),
  ),
```

あとは通常の「ListView」の使い方と代わりはありません。  
ただ、「DrawerHeader」を使うことでヘッダー要素を作ることが可能です。

```dart
child: ListView(
  children: <Widget>[
    DrawerHeader(
      child: Text('Drawer Header'),
      decoration: BoxDecoration(
        color: Colors.blue,
      ),
    ),
    ListTile(
      title: Text("Item 1"),
      trailing: Icon(Icons.arrow_forward),
    ),
    ListTile(
      title: Text("Item 2"),
      trailing: Icon(Icons.arrow_forward),
    ),
  ],
),
```

このようにして簡単に「Drawer」の作成ができました。

ListViewについては[こちら](/basic/layout/listview/#list_view)   
ListTileについては[こちら](/basic/layout/listview/#list_tile)
