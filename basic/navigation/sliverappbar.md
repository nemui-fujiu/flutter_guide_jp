+++
title = "SliverAppBar"
date = 2019-03-09T00:00:00+09:00
draft = false
weight = 354
description = "「SliverAppBar」はスクロールに応じてヘッダー要素を隠すことができるようになるウィジェットです。SliverFixedExtentListとSliverGrid両方の説明を行います。"
+++
## SliverAppBar

「SliverAppBar」はスクロールに応じてヘッダー要素を隠すことができるようになるウィジェットです。  
「SliverAppBar」の一覧要素には、大きく分けてリストとグリッドの2種類あるのでそれぞれ説明していきます。   
最初に「SliverFixedExtentList」についての解説をしていきます。

### SliverFixedExtentList

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('SliverAppBar'),
      ),
      body: CustomScrollView(
        slivers: <Widget>[
          const SliverAppBar(
            floating: true,
            pinned: true,
            snap: true,
            expandedHeight: 250.0,
            flexibleSpace: FlexibleSpaceBar(
              title: Text('Demo'),
            ),
          ),
          SliverFixedExtentList(
            itemExtent: 200.0,
            delegate: SliverChildBuilderDelegate(
                  (BuildContext context, int index) {
                return Container(
                  alignment: Alignment.center,
                  color: Colors.lightBlue[100 * (index % 9)],
                  child: Text('list item $index', style: TextStyle(fontSize: 30),),
                );
              },
            ),
          ),
        ],
      )
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/navigation/04/sliverappbar_01.gif" style="min-width:300px;max-width:600px;" alt="SliverAppBar SliverFixedExtentList"/>

- ``floating``がtrueの時は、リストの最初に戻らなくても上にスクロールするとヘッダーが表示されるようになります。
- ``pinned``がtrueの時は、ヘッダーを完全に隠すのではなく、タイトルの1行分を残した状態で表示されるようになります。
- ``snap``がtrueの時は、ヘッダーがスクロールにより中途半端に表示されなくなり、一気に最大表示されます。  
※``snap``をtrueにしたい時は必ず``floating``がtrueである必要があります。   
- ``expandedHeight``は表示するヘッダーの高さになります。


「SliverAppBar」はこのように「CustomScrollView」の子要素として作成します。  
「CustomScrollView」のスクロール動作に対して、「SliverAppBar」のヘッダー要素が隠れるようになります。

{{< highlight dart>}}
body: CustomScrollView(
  slivers: <Widget>[
    const SliverAppBar(
    ),
  ]
),
{{< /highlight >}}

次に表示するリストを作成します。  
``itemExtent``が行の高さになり、リストの内容は``delegate``にて行になる要素を作成してください。  
今回は「SliverChildBuilderDelegate」を使うことで、表示している範囲のみ要素を生成するようにします。  
``delegate``にはスクロール時に現在の行番号(``index``)を受け取ることができます。  

{{< highlight dart>}}
SliverFixedExtentList(
  itemExtent: 200.0,
  delegate: SliverChildBuilderDelegate(
        (BuildContext context, int index) {
      return Container(
        alignment: Alignment.center,
        color: Colors.lightBlue[100 * (index % 9)],
        child: Text('list item $index', style: TextStyle(fontSize: 30),),
      );
    },
  ),
),
{{< /highlight >}}

「SliverChildBuilderDelegate」は``childCount``を指定することで、表示件数を制限することも可能です。  
``childCount``を指定しない場合は、スクロールに応じて無限にコンテンツを構築しようとするので、動的に内容を制御している場合は、表示したいコンテンツが足りなくなる可能性があるので注意してください。

{{< highlight dart>}}
delegate: SliverChildBuilderDelegate(
    (BuildContext context, int index) {
    return Container(
      alignment: Alignment.center,
      color: Colors.lightBlue[100 * (index % 9)],
      child: Text('list item $index', style: TextStyle(fontSize: 30),),
    );
  },
  childCount: 10,
),
{{< /highlight >}}

これにより、ヘッダーがスクロールで表示、非表示される「SliverAppBar」が完成です。


### SliverGrid

次に「SliverGrid」による、グリッド一覧の作り方もみてみましょう。

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('SliverAppBar'),
      ),
      body: CustomScrollView(
        slivers: <Widget>[
          const SliverAppBar(
            floating: true,
            pinned: true,
            snap: true,
            expandedHeight: 250.0,
            flexibleSpace: FlexibleSpaceBar(
              title: Text('Demo'),
            ),
          ),
          SliverGrid(
            gridDelegate: SliverGridDelegateWithMaxCrossAxisExtent(
              maxCrossAxisExtent: 200.0,
              mainAxisSpacing: 10.0,
              crossAxisSpacing: 10.0,
              childAspectRatio: 4.0,
            ),
            delegate: SliverChildBuilderDelegate(
                  (BuildContext context, int index) {
                return Container(
                  alignment: Alignment.center,
                  color: Colors.teal[100 * (index % 9)],
                  child: Text('grid item $index'),
                );
              },
              childCount: 100,
            ),
          ),
        ],
      )
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/navigation/04/sliverappbar_02.gif" style="min-width:300px;max-width:600px;" alt="SliverAppBar SliverGrid"/>

基本は同じですが、グリッドの場合は「SliverGrid」を使います。  
「SliverGrid」では、``gridDelegate``によって、グリッドの調整を行います。   
``gridDelegate``には以下2つの方法で、グリッドを制御できます。  

- ``SliverGridDelegateWithMaxCrossAxisExtent``は``CrossAxis``方向に表示する数を画面のサイズから判定します。  
- ``SliverGridDelegateWithFixedCrossAxisCount``は``CrossAxis``方向に表示する数を固定で指定します。  


GridViewの内容と似かよっているので、詳細については、[こちら](/basic/layout/gridview/)を参照してください。  
MainAxisやCrossAxisについては、[こちら](/basic/layout/gridview/#axis_spacing)が参考になるかと思います。
