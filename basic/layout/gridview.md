+++
title = "GridView"
date = 2019-02-24T00:00:00+09:00
draft = false
weight = 316
description = "GridViewは写真などのボックス状のレイアウトを配置しスクロールするのに便利なウィジェットです。 "
+++

## GridView

GridViewは写真などのボックス状のレイアウトを配置しスクロールするのに便利なウィジェットです。  

それでは細かいところを一つずつみていきましょう。

GridViewを構築するためには4つの方法があります。

1. 「GridView.count」は最も一般的に使用されるグリッドレイアウトとなり、横に並べる数を固定数で指定してグリッドを表示します。
2. 「GridView.extent」は横に並べる幅を指定値分最大限確保してグリッドを並べる表示に方法になります。
3. 「GridView.builder」で作成すると、実際に表示されている子要素のみビルダーが呼び出されるため随時読み込みなど、多数(または無限)のリストを表示する場合に利用する作成方法です。
4. 「GridView.custom」で作成すると、「SliverGridDelegate」を追加してカスタマイズでき、整列されていないまたは重ねた配置を実現できます。

今回は、1~3について説明していきます。

### GridView.count

一般的なGridViewの表示方法です。  

{{< highlight dart >}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var list = [
      _photoItem("pic0"),
      _photoItem("pic1"),
      _photoItem("pic2"),
      _photoItem("pic3"),
      _photoItem("pic4"),
      _photoItem("pic5"),
    ];
    return MaterialApp(
        home: Scaffold(
            appBar: AppBar(
              title: Text('GridView'),
            ),
            body: GridView.count(
                crossAxisCount: 2,
                children: list
            )
        )
    );
  }

  Widget _photoItem(String image) {
    var assetsImage = "assets/img/" + image + ".png";
    return Container(
      child: Image.asset(assetsImage, fit: BoxFit.cover,),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/layout/06/gridview_01.png" style="min-width:300px;max-width:600px;" alt="GridView.count"/>

このように``crossAxisCount``で横に並べる件数を指定することでグリッドが並びます。

### GridView.extent

``GridView.extent``は``maxCrossAxisExtent``で指定した幅を最大値として、グリッドを並べる表示方法です。  
例えば、500pxの横幅のある「GridView」に対して、``maxCrossAxisExtent``を150と指定した婆、幅125pxのグリッドが4列並んだ表示となります。  


{{< highlight dart >}}
body: GridView.extent(
    maxCrossAxisExtent: 150,
    padding: const EdgeInsets.all(4),
    mainAxisSpacing: 4,
    crossAxisSpacing: 4,
    children: list
)
{{< /highlight >}}

<img src="/images/basic/layout/06/gridview_02.png" style="min-width:300px;max-width:600px;" alt="GridView.extent"/>

``padding``で「GridView」の外周のPaddingを設定し、``mainAxisSpacing``、``crossAxisSpacing``によりグリッド間にスペースを作っています。  
今回のGridViewは幅400pxのため、グリッドの間の数値を引いて128pxのグリッドが3列になっている状態です。  

### GridView.builder

``GridView.builder``は「ListView」の時同様、表示する要素が事前にわからない場合に利用する書き方です。  
``itemBuilder``は画面表示時に実行されるため、無限にグリッドを作成することが可能です。  
ギャラリー、検索結果の表示などに利用すると良いかと思います。 

{{< highlight dart >}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var grid = ["pic0", "pic1", "pic2", "pic3", "pic4", "pic5",];
    return MaterialApp(
        home: Scaffold(
            appBar: AppBar(
              title: Text('GridView'),
            ),
            body: GridView.builder(
                gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                  crossAxisCount: 2,
                ),
                itemBuilder: (BuildContext context, int index) {
                  if (index >= grid.length) {
                    grid.addAll(["pic0", "pic1", "pic2", "pic3", "pic4", "pic5",]);
                  }
                  return _photoItem(grid[index]);
                }
            )
        )
    );
  }

  Widget _photoItem(String image) {
    var assetsImage = "assets/img/" + image + ".png";
    return Container(
      child: Image.asset(assetsImage, fit: BoxFit.cover,),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/layout/06/gridview_03.png" style="min-width:300px;max-width:600px;" alt="GridView.builder"/>

上記の例では``itemBuilder``で画像を表示するウィジェットを作成しています。  
また、gridの件数が表示件数を上回る場合に追加で画像を生成しています。  
このようにすることで無限にグリッドが生成される状態になりました。

### SliverGridDelegateWithFixedCrossAxisCount

``builder``で利用している、``SliverGridDelegateWithFixedCrossAxisCount``は``GridView.count``と同じです。  
``crossAxisCount``で横に並べる数を指定して、グリッドを作成します。

{{< highlight dart >}}
body: GridView.builder(
    gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
      crossAxisCount: 2,
    ),
    itemBuilder: (BuildContext context, int index) {
      if (index >= grid.length) {
        grid.addAll(["pic0", "pic1", "pic2", "pic3", "pic4", "pic5",]);
      }
      return _photoItem(grid[index]);
    }
)
{{< /highlight >}}

### SliverGridDelegateWithMaxCrossAxisExtent

``builder``では、``SliverGridDelegateWithMaxCrossAxisExtent``も利用でき、``GridView.extent``と同じ制御が可能です。    
``maxCrossAxisExtent``で横に並べるグリッドの最大幅を指定して、グリッドを作成します。

{{< highlight dart >}}
body: GridView.builder(
  gridDelegate: SliverGridDelegateWithMaxCrossAxisExtent(
    maxCrossAxisExtent: 150,
  ),
  itemBuilder: (BuildContext context, int index) {
    if (index >= grid.length) {
      grid.addAll(["pic0", "pic1", "pic2", "pic3", "pic4", "pic5",]);
    }
    return _photoItem(grid[index]);
  }
)
{{< /highlight >}}

### ScrollDirection

「GridView」クラスであればどれでも利用できるのですが、``scrollDirection``はグリッドを並べる方向を変更するために使います。  
デフォルトでは``Axis.vertical``です。

{{< highlight dart >}}
body: GridView.builder(
  scrollDirection: Axis.horizontal,
  gridDelegate: SliverGridDelegateWithMaxCrossAxisExtent(
    maxCrossAxisExtent: 150,
  ),
  itemBuilder: (BuildContext context, int index) {
    if (index >= grid.length) {
      grid.addAll(["pic0", "pic1", "pic2", "pic3", "pic4", "pic5",]);
    }
    return _photoItem(grid[index]);
  }
)
{{< /highlight >}}

<img src="/images/basic/layout/06/gridview_04.gif" style="min-width:300px;max-width:600px;" alt="GridView scrollDirection"/>

このようにスクロールする方向が変わったのがわかると思います。

<span id="axis_spacing"></span>
### AxisSpacing

グリッド同士の間にスペースを作成したいときに指定するのが、``mainAxisSpacing``と``crossAxisSpacing``になります。  
RowやColumnでも同じ考え方が出てきましたが、以下のようなイメージで利用します。

このプロパティは、「GridView」クラスであれば上記で説明したどれでも利用できます。

<img src="/images/basic/layout/06/gridview_05.svg" style="width:300px;" alt="GridView　AxisSpacing"/>

このように並ぶ方向に対して、``mainAxisSpacing``と``crossAxisSpacing``が決まることに注意してください。  
特に、先に説明した``scrollDirection``と併用する場合はスペース位置が変わりますのでお気をつけください。  

{{< highlight dart >}}
body: GridView.builder(
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 3,
    mainAxisSpacing: 4,
  ),
  itemBuilder: (BuildContext context, int index) {
    if (index >= grid.length) {
      grid.addAll(["pic0", "pic1", "pic2", "pic3", "pic4", "pic5",]);
    }
    return _photoItem(grid[index]);
  }
)
{{< /highlight >}}

<img src="/images/basic/layout/06/gridview_06.png" style="min-width:300px;max-width:600px;" alt="GridView mainAxisSpacing"/>

このように上下に対してスペースができます。

{{< highlight dart >}}
body: GridView.builder(
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 3,
    crossAxisSpacing: 4,
  ),
  itemBuilder: (BuildContext context, int index) {
    if (index >= grid.length) {
      grid.addAll(["pic0", "pic1", "pic2", "pic3", "pic4", "pic5",]);
    }
    return _photoItem(grid[index]);
  }
)
{{< /highlight >}}

<img src="/images/basic/layout/06/gridview_07.png" style="min-width:300px;max-width:600px;" alt="GridView crossAxisSpacing"/>

このように左右に対してスペースができます。

### ChildAspectRatio

今まで紹介したやり方だとGridのタテヨコ比は常に同じ割合となります。  
そのため縦の長さを伸ばしたい場合は、``childAspectRatio``で比率を変えてあげます。  
このプロパティは、「GridView」クラスであれば上記で説明したどれでも利用できます。

{{< highlight dart >}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var list = [
      _photoItem("pic0"),
      _photoItem("pic1"),
      _photoItem("pic2"),
      _photoItem("pic3"),
      _photoItem("pic4"),
      _photoItem("pic5"),
    ];
    return MaterialApp(
        home: Scaffold(
            appBar: AppBar(
              title: Text('GridView'),
            ),
            body: GridView.count(
                crossAxisCount: 2,
                childAspectRatio: 0.7,
                children: list
            )
        )
    );
  }

  Widget _photoItem(String image) {
    var assetsImage = "assets/img/" + image + ".png";
    return Container(
      child: Image.asset(assetsImage, fit: BoxFit.cover,),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/layout/06/gridview_08.png" style="min-width:300px;max-width:600px;" alt="GridView ChildAspectRatio"/>

## 参考

[GridView](https://docs.flutter.io/flutter/widgets/GridView-class.html)
