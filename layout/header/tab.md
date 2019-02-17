+++
title = "タブ"
date = 2019-02-07T00:00:00+09:00
draft = false
weight = 302
description = "タブヘッダーで表現できるレイアウトをいくつか紹介します。使いそうなパターンを提示していこうと思います。"
keywords = "Flutter,アプリ,日本語,インストール,install,ヘッダー,AppBar,タブ,tab"
+++

タブヘッダーで表現できるレイアウトをいくつか紹介します。使いそうなパターンを提示していこうと思います。


<span id="tab_header"></span>
## タブ

---

<img src="http://flutter.ctrnost.com/images/layout/header/tab/tab_header.png" style="min-width:300px" alt="tab header" />


```dart
import 'package:flutter/material.dart';

void main() => runApp(TabbedAppBarSample());

class TabbedAppBarSample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: DefaultTabController(
        length: choices.length,
        child: Scaffold(
          appBar: AppBar(
            title: const Text('Tabbed AppBar'),
            bottom: TabBar(
              tabs: choices.map((Choice choice) {
                return Tab(
                  text: choice.title,
                  icon: Icon(choice.icon),
                );
              }).toList(),
            ),
          ),
          body: TabBarView(
            children: choices.map((Choice choice) {
              return Padding(
                padding: const EdgeInsets.all(16.0),
                child: ChoiceCard(choice: choice),
              );
            }).toList(),
          ),
        ),
      ),
    );
  }
}

class Choice {
  const Choice({this.title, this.icon});

  final String title;
  final IconData icon;
}

const List<Choice> choices = const <Choice>[
  const Choice(title: 'CAR', icon: Icons.directions_car),
  const Choice(title: 'BICYCLE', icon: Icons.directions_bike),
  const Choice(title: 'BOAT', icon: Icons.directions_boat),
];

class ChoiceCard extends StatelessWidget {
  const ChoiceCard({Key key, this.choice}) : super(key: key);

  final Choice choice;

  @override
  Widget build(BuildContext context) {
    final TextStyle textStyle = Theme.of(context).textTheme.display1;
    return Card(
      color: Colors.white,
      child: Center(
        child: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: <Widget>[
            Icon(choice.icon, size: 128.0, color: textStyle.color),
            Text(choice.title, style: textStyle),
          ],
        ),
      ),
    );
  }
}
```

<span id="tab_no_icon_header"></span>
## タブ(アイコンなし)

---

先に紹介したタブヘッダーを以下のように変更すると、アイコンなしタブヘッダーを実現できます。

<img src="http://flutter.ctrnost.com/images/layout/header/tab/tab_header02.png" style="min-width:300px" alt="tab2 header" />

以下のようにTabの引数からIconを抜けばOKです。

```dart
  appBar: AppBar(
    title: const Text('Tabbed AppBar'),
    bottom: TabBar(
      tabs: choices.map((Choice choice) {
        return Tab(
          text: choice.title,
        );
      }).toList(),
    ),
  ),
```

<span id="tab_scroll_header"></span>
## スクロールタブ

---

先に紹介したタブヘッダーを以下のように変更すると、スクロールタブを実現できます。

<img src="http://flutter.ctrnost.com/images/layout/header/tab/scroll_tab_header.png" style="min-width:300px" alt="scroll tab header" />

1. TabbedAppBarSampleクラスのTabBarに``isScrollable: true``を追加
    ```dart
    appBar: AppBar(
      title: const Text('Tabbed AppBar'),
      bottom: TabBar(
        isScrollable: true,
        tabs: choices.map((Choice choice) {
          return Tab(
            text: choice.title,
            icon: Icon(choice.icon),
          );
        }).toList(),
      ),
    ),
    ```

2. 表示が3つだとわかりづらいので表示するタブ数を増やして完了
    ```dart
    const List<Choice> choices = const <Choice>[
      const Choice(title: 'CAR', icon: Icons.directions_car),
      const Choice(title: 'BICYCLE', icon: Icons.directions_bike),
      const Choice(title: 'BOAT', icon: Icons.directions_boat),
      const Choice(title: 'BUS', icon: Icons.directions_bus),
      const Choice(title: 'TRAIN', icon: Icons.directions_railway),
      const Choice(title: 'WALK', icon: Icons.directions_walk),
    ];
    ```
