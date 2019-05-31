+++
title = "グリッド"
date = 2019-02-07T00:00:00+09:00
draft = false
weight = 423
description = "GridViewを使ったレイアウト一覧を作成して、よく使うパターンをいくつか紹介していきます。ご自身で作りたいレイアウトを見つけて参考にしてみてください。"
+++

よくあるグリッド表示のパターンをいくつか紹介していきます。

<span id="grid"></span>
## グリッド

GridViewレイアウトはcrossAxisCountで並べる数を制御して、グリッドを作成します。

<img src="/images/layout/body/grid/grid.png" style="min-width:300px" alt="grid" />

{{< highlight dart>}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      title: 'Grid List',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Grid List'),
        ),
        body: GridView.count(
          crossAxisCount: 3,
          children: List.generate(100, (index) {
            return Center(
              child: Text(
                'Item $index',
                style: Theme.of(context).textTheme.headline,
              ),
            );
          }),
        ),
      ),
    );
  }
}
{{< /highlight >}}

<span id="grid_icon"></span>
## グリッドアイコンカード

今度はGridを調整して、正方形のカードを並べてみました。  
「GridView.count」で一行に表示する数や、縦横のスペースを制御しています。  
「Container」で「GridTile」の中の配置を調整しています。

<img src="/images/layout/body/grid/grid_icon.png" style="min-width:300px" alt="grid icon" />

{{< highlight dart>}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      title: 'Grid List',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Grid List'),
        ),
        body: GridView.count(
          crossAxisCount: 3, // 1行に表示する数
          crossAxisSpacing: 4.0, // 縦スペース
          mainAxisSpacing: 4.0, // 横スペース
          shrinkWrap: true,
          children: List.generate(100, (index) {
            return Container(
              padding: const EdgeInsets.all(8.0),
              alignment: Alignment.center,
              decoration: BoxDecoration(
                color: Colors.green,
              ),
              child:GridTile(
                child: Icon(Icons.map),
                footer: Center(
                  child: Text(
                    'Meeage $index',
                  ),
                )
              )
            );
          }),
        ),
      ),
    );
  }
}
{{< /highlight >}}

ちなみに「GridView.count」だと常に正方形のグリッドが作られますが高さの変更をしたいときはAspectをいじる事で可能です。
{{< highlight dart>}}
GridView.count(
 childAspectRatio: 0.7, // 高さ
)
{{< /highlight >}}

<span id="grid_image"></span>
## 画像グリッドリスト

画像を並べるグリッドレイアウトって多いですよね？  
以下のような形で、画像を並べて本文を載せるレイアウトにすると1つ1つのコンテンツが目立つレイアウトを作ることができます。  

グリッドの1塊を「GridTile」ではなく「Column」で作成する事で自由なレイアウトにしています。  

<img src="/images/layout/body/grid/grid_images.png" style="min-width:300px" alt="grid images" />

{{< highlight dart>}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      title: 'Grid List',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Grid List'),
        ),
        body: GridView.count(
          padding: EdgeInsets.all(4.0),
          crossAxisCount: 2,
          crossAxisSpacing: 10.0, // 縦
          mainAxisSpacing: 10.0, // 横
          childAspectRatio: 0.7, // 高さ
          shrinkWrap: true,
          children: List.generate(100, (index) {

            var rng = new Random();
            var imgNumber = rng.nextInt(3);
            var assetsImage = "assets/grid/m" + imgNumber.toString() + ".png";

            return Container(
              alignment: Alignment.center,
              decoration: BoxDecoration(
                color: Colors.white,
                boxShadow: [
                  new BoxShadow(
                    color: Colors.grey,
                    offset: new Offset(5.0, 5.0),
                    blurRadius: 10.0,
                  )
                ],
              ),
              child:Column(
                children: <Widget>[
                  Image.asset(assetsImage, fit: BoxFit.cover,),
                  Container(
                    margin: EdgeInsets.all(16.0),
                    child: Text(
                      'Meeage $index',
                    ),
                  ),
                ]
              ),
            );
          }),
        ),
      ),
    );
  }
}
{{< /highlight >}}


本来はちゃんと画像を取得するべきですが、今回は横着して3枚の画像をランダムで出しています。  
「Random」クラスはDartのmathパッケージにあるので事前にimportしてください。  

{{< highlight dart>}}
import 'dart:math';
{{< /highlight >}}

{{< highlight dart>}}
    var rng = new Random();
    var imgNumber = rng.nextInt(3);
    var assetsImage = "assets/grid/m" + imgNumber.toString() + ".png";
{{< /highlight >}}

## 参考

[GridView](https://docs.flutter.io/flutter/widgets/GridView-class.html)  
[Grid List](https://flutter.io/docs/cookbook/lists/grid-lists)  
