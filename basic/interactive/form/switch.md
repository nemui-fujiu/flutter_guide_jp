+++
title = "スイッチ"
date = 2019-03-06T00:00:00+09:00
draft = false
weight = 327
description = ""
+++

## Switch

スイッチはOnとOffの切り替えを行うことができます。  
色の変更や、ListTileを使ったレイアウトなどがあるので、色々試してみてください。  

### Switch

「Switch」はWebページでおなじみのスイッチトグルと同じものを作成することができます。

{{< highlight dart>}}
class _ChangeFormState extends State<ChangeForm> {

  bool _active = false;

  void _changeSwitch(bool e) => setState(() => _active = e);

  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.all(50.0),
      child: Column(
        children: <Widget>[
          Center(
            child: new Icon(
              Icons.thumb_up,
              color: _active ? Colors.orange[700] : Colors.grey[500],
              size: 100.0,
            ),
          ),
          new Switch(
            value: _active,
            activeColor: Colors.orange,
            activeTrackColor: Colors.red,
            inactiveThumbColor: Colors.blue,
            inactiveTrackColor: Colors.green,
            onChanged: _changeSwitch,
          )
        ],
      )
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/interactive/02/switch_01.gif" style="min-width:300px;max-width:600px;" alt="Switch"/>

- ``activeColor``スイッチが選択状態になっている時の下地の色
- ``activeTrackColor``スイッチが選択状態になっている時のトラッカーの色
- ``inactiveColor``スイッチが未選択状態になっている時の下地の色
- ``inactiveThumbColor``スイッチが未選択状態になっている時のトラッカーの色
- ``onChanged``値を変更した時に動作する。

### SwitchListTile

「SwitchListTile」は「ListTile」の一種で、標準的なレイアウトを提供してくれます。

{{< highlight dart>}}
  Widget build(BuildContext context) {
    return Container(padding: const EdgeInsets.all(10.0), 
      child: Column(
        children: <Widget>[
          new SwitchListTile(
            value: _active,
            activeColor: Colors.orange,
            activeTrackColor: Colors.red,
            inactiveThumbColor: Colors.blue,
            inactiveTrackColor: Colors.grey,
            secondary: new Icon(
              Icons.thumb_up,
              color: _active ? Colors.orange[700] : Colors.grey[500],
              size: 50.0,
            ),
            title: Text('タイトル'),
            subtitle: Text('サブタイトル'),
            onChanged: _changeSwitch,
          )
        ],
      )
    );
  }
{{< /highlight >}}

<img src="/images/basic/interactive/02/switch_02.gif" style="min-width:300px;max-width:600px;" alt="SwitchListTile"/>

- ``title``はスイッチトグルのタイトル
- ``subtitle``はスイッチトグルのサブタイトル
- ``secondary``はアイコンなどをつけたい場合に利用してください。

スイッチを作成する時に``SwitchListTile.adaptive``を使って作成することも可能ですが、こちらはiOS向けのデザインを提供するために使います。  
``SwitchListTile.adaptive``を利用すると、iOS側ではいくつかの要素が使えなくなるので注意が必要です。  
