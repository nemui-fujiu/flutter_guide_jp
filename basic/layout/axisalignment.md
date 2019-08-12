+++
title = "AxisAlignment"
date = 2019-02-22T00:00:00+09:00
draft = false
weight = 314
description = "ColoumnやRowを使う上で必要なMainAxisAlignment & CrossAxisAlignmentで設定できる各種表示についての基本を解説していきます。"
+++

## MainAxisAlignment & CrossAxisAlignment

ColoumnやRowを使う上で必要なMainAxisAlignment & CrossAxisAlignmentで設定できる各種表示についての基本を解説していきます。  
ColumnとRow基本や、MinAxisAlignment、CrossAxisAlignmentの初歩的なつかいかたについては[Column & Row](/basic/detail03/)をご確認ください。　　

### MainAxisAlignmentのSpace

これから説明するSpaceに対する制御は行でも列でもどちらでも同じように作用します。    
今回のサンプルは、スペース部分がわかりづらいため、スペースが白く見えるようにしています。    
ご自身で試すときは``debugPaintSizeEnabled``を使って確認するのも良いでしょう。    
[debugPaintSizeEnabled](/basic/detail01/#debugPaintSizeEnabled)

#### MainAxisAlignment.spaceAround

まず一つ目は``MainAxisAlignment.spaceAround``についてです。

{{< highlight dart >}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Alignment',
      home: Center(
        child:Container(
          color: Colors.white,
          child: Row(
            mainAxisAlignment: MainAxisAlignment.spaceAround,
            children: <Widget>[
              Container( color: Colors.blue, width: 50, height:50 ),
              Container( color: Colors.red, width: 50, height:50 ),
              Container( color: Colors.green, width: 50, height:50 ),
              Container( color: Colors.orange, width: 50, height:50 ),
            ],
          ),
        ),
      ),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/layout/04/alignment_01.png" style="min-width:300px;max-width:600px;" alt="MainAxisAlignment.spaceAround"/>

並べた要素に対して均等に余白を作成し並べるために利用します。  
最初と最後の余白は自動で最適なサイズに調整されます。

#### MainAxisAlignment.spaceBetween

``MainAxisAlignment.spaceBetween``は以下のようになります。

{{< highlight dart >}}
    mainAxisAlignment: MainAxisAlignment.spaceBetween,
{{< /highlight >}}

<img src="/images/basic/layout/04/alignment_02.png" style="min-width:300px;max-width:600px;" alt="MainAxisAlignment.spaceBetween"/>

``spaceAround``と違い最初と最後のスペースがない状態で、均等に配置されます。

#### MainAxisAlignment.spaceEvenly

``MainAxisAlignment.spaceEvenly``は以下のようになります。

{{< highlight dart >}}
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
{{< /highlight >}}

<img src="/images/basic/layout/04/alignment_03.png" style="min-width:300px;max-width:600px;" alt="MainAxisAlignment.spaceEvenly"/>

``spaceAround``と似ていますが、最初と最後のスペースも各要素と同じ幅になるように均等に配置されると言う違いがあります。

### MainAxisSize

``MainAxisSize``は空いている領域をColumnまたはRowに含めるかを設定できます。

#### MainAxisSize.max

``MainAxisSize.max``は以下のようになります。

{{< highlight dart >}}
  child: Row(
    mainAxisSize: MainAxisSize.max,
    mainAxisAlignment: MainAxisAlignment.center,
    children: <Widget>[
      Container( color: Colors.blue, width: 50, height:50 ),
      Container( color: Colors.red, width: 50, height:50 ),
      Container( color: Colors.green, width: 50, height:50 ),
      Container( color: Colors.orange, width: 50, height:50 ),
    ],
  ),
{{< /highlight >}}

<img src="/images/basic/layout/04/alignment_04.png" style="min-width:300px;max-width:600px;" alt="MainAxisAlignment.spaceEvenly"/>

みてわかる通り、両端が全てRowの領域として確保された状態となります。

#### MainAxisSize.min

``MainAxisSize.min``は以下のようになります。

{{< highlight dart >}}
    mainAxisSize: MainAxisSize.min,
{{< /highlight >}}

<img src="/images/basic/layout/04/alignment_05.png" style="min-width:300px;max-width:600px;" alt="MainAxisAlignment.spaceEvenly"/>

こうすることによってRowの余分な領域を消すことができました。

#### 均等に配置　Expandedの利用

ここまでくると、あと残っているのは、要素自体を均等に配置することだと思います。  
そう言うときは``Expanded``を使います。

{{< highlight dart >}}
return MaterialApp(
  title: 'Expanded',
  home: Container(
    child: Row(
      children: <Widget>[
        Expanded(
          child:Container( color: Colors.blue)
        ),
        Expanded(
          child:Container( color: Colors.red),
        ),
        Expanded(
          child:Container( color: Colors.green),
        ),
        Expanded(
          child:Container( color: Colors.orange),
        ),
      ],
    ),
  ),
);
{{< /highlight >}}

<img src="/images/basic/layout/04/expanded_01.png" style="min-width:300px;max-width:600px;" alt="Expanded"/>

``Expanded``はColumn、RowまたはFlexの子要素として利用でき、領域を最大限確保します。  
この効果を使うことにより、要素を均等に並べることが可能になります。  

さらにflexを指定することで、要素の幅を変更できます。

{{< highlight dart >}}
    return MaterialApp(
      title: 'Expanded',
      home: Container(
        child: Row(
          children: <Widget>[
            Expanded(
                child:Container( color: Colors.blue)
            ),
            Expanded(
              flex: 2,
              child:Container( color: Colors.red),
            ),
            Expanded(
              child:Container( color: Colors.green),
            ),
          ],
        ),
      ),
    );
{{< /highlight >}}

<img src="/images/basic/layout/04/expanded_02.png" style="min-width:300px;max-width:300px;" alt="Expanded flex"/>

このように他の要素の2倍のサイズで赤い領域が確保されました。  
flexの数値を変えることで、自由に設定が可能です。  

### CrossAxisAlignmentのStretch & Baseline

CrossAxisAlignmentには他にStretchとBaselineと言う2つの要素があります。  

#### Stretch

``CrossAxisAlignment.stretch``を使うと以下のように画面いっぱいまで要素を広げることが可能になります。

{{< highlight dart >}}
  home: Container(
    child: Row(
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: <Widget>[
        Container( color: Colors.blue, width: 100, height:100 ),
        Container( color: Colors.red, width: 100, height:100 ),
      ],
    ),
  ),
{{< /highlight >}}
{{< highlight dart >}}
  home: Container(
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: <Widget>[
        Container( color: Colors.blue, width: 100, height:100 ),
        Container( color: Colors.red, width: 100, height:100 ),
      ],
    ),
  ),
{{< /highlight >}}

<img src="/images/basic/layout/04/alignment_06.png" style="min-width:300px;max-width:600px;" alt="CrossAxisAlignmentのStretch"/>

#### Baseline

``CrossAxisAlignment.baseline``使うのは文字や図形などのそれぞれ大きさが違うコンテンツを揃えて並べる時に利用します。  

文字の大きさの違う2つを並べると通常はこのようになります。

{{< highlight dart >}}
return MaterialApp(
  title: 'Column & Row',
  home: Center(
    child:Container(
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          Text(
            'Baseline',
            style: TextStyle(color:Colors.blue,fontSize: 50),
          ),
          Text(
            'Baseline',
            style: TextStyle(color:Colors.red,fontSize: 25)
          ),
        ],
      ),
    ),
  ),
);
{{< /highlight >}}
<img src="/images/basic/layout/04/alignment_07.png" style="min-width:300px;max-width:600px;" alt="CrossAxisAlignmentのBaseline適応前"/>

これを``CrossAxisAlignment.baseline``によって中央揃えにして以下のように表示することが可能です。

{{< highlight dart >}}
return MaterialApp(
  title: 'Column & Row',
  home: Center(
    child:Container(
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.baseline,
        textBaseline: TextBaseline.alphabetic,
        children: <Widget>[
          Text(
            'Baseline',
            style: TextStyle(color:Colors.blue,fontSize: 50),
          ),
          Text(
            'Baseline',
            style: TextStyle(color:Colors.red,fontSize: 25)
          ),
        ],
      ),
    ),
  ),
);
{{< /highlight >}}
<img src="/images/basic/layout/04/alignment_08.png" style="min-width:300px;max-width:600px;" alt="CrossAxisAlignmentのBaseline適応後"/>

### TextDirection

TextDirectionは水平方向に対する、順序の制御を行います。  

#### Row

{{< highlight dart >}}
return MaterialApp(
  title: 'Direction',
  home: Container(
    child: Row(
      crossAxisAlignment: CrossAxisAlignment.center,
      textDirection: TextDirection.ltr,
      children: [
        Text(
          'Direction',
          style: TextStyle(color:Colors.blue,fontSize: 30),
        ),
        Text(
          'Direction',
          style: TextStyle(color:Colors.red,fontSize: 20)
        ),
      ],
    ),
  ),
);
{{< /highlight >}}

<img src="/images/basic/layout/04/direction_01.png" style="min-width:300px;max-width:600px;" alt="Rowの時のTextDirection.ltr"/>

このままだと通常の表示と変わりません。  
この水平方向の順序を``TextDirection.rtl``で逆転します。  

{{< highlight dart >}}
return MaterialApp(
  title: 'Direction',
  home: Container(
    child: Row(
      crossAxisAlignment: CrossAxisAlignment.center,
      textDirection: TextDirection.rtl,
      children: [
        Text(
          'Direction',
          style: TextStyle(color:Colors.blue,fontSize: 30),
        ),
        Text(
          'Direction',
          style: TextStyle(color:Colors.red,fontSize: 20)
        ),
      ],
    ),
  ),
);
{{< /highlight >}}

<img src="/images/basic/layout/04/direction_02.png" style="min-width:300px;max-width:600px;" alt="Rowの時のTextDirection.rtl"/>

このように順番が逆転したのがわかると思います。  

すでにお察しかとは思いますが、ltrとrtlは以下のようなの略語です。

- TextDirection.ltr => left to right  
- TextDirection.rtl => right to left

#### Column

今度はColumn上での見た目についてみてみましょう。   

{{< highlight dart >}}
return MaterialApp(
  title: 'Direction',
  home: Container(
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      textDirection: TextDirection.ltr,
      children: [
        Text(
          'Direction',
          style: TextStyle(color:Colors.blue,fontSize: 30),
        ),
        Text(
          'Direction',
          style: TextStyle(color:Colors.red,fontSize: 20)
        ),
      ],
    ),
  ),
);
{{< /highlight >}}

<img src="/images/basic/layout/04/direction_03.png" style="min-width:300px;max-width:600px;" alt="Columnの時のTextDirection.ltr"/>

このままだと通常の表示と変わりません。  
この水平方向の順序を``TextDirection.rtl``で逆転します。

{{< highlight dart >}}
return MaterialApp(
  title: 'Direction',
  home: Container(
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      textDirection: TextDirection.rtl,
      children: [
        Text(
          'Direction',
          style: TextStyle(color:Colors.blue,fontSize: 30),
        ),
        Text(
          'Direction',
          style: TextStyle(color:Colors.red,fontSize: 20)
        ),
      ],
    ),
  ),
);
{{< /highlight >}}

<img src="/images/basic/layout/04/direction_04.png" style="min-width:300px;max-width:600px;" alt="Columnの時のTextDirection.rtl"/>

``TextDirection.rtl``を指定すると、今度は右寄せになったかと思います。  
このように水平方向への逆転をするのが、TextDirectionです。  

ちなみに``CrossAxisAlignment.start``を``CrossAxisAlignment.end``に変更すると逆の動きとなります。

### VerticalDirection

VerticalDirectionは垂直方向に対する、順序の制御を行います。 

#### Row

{{< highlight dart >}}
return MaterialApp(
  title: 'Direction',
  home: Container(
    child: Row(
      crossAxisAlignment: CrossAxisAlignment.start,
      verticalDirection: VerticalDirection.down,
      children: [
        Text(
          'Direction',
          style: TextStyle(color:Colors.blue,fontSize: 30),
        ),
        Text(
          'Direction',
          style: TextStyle(color:Colors.red,fontSize: 20)
        ),
      ],
    ),
  ),
);
{{< /highlight >}}

<img src="/images/basic/layout/04/direction_05.png" style="min-width:300px;max-width:600px;" alt="Rowの時のVerticalDirection.down"/>

このままだと通常の表示と変わりません。  
この垂直方向の順序を``VerticalDirection.up``で逆転します。  

{{< highlight dart >}}
return MaterialApp(
  title: 'Direction',
  home: Container(
    child: Row(
      crossAxisAlignment: CrossAxisAlignment.start,
      verticalDirection: VerticalDirection.up,
      children: [
        Text(
          'Direction',
          style: TextStyle(color:Colors.blue,fontSize: 30),
        ),
        Text(
          'Direction',
          style: TextStyle(color:Colors.red,fontSize: 20)
        ),
      ],
    ),
  ),
);
{{< /highlight >}}

<img src="/images/basic/layout/04/direction_06.png" style="min-width:300px;max-width:600px;" alt="Rowの時のVerticalDirection.up"/>

このように順番が逆転したのがわかると思います。  

#### Column
今度はColumn上での見た目についてみてみましょう。   

{{< highlight dart >}}
return MaterialApp(
  title: 'Direction',
  home: Container(
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.center,
      verticalDirection: VerticalDirection.down,
      children: [
        Text(
          'Direction',
          style: TextStyle(color:Colors.blue,fontSize: 30),
        ),
        Text(
          'Direction',
          style: TextStyle(color:Colors.red,fontSize: 20)
        ),
      ],
    ),
  ),
);
{{< /highlight >}}

<img src="/images/basic/layout/04/direction_07.png" style="min-width:300px;max-width:600px;" alt="Columnの時のVerticalDirection.down"/>

このままだと通常の表示と変わりません。  
この垂直方向の順序を``VerticalDirection.up``で逆転します。

{{< highlight dart >}}
return MaterialApp(
  title: 'Direction',
  home: Container(
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.center,
      verticalDirection: VerticalDirection.up,
      children: [
        Text(
          'Direction',
          style: TextStyle(color:Colors.blue,fontSize: 30),
        ),
        Text(
          'Direction',
          style: TextStyle(color:Colors.red,fontSize: 20)
        ),
      ],
    ),
  ),
);
{{< /highlight >}}

<img src="/images/basic/layout/04/direction_08.png" style="min-width:300px;max-width:600px;" alt="Columnの時のVerticalDirection.up"/>

``VerticalDirection.up``を指定すると、今度は上下が順序が反転したのがわかるかと思います。  
このように垂直方向への逆転をするのが、VerticalDirectionです。 


