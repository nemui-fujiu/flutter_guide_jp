+++
title = "アニメーション(その他)"
date = 2019-03-15T00:00:00+09:00
draft = true
weight = 373
description = "今回はImplicitlyAnimatedWidget継承以外のアニメーションについて解説していきます。アニメーションを作成する方法として、AnimatedCrossFadeやAnimatedModalBarrierなど色々と簡単に作成できる方法をご紹介します。"
+++

## アニメーション(その他)

今回はアニメーション(ImplicitlyAnimatedWidget継承以外)について解説していきます。  
アニメーションを作成する方法として、[AnimatedContainer](/basic/animation/animatedcontainer)や[ImplicitlyAnimatedWidget](/basic/animation/implicitlyanimated)を使った方法もあるのですが、それ以外の方法を紹介します。  
AnimatedContainerの解説は[こちら](/basic/animation/animatedcontainer)   
ImplicitlyAnimatedWidgetの解説は[こちら](/basic/animation/implicitlyanimated)   

- [AnimatedCrossFade](#animated_cross_fade)
- [AnimatedModalBarrier](#animated_modal_barrier)
- [AnimatedSwitcher](#animated_switcher)
- [AnimatedDefaultTextStyle](#animated_default_text_style)
- [AnimatedOpacity](#animated_opacity)
- [AnimatedPhysicalModel](#animated_physical_model)
- [AnimatedPositioned](#animated_positioned)
- [AnimatedPositionedDirectional](#animated_positioned_directional)

<span id="animated_cross_fade"></span>
## AnimatedCrossFade

画像の切り替え

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  CrossFadeState _crossFadeState = CrossFadeState.showFirst;

  void _onTap() => setState(() => _crossFadeState = CrossFadeState.showSecond);

  Widget build(BuildContext context) {
    var first =  Image.asset("assets/img/pic0.png", fit: BoxFit.cover, width: 200, height: 200,);
    var second = Image.asset("assets/img/pic1.png", fit: BoxFit.cover, width: 200, height: 200,);
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedCrossFade'),
      ),
      body: Center(
        child: AnimatedCrossFade(
          firstChild: first,
          secondChild: second,
          duration: Duration(seconds: 1),
          crossFadeState: _crossFadeState,
        ),
      ),
      floatingActionButton: FloatingActionButton(onPressed: _onTap),
    );
  }
}
{{< /highlight >}}
<img src="/images/basic/animation/03/animated_cross_fade.gif" style="min-width:300px;max-width:600px;" alt="AnimatedCrossFade"/>

<span id="animated_modal_barrier"></span>
## AnimatedModalBarrier

<img src="/images/basic/animation/03/animated_modal_barrier.gif" style="min-width:300px;max-width:600px;" alt="AnimatedModalBarrier"/>


<span id="animated_switcher"></span>
## AnimatedSwitcher

{{< highlight dart >}}

class _MainPageState extends State<MainPage> {

  bool isVisible = false;

  void _onTap() => setState(() => isVisible = !isVisible);

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('AnimatedSwitcher'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget>[
              AnimatedSwitcher(
                child: isVisible
                    ? Container(
                  key: UniqueKey(),
                  height: 100.0,
                  width: 100.0,
                  color: Colors.red,
                )
                    : Container(
                  key: UniqueKey(),
                  height: 200.0,
                  width: 100.0,
                  color: Colors.blue,
                ),
                duration: Duration(seconds: 2),
              ),
            ],
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(onPressed: _onTap),
    );
  }
}
{{< /highlight >}}
<img src="/images/basic/animation/03/animated_modal_barrier.gif" style="min-width:300px;max-width:600px;" alt="AnimatedSwitcher"/>


<span id="animated_icon"></span>
## AnimatedIcon

<img src="/images/basic/animation/03/animated_icon.gif" style="min-width:300px;max-width:600px;" alt="AnimatedIcon"/>



<span id="animated_list"></span>
## AnimatedList

<img src="/images/basic/animation/03/animated_list.gif" style="min-width:300px;max-width:600px;" alt="AnimatedList"/>


<span id="fade_in_image"></span>
## FadeInImage

<img src="/images/basic/animation/03/animated_modal_barrier.gif" style="min-width:300px;max-width:600px;" alt="FadeInImage"/>


<span id="hero"></span>
## Hero


<img src="/images/basic/animation/03/hero.gif" style="min-width:300px;max-width:600px;" alt="Hero"/>
