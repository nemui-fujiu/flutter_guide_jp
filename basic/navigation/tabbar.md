+++
title = "TabBar"
date = 2019-03-09T00:00:00+09:00
draft = false
weight = 353
description = "「TabBar」を使うことで、簡単にヘッダーにタブバーを作ることができるようになります。今回は、「TabBar」を使ったサンプルを説明していきます。"
+++

## TabBar

「TabBar」を使うことで、簡単にヘッダーにタブバーを作ることができるようになります。   
今回は、「TabBar」を使ったサンプルを説明していきます。 

```dart
class _MainPageState extends State<MainPage> {

  final _tab = <Tab> [
    Tab( text:'Car', icon: Icon(Icons.directions_car)),
    Tab( text:'Bisycle', icon: Icon(Icons.directions_bike)),
    Tab( text:'Boat', icon: Icon(Icons.directions_boat)),
  ];

  Widget build(BuildContext context) {
    return DefaultTabController(
      length: _tab.length,
      child: Scaffold(
        appBar: AppBar(
          title: const Text('TabBar'),
          bottom: TabBar(
            tabs: _tab,
          ),
        ),
        body: TabBarView(
            children: <Widget> [
              TabPage(title: 'Car', icon: Icons.directions_car),
              TabPage(title: 'Bisycle', icon: Icons.directions_bike),
              TabPage(title: 'Boat', icon: Icons.directions_boat),
            ]
        ),
      ),
    );
  }
}

class TabPage extends StatelessWidget {

  final IconData icon;
  final String title;

  const TabPage({Key key, this.icon, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final TextStyle textStyle = Theme.of(context).textTheme.display1;
    return Center(
      child:Column(
        mainAxisSize: MainAxisSize.min,
        crossAxisAlignment: CrossAxisAlignment.center,
        children: <Widget>[
          Icon(icon, size: 64.0, color: textStyle.color),
          Text(title, style: textStyle),
        ],
      ),
    );
  }
}
```
<img src="/images/basic/navigation/03/tabbar_01.gif" style="min-width:300px;max-width:600px;" alt="TabBar"/>


- ``isScrollable``をtrueにするとタブをスクロールできるようになります。
- ``indicatorColor``はタブ下に表示さている線の色を指定できます。
- ``indicatorWeight``はタブ下に表示さている線の太さをしていてきます。
- ``indicatorPadding``はタブ下に表示さている線にPaddingを与えることができ、指定は「EdgeInsets」で行います。  
- ``indicatorSize``はタブ下に表示さている線に対して、以下二つの要素どちらかによって幅が変わります。
  - ``TabBarIndicatorSize.tab``はタブと同じ幅
  - ``TabBarIndicatorSize.tab``はラベルと同じ幅
- ``indicator``はタブ下に表示さている線に「Decoration」を使ってデザインの変更ができます。
- ``labelColor``は表示しているラベルの色
- ``labelStyle``はラベルのスタイルを「TextStyle」で指定できます。
- ``labelPadding``はラベルのPaddin要素を指定します。
- ``unselectedLabelColor``は未選択状態でのアイコンラベルの表示色を指定できます。
- ``unselectedLabelStyle``は未選択状態でのラベルのスタイルを「TextStyle」で指定できます。


「TabBar」を作るときは、「TabBar」、「TabBarView」に「TabController」クラスを設定するか、「DefaultTabController」の子要素としてそれぞれを作成する方法の2種類があります。
まずは、「DefaultTabController」での作成方法をみていきましょう。  

```dart
return DefaultTabController(
  length: _tab.length,
  child: Scaffold(
    appBar: AppBar(
      title: const Text('TabBar'),
      bottom: TabBar(
        tabs: _tab,
      ),
    ),
    body: TabBarView(
      children: <Widget> [
        TabPage(title: 'Car', icon: Icons.directions_car),
        TabPage(title: 'Bisycle', icon: Icons.directions_bike),
        TabPage(title: 'Boat', icon: Icons.directions_boat),
      ]
    ),
  ),
);
```

「DefaultTabController」には必ず、「TabBar」で表示するタブ数を``length``に設定しておかなければなりません。

```dart
return DefaultTabController(
  length: _tab.length,
  child: Scaffold(
    appBar: AppBar(
```

次に「AppBar」の``bottom``に「TabBar」クラスを定義して、表示するタブのリストを渡します。  

```dart
appBar: AppBar(
  title: const Text('TabBar'),
  bottom: TabBar(
    tabs: _tab,
  ),
),
```

今回は「Tab」の一覧は``_tab``変数に定義しておきます。

```dart
  final _tab = <Tab> [
    Tab( text:'Car', icon: Icon(Icons.directions_car)),
    Tab( text:'Bisycle', icon: Icon(Icons.directions_bike)),
    Tab( text:'Boat', icon: Icon(Icons.directions_boat)),
  ];
```

これで、タブの表示ができましたが、画面表示の切り替えも確認しましょう。  
``body``に「TabBarView」クラスを定義し、表示する画面となるウィジェットの一覧を渡します。  
今回は「TabPage」という画面ウィジェットを作ってあるので、一覧として設定して完成です。

```dart
body: TabBarView(
  children: <Widget> [
    TabPage(title: 'Car', icon: Icons.directions_car),
    TabPage(title: 'Bisycle', icon: Icons.directions_bike),
    TabPage(title: 'Boat', icon: Icons.directions_boat),
  ]
),
```

### TabController

今度は「TabController」での書き方をみてみましょう。


```dart
class _MainPageState extends State<MainPage> with SingleTickerProviderStateMixin {

  TabController _tabController;

  @override
  void initState() {
    super.initState();
    _tabController = TabController(vsync: this, length: _tab.length);
  }

  @override
  void dispose() {
    _tabController.dispose();
    super.dispose();
  }

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('TabBar'),
        bottom: TabBar(
          controller: _tabController,
          tabs: _tab,
        ),
      ),
      body: TabBarView(
        controller: _tabController,
        children: <Widget> [
          TabPage(title: 'Car', icon: Icons.directions_car),
          TabPage(title: 'Bisycle', icon: Icons.directions_bike),
          TabPage(title: 'Boat', icon: Icons.directions_boat),
        ]
      ),
    );
  }
}
```

このように「TabController」を使うときは必ず``initState``メソッドで、初期化し、``dispose``メソッドで破棄を書いてください。  
こうすることで、``_MainPageState``の作成、破棄と同じタイミングで「TabController」も作成、破棄が行われるようになります。  

```dart

  TabController _tabController;

  @override
  void initState() {
    super.initState();
    _tabController = TabController(vsync: this, length: _tab.length);
  }

  @override
  void dispose() {
    _tabController.dispose();
    super.dispose();
  }
```

次に「TabBar」、「TabBarView」それぞれに「TabController」を設定することで、タブが動作するようになるので、完成です。

```dart
bottom: TabBar(
  controller: _tabController,
  tabs: _tab,
),
```

```dart
body: TabBarView(
  controller: _tabController,
  children: <Widget> [
    TabPage(title: 'Car', icon: Icons.directions_car),
    TabPage(title: 'Bisycle', icon: Icons.directions_bike),
    TabPage(title: 'Boat', icon: Icons.directions_boat),
  ]
),
```

