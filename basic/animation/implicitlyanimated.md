+++
title = "アニメーション(ImplicitlyAnimatedWidget)"
date = 2019-03-20T00:00:00+09:00
draft = false
weight = 372
description = "今回はアニメーション(ImplicitlyAnimatedWidget)について解説していきます。 アニメーションを作成する方法として、AnimatedAlignなど色々と簡単に作成できる方法をご紹介します。"
+++

## アニメーション(ImplicitlyAnimatedWidget)

今回はアニメーション(ImplicitlyAnimatedWidget)について解説していきます。  
アニメーションを作成する方法として、[AnimatedContainer](/basic/animation/animatedcontainer)を使った方法もあるのですが、他にも色々と簡単に作成できる方法が提供されています。  
AnimatedContainerの解説は[こちら](/basic/animation/animatedcontainer)   

ImplicitlyAnimatedWidgetクラスを継承しているとAnimateControllerなどのような複雑な制御をせずにアニメーションを作成できます。  

- [AnimatedAlign](#animated_align)
- [AnimatedPadding](#animated_padding)
- [AnimatedSize](#animated_size)
- [AnimatedDefaultTextStyle](#animated_default_text_style)
- [AnimatedOpacity](#animated_opacity)
- [AnimatedPhysicalModel](#animated_physical_model)
- [AnimatedPositioned](#animated_positioned)
- [AnimatedPositionedDirectional](#animated_positioned_directional)

<span id="animated_align"></span>
## 配置の変更(AnimatedAlign)

「AnimatedAlign」を使うことで子要素の配置を変更してアニメーションさせることができます。  
[AnimatedContainer](/basic/animation/animatedcontainer#alignment)のAlignmentでも同じ動きを作ることができます。　

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  Alignment _alg = Alignment.topLeft;

  void _onTap() => setState(() => _alg = Alignment.bottomRight);

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedAlign'),
      ),
      body: AnimatedAlign(
        alignment: _alg,
        duration: Duration(seconds: 1),
        child: Container(
          width: 100,
          height: 100,
          color: Colors.blueAccent,
        ),
      ),
      floatingActionButton: FloatingActionButton(onPressed: _onTap),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/animation/02/animated_align.gif" style="min-width:300px;max-width:600px;" alt="AnimatedAlign"/>

AnimatedAlignは``alignment``で子要素の配置を変更します。   
サンプルは、``topLeft``の位置から、``bottomRight``の位置へ斜めに移動しているのがわかると思います。   
この時の、アニメーションの動き(Curve)や、時間(Duration)については[AnimatedContainer](/basic/animation/animatedcontainer#duration)で説明していますので、そちらを参照してください。

<span id="animated_padding"></span>
## AnimatedPadding

「AnimatedPadding」を使うことでPaddingを変更してアニメーションさせることができます。  
[AnimatedContainer](/basic/animation/animatedcontainer#padding)のPaddingでも同じ動きを作ることができます。　

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  double _padding = 50;

  void _onTap() => setState(() => _padding = 0);

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedPadding'),
      ),
      body: Center(
        child: Container(
          color: Colors.orange,
          child: AnimatedPadding(
            padding: EdgeInsets.all(_padding),
            duration: Duration(seconds: 1),
            child: Container(
              width: 100,
              height: 100,
              color: Colors.blueAccent,
            ),
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(onPressed: _onTap),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/animation/02/animated_padding.gif" style="min-width:300px;max-width:600px;" alt="AnimatedPadding"/>

AnimatedPaddingは``padding``で余白を変更します。    
サンプルは、Paddingを``50``から、``0``に変更しているので、オレンジ色の部分がなくなったのがわかると思います。   

<span id="animated_size"></span>
## AnimatedSize

「AnimatedSize」を使うことでサイズの変更してアニメーションさせることができます。    
[AnimatedContainer](/basic/animation/animatedcontainer#size)のサイズ調整でも同じ動きを作ることができます。　   
画像などを子要素にすることもできるので、クリックすると画像が拡大されるようなアニメーションを作ることも可能です。  

{{< highlight dart>}}
class _MainPageState extends State<MainPage> with SingleTickerProviderStateMixin {

  double _width = 50;

  void _onTap() => setState(() => _width = 100);

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedSize'),
      ),
      body: Center(
        child: AnimatedSize(
          vsync: this,
          duration: Duration(seconds: 1),
          child: Container(
            width: _width,
            height: 100,
            color: Colors.blueAccent,
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(onPressed: _onTap),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/animation/02/animated_size.gif" style="min-width:300px;max-width:600px;" alt="AnimatedSize"/>

AnimatedSizeは``width``または``height``の値を変更してアニメーションさせます。   
サンプルは、``width``を``50``から、``100``に変更しているので、横に大きくなり正方形に変わりました。    

<span id="animated_default_text_style"></span>
## AnimatedDefaultTextStyle

「AnimatedDefaultTextStyle」を使うことでテキスト要素のアニメーションさせることができます。   
テキストを目立たせたり動きのあるメッセージを作ることが可能です。   

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  TextStyle _style = TextStyle(color: Colors.blueAccent, fontSize: 30, fontWeight: FontWeight.w900);

  void _onTap() => setState(() => _style = TextStyle(color: Colors.black, fontSize: 20, fontWeight: FontWeight.w300));

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedDefaultTextStyle'),
      ),
      body: Center(
        child: AnimatedDefaultTextStyle(
            style: _style,
            duration: Duration(seconds: 1),
            child:Text('AnimatedDefaultTextStyle')
        ),
      ),
      floatingActionButton: FloatingActionButton(onPressed: _onTap),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/animation/02/animated_default_text_style.gif" style="min-width:300px;max-width:600px;" alt="AnimatedDefaultTextStyle"/>

AnimatedDefaultTextStyleは``style``の値を変更してアニメーションさせます。   
サンプルは、``style``の「TextStyle」クラスの設定を変更することで、フォントのサイズや色、太さなどが変わっています。   

<span id="animated_opacity"></span>
## AnimatedOpacity

「AnimatedOpacity」を使うことで色の濃さに対してのアニメーションさせることができます。   
色の濃さをアニメーションすることで、フィードアウトやフィードインのようなアニメーションが簡単に作れます。  

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  double _opacity = 1.0;

  void _onTap() => setState(() => _opacity = 0.5);

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedOpacity'),
      ),
      body: Center(
        child: AnimatedOpacity(
          opacity: _opacity,
          duration: Duration(seconds: 1),
          child: Container(
            width: 100,
            height: 100,
            color: Colors.blueAccent,
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(onPressed: _onTap),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/animation/02/animated_opacity.gif" style="min-width:300px;max-width:600px;" alt="AnimatedOpacity"/>

AnimatedOpacityは``opacity``の値を変更してアニメーションさせます。    
サンプルは、``opacity``を``1.0``から``0.5``に変更することで、徐々に薄くなっていくアニメーションになっています。   

<span id="animated_physical_model"></span>
## AnimatedPhysicalModel

「AnimatedPhysicalModel」を使うことで影の濃さなどに対してのアニメーションさせることができます。   
モーダルやボタンが浮き上がったり、沈み込むようなアニメーションを作ることができます。  

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  bool _isDisabled = true;

  void _onTap() => setState(() => _isDisabled = false);

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedPhysicalModel'),
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(40),
          child: AnimatedPhysicalModel(
            duration: Duration(seconds: 1),
            child: Container(
              width: 100,
              height: 100
            ),
            borderRadius: _isDisabled ? BorderRadius.all(Radius.zero) : BorderRadius.all(Radius.circular(10.0)),
            elevation: _isDisabled ? 10 : 20,
            color: _isDisabled ? Colors.orange : Colors.deepOrange,
            animateColor: true,
            shape: BoxShape.rectangle,
            shadowColor: Colors.black,
            curve: Curves.fastOutSlowIn,
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(onPressed: _onTap),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/animation/02/animated_physical_model.gif" style="min-width:300px;max-width:600px;" alt="AnimatedPhysicalModel"/>

AnimatedPhysicalModelは`` borderRadius``と``elevation``をアニメーションさせることができます。    
``shape``はアニメーションしません。  
``animateColor``を``true``にすることで、色もアニメーションさせることができます。  
サンプルは、`` borderRadius``と``elevation``、``color``を変更することで、浮き上がって色が代わり角が取れるアニメーションになっています。   

<span id="animated_positioned"></span>
## AnimatedPositioned

「AnimatedPositioned」を使うことで位置を変化をアニメーションさせることができます。   
「AnimatedAlign」との違いは、四辺からの距離の指定で細かく移動させることができる点です。  

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  double _top = 0;

  void _onTap() => setState(() => _top = 100);

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedPositioned'),
      ),
      body: Center(
        child: Stack(
          children: <Widget>[
            AnimatedPositioned(
              top: _top,
              left: 0,
              duration: Duration(seconds: 1),
              child: Container(
                width: 100,
                height: 100,
                color: Colors.blueAccent,
              ),
            ),
          ]
        )
      ),
      floatingActionButton: FloatingActionButton(onPressed: _onTap),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/animation/02/animated_positioned.gif" style="min-width:300px;max-width:600px;" alt="AnimatedPositioned"/>

AnimatedPositionedは以下の値を変更してアニメーションさせます。  

- ``top`` 上辺からの距離
- ``bottom``　下辺からの距離
- ``left``　左辺からの距離
- ``right``　右辺からの距離

必ずStackの子要素である必要があります。  
サンプルは、``top``を``0``から``100``に変更することで、上から下へ移動するアニメーションになっています。  

<span id="animated_positioned_directional"></span>
## AnimatedPositionedDirectional

「AnimatedPositionedDirectional」を使うことで位置を変化をアニメーションさせることができます。   
「AnimatedPositioned」との違いは、ほとんどありません。   
違いとしては、横方向への動きが``left``、``right``だったものが、``start``、``end``に変わっています。    
これは、多言語化意識したときに特に有用な要素です。   
例えばアラビア語のように文字の読む方向が、右から左になっているので、通常とは逆に``start``は右、``end``は左になります。   
このように国によって右から左、左から右など見る順が違うので、Directionalityの設定で、方向を容易に制御できるようになります。   

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  double _start = 0;

  void _onTap() => setState(() => _start = 100);

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedPositionedDirectional'),
      ),
      body: Center(
        child: Stack(
          children: <Widget>[
            Directionality(
              textDirection: TextDirection.rtl,
              child: AnimatedPositionedDirectional(
                top: 0,
                start: _start,
                duration: Duration(seconds: 1),
                child: Container(
                  width: 100,
                  height: 100,
                  color: Colors.blueAccent,
                ),
              ),
            )
          ]
        )
      ),
      floatingActionButton: FloatingActionButton(onPressed: _onTap),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/animation/02/animated_positioned_directional.gif" style="min-width:300px;max-width:600px;" alt="AnimatedPositionedDirectional"/>

AnimatedPositionedDirectionalは以下の値を変更してアニメーションさせます。  

- ``top`` 上辺からの距離
- ``bottom``　下辺からの距離
- ``start``　開始位置からの距離
- ``end``　終了位置からの距離

必ずStackの子要素である必要があります。  
サンプルは、``start``を``0``から``100``に変更することで、開始位置から、終了位置へ移動するアニメーションになっています。  
サンプルは、``Directionality.rtl``(右から左)となっているので、右を``0``として左側へ``100``移動しています。
