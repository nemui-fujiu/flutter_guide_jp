+++
title = "スライダー"
date = 2019-03-06T00:00:00+09:00
draft = false
weight = 328
description = "スライダーは数値の変更でよく使われるUIです。値の間隔など細かくすることも可能なのでとても便利に利用することができます。"
+++

## Slider

スライダーは数値の変更でよく使われるUIです。  
値の間隔など細かくすることも可能なのでとても便利に利用することができます。

### Slider

{{< highlight dart>}}
class _ChangeFormState extends State<ChangeForm> {

  double _value = 0.0;
  double _startValue = 0.0;
  double _endValue = 0.0;

  void _changeSlider(double e) => setState(() { _value = e; });
  void _startSlider(double e) => setState(() { _startValue = e; });
  void _endSlider(double e) => setState(() { _endValue = e; });

  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.all(50.0),
      child: Column(
        children: <Widget>[
          Center(child:Text("現在の値：${_value}")),
          Center(child:Text("開始時の値：${_startValue}")),
          Center(child:Text("終了時の値：${_endValue}")),
          new Slider(
            label: '${_value}',
            min: 0,
            max: 10,
            value: _value,
            activeColor: Colors.orange,
            inactiveColor: Colors.blueAccent,
            divisions: 10,
            onChanged: _changeSlider,
            onChangeStart: _startSlider,
            onChangeEnd: _endSlider,
          )
        ],
      )
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/interactive/02/slider_01.gif" style="min-width:300px;max-width:600px;" alt="Slider"/>

- ``label``はスライダーを動かしている時に表示されるラベル
- ``min``は最小値
- ``max``は最大値
- ``value``はスライダーの値(double)
- ``activeColor``はスライダーの選択範囲の線の色
- ``inactiveColor``はスライダーの未選択範囲の線の色
- ``divisions``はメモリの数値を決めるための値で、``(max - min) / divisions``の計算で値が決定します。
- ``onChanged``は値を変更した時に動作する。
- ``onChangeStart``は値を変更開始した時に動作する。
- ``onChangeEnd``は値を変更終了した時に動作する。

``divisions``など特に細かな設定をしない場合はスライダーはdoubleの値を取得できるため、細かな値を取ることも可能です。
