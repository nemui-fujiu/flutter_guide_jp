+++
title = "リスト"
date = 2019-02-07T00:00:00+09:00
draft = false
weight = 422
description = "ListViewを使ったレイアウト一覧を作成して、よく使うパターンをいくつか紹介していきます。ご自身で作りたいレイアウトを見つけて参考にしてみてください。"
+++


よくあるリスト表示のパターンをいくつか紹介していきます。


<span id="list"></span>
## リスト

リスト表示をするときは「ListView」を使います。  
一つ一つの行は「ListTile」を使うと簡単に綺麗な表示が可能です。  

<img src="/images/layout/body/list/list.png" style="min-width:300px" alt="list" />

{{< highlight dart>}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Basic List';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: ListView(
          children: listTiles
        ),
      ),
    );
  }
  
  static const List<Widget> listTiles = const <Widget>[
    ListTile(
      leading: Icon(Icons.map),
      title: Text('Map'),
    ),
    ListTile(
      leading: Icon(Icons.photo_album),
      title: Text('Album'),
    ),
    ListTile(
      leading: Icon(Icons.phone),
      title: Text('Phone'),
    ),
  ];
}
{{< /highlight >}}

<span id="separator"></span>
## 区切り線

上記「リスト」の「ListView」を「ListView.separated」に変更します。  
これにより、itemBuilderで値を受け取り「ListTile」間に「Divider」が表示されるようになります。  
ただし、一番上と、一番下には表示されません。区切り線はコンテンツの区切りにのみ表示されます。

<img src="/images/layout/body/list/list_separator.png" style="min-width:300px" alt="separator" />



{{< highlight dart>}}
return MaterialApp(
  title: title,
  home: Scaffold(
    appBar: AppBar(
      title: Text(title),
    ),
    body: ListView.separated(
      itemCount: listTiles.length,
      separatorBuilder: (BuildContext context, int index) => Divider(color: Colors.black,),
      itemBuilder: (BuildContext context, int index) {
        return listTiles[index];
      },
    ),
  ),
);
{{< /highlight >}}

<span id="list_bottom_separator"></span>
## リストの最後にも区切り線

区切り線を表示するけどどうしても、最後にも区切り線が出したい人ように別のパターンを紹介します。  
上記「リスト」の「listTile」を編集して「Container」で囲いこみ、底辺に線が表示されるようにしています。

<img src="/images/layout/body/list/list_bottom_separator.png" style="min-width:300px" alt="bottom separator" />

{{< highlight dart>}}
  List<Widget> listTiles = <Widget>[
    Container(
      decoration: new BoxDecoration(
        border: new Border(
          bottom: new BorderSide(color: Colors.black),
        ),
      ),
      child: ListTile(
          leading: Icon(Icons.map),
          title: Text('Map'),
      )
    ),
    Container(
        decoration: new BoxDecoration(
          border: new Border(
            bottom: new BorderSide(color: Colors.black),
          ),
        ),
        child: ListTile(
          leading: Icon(Icons.photo_album),
          title: Text('Album'),
        )
    ),
    Container(
        decoration: new BoxDecoration(
          border: new Border(
            bottom: new BorderSide(color: Colors.black),
          ),
        ),
        child: ListTile(
          leading: Icon(Icons.phone),
          title: Text('Phone'),
        )
    ),
  ];
{{< /highlight >}}

<span id="header_list"></span>
## ヘッダー付きのリスト

連絡先一覧やカテゴリなど一つの画面で複数のリストを表示したい場合はヘッダーが欲しくなることってありますよね？  
そんなときはこんな感じに表示するのはいかがでしょうか。  
上記「リスト」の「listTile」を編集して「Container」で囲いこみ、Header行のみStyleを変更して表示する事で、ヘッダーを表現しています。  

<img src="/images/layout/body/list/header_list.png" style="min-width:300px" alt="header list" />


{{< highlight dart>}}
  List<Widget> listTiles = <Widget>[
    Container(
      child: ListTile(
        title: Text(
          "Activite",
          style: TextStyle(fontSize:25, fontWeight: FontWeight.bold),
        ),
      ),
      decoration: BoxDecoration(
        color: Colors.green,
      ),
    ),
    Container(
      child: ListTile(
        leading: Icon(Icons.directions_bike),
        title: Text('Bike'),
      )
    ),
    Container(
      child: ListTile(
        title: Text(
          "Settings",
          style: TextStyle(fontSize:25, fontWeight: FontWeight.bold),
        ),
      ),
      decoration: BoxDecoration(
        color: Colors.green,
      ),
    ),
    Container(
      child: ListTile(
        leading: Icon(Icons.map),
        title: Text('Map'),
      )
    ),
    Container(
      child: ListTile(
        leading: Icon(Icons.photo_album),
        title: Text('Album'),
      )
    ),
    Container(
      child: ListTile(
        leading: Icon(Icons.phone),
        title: Text('Phone'),
      )
    ),
  ];
{{< /highlight >}}

---

## 参考

[ListView](https://docs.flutter.io/flutter/widgets/ListView-class.html)   
[ListTile](https://docs.flutter.io/flutter/material/ListTile-class.html)   
[Basic List](https://flutter.io/docs/cookbook/lists/basic-list)   
