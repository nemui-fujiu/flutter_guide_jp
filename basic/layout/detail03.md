+++
title = "Columns & Row"
date = 2019-02-22T00:00:00+09:00
draft = false
weight = 313
description = "Columns & Rowの基本について、MinAxisAlignment、CrossAxisAlignmentの基礎を交えどんなものがありどのように利用するのかを解説していきます。"
+++

## Column & Row

ColumnとRowはその名の通り、行と列を制御します。  
一つずつみていきましょう。

### Row

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Column & Row',
      home: Container(
        child: Center(
          child: Row(
            children: <Widget>[
              Container( color: Colors.blue, width: 100, height:100 ),
              Container( color: Colors.red, width: 100, height:100 ),
            ],
          ),
        ),
      ),
    );
  }
}
```

<img src="/images/basic/layout/03/column_row_01.png" style="min-width:300px;max-width:600px;" alt="row"/>

要素の横並びは以下のようにRowクラスに対してウィジェットのリストを持つことで実現できます。  
```dart
child: Row(
  children: <Widget>[
    Container( color: Colors.blue, width: 100, height:100 ),
    Container( color: Colors.red, width: 100, height:100 ),
  ],
),
```


### Column

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Column & Row',
      home: Container(
        child: Center(
          child: Column(
            children: <Widget>[
              Container( color: Colors.blue, width: 100, height:100 ),
              Container( color: Colors.red, width: 100, height:100 ),
            ],
          ),
        ),
      ),
    );
  }
}
```

<img src="/images/basic/layout/03/column_row_02.png" style="min-width:300px;max-width:600px;" alt="column"/>

要素の縦並びは以下のようにColumnクラスに対してウィジェットのリストを持つことで実現できます。  
```dart
child: Column(
  children: <Widget>[
    Container( color: Colors.blue, width: 100, height:100 ),
    Container( color: Colors.red, width: 100, height:100 ),
  ],
),
```

### 入れ子

ColumnとRowは入れ子構造にすることができます。

```dart
return MaterialApp(
  title: 'Column & Row',
  home: Container(
    child: Row(
      children: <Widget>[
        Column(
          children: <Widget>[
            Container( color: Colors.blue, width: 100, height:100 ),
            Container( color: Colors.red, width: 100, height:100 ),
          ],
        ),
        Column(
          children: <Widget>[
            Container( color: Colors.green, width: 100, height:100 ),
            Container( color: Colors.orange, width: 100, height:100 ),
          ],
        ),
      ],
    ),
  ),
);
```

<img src="/images/basic/layout/03/column_row_06.svg" style="min-width:300px;max-width:600px;" alt="Column Rowの入れ子"/>

このように入れ子構造を作ることによって、行列を自在に組み合わせてレイアウトが作成できます。  

### 配置

これで、行と列が使えるようになりましたが、これだけでは不十分です。  
レイアウトをしていく中で、位置調整は大事な要素です。  
ColumnとRowの位置調整で大事になる要素は``mainAxisAlignment``と``crossAxisAlignment``です。

Containerの説明でもしましたAlignmentと同じで、位置の調整が可能です。  
ですがもともと、行と列という並び順を持っているため、位置調整が少々特殊になっています。

<img src="/images/basic/layout/03/column_row_03.svg" style="min-width:300px;max-width:600px;" alt="column"/>

図で示す通り、ColumnかRowかによって``mainAxisAlignment``と``crossAxisAlignment``の位置調整の方向が変わります。  
名称を読めばわかることですが、Column,Rowが伸びる方向をmainとして、それと直角に交わる方向をcrossとして位置調整していると覚えておきましょう。  


実際にレイアウトしたものは以下になります。

#### MinAxisAlignment

ColumnとRowをそれぞれ``MainAxisAlignment.start``で並べると以下のようになります。

```dart
return MaterialApp(
  title: 'Column & Row',
  home: Container(
    child: Column(
      mainAxisAlignment: MainAxisAlignment.start,
      children: <Widget>[
        Container( color: Colors.blue, width: 100, height:100 ),
        Container( color: Colors.red, width: 100, height:100 ),
      ],
    ),
  ),
);
```
```dart
return MaterialApp(
  title: 'Column & Row',
  home: Container(
    child: Row(
      mainAxisAlignment: MainAxisAlignment.start,
      children: <Widget>[
        Container( color: Colors.blue, width: 100, height:100 ),
        Container( color: Colors.red, width: 100, height:100 ),
      ],
    ),
  ),
);
```

<img src="/images/basic/layout/03/column_row_04.png" style="min-width:300px;max-width:600px;" alt="MainAxisAlignment start"/>

MainAxisAlignmentの基本の位置調整は以下の3種類です。

||||
|---|---|---|
|start|center|end|


#### CrossAxisAlignment

ColumnとRowをそれぞれ``CrossAxisAlignment.start``で並べると以下のようになります。

```dart
return MaterialApp(
  title: 'Column & Row',
  home: Container(
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        Container( color: Colors.blue, width: 100, height:100 ),
        Container( color: Colors.red, width: 100, height:100 ),
      ],
    ),
  ),
);
```
```dart
return MaterialApp(
  title: 'Column & Row',
  home: Container(
    child: Row(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        Container( color: Colors.blue, width: 100, height:100 ),
        Container( color: Colors.red, width: 100, height:100 ),
      ],
    ),
  ),
);
```

<img src="/images/basic/layout/03/column_row_05.png" style="min-width:300px;max-width:600px;" alt="CrossAxisAlignment start"/>

CrossAxisAlignmentの基本の位置調整はMainAxisAlignmentと同じで以下の3種類です。

||||
|---|---|---|
|start|center|end|

