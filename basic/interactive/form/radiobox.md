+++
title = "ラジオボタン"
date = 2019-03-06T00:00:00+09:00
draft = false
weight = 326
description = "ラジオはOnとOffの切り替えを行うことができます。 色の変更や、ListTileを使ったレイアウトなどがあるので、色々試してみてください。 "
+++
## Radio

ラジオはOnとOffの切り替えを行うことができます。  
色の変更や、ListTileを使ったレイアウトなどがあるので、色々試してみてください。  

### Radio

「Radio」はWebページでおなじみのラジオボタンと同じものを作成することができます。

{{< highlight dart>}}
class _ChangeFormState extends State<ChangeForm> {

  String _type = '';

  void _handleRadio(String e) => setState(() {_type = e;});

  IconData _changeIcon(String e) {
    IconData icon = null;
    switch (e) {
      case 'thumb_up':
        icon = Icons.thumb_up;
        break;
      case 'favorite':
        icon = Icons.favorite;
        break;
      default:
        icon = Icons.thumb_up;
    }
    return icon;
  }

  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.all(50.0),
      child: Column(
        children: <Widget>[
          Center(
            child: new Icon(
              _changeIcon(_type),
              color: Colors.orange[700],
              size: 100.0,
            ),
          ),
          new Radio(
            activeColor: Colors.blue,
            value: 'thumb_up',
            groupValue: _type,
            onChanged: _handleRadio,
          ),
          new Radio(
            activeColor: Colors.orange,
            value: 'favorite',
            groupValue: _type,
            onChanged: _handleRadio,
          ),
        ],
      )
    );
  }
}
{{< /highlight >}}
<img src="/images/basic/interactive/02/radio_01.gif" style="min-width:300px;max-width:600px;" alt="Radio"/>

- ``value``はラジオボタンの値です。
- ``groupValue``は同じラジオボタンとして動作する「Radio」の値です。
- ``activeColor``はラジオがOnになっている時の見た目です。
- ``onChanged``はラジオボタンの値が変更された時に動作します。

### RadioListTile

「RadioListTile」は「ListTile」の一種で、標準的なレイアウトを提供してくれます。

{{< highlight dart>}}
children: <Widget>[
  Center(
    child: new Icon(
      _changeIcon(_type),
      color: Colors.orange[700],
      size: 100.0,
    ),
  ),
  new RadioListTile(
    secondary: Icon(Icons.thumb_up),
    activeColor: Colors.blue,
    controlAffinity: ListTileControlAffinity.trailing,
    title: Text('Good'),
    subtitle: Text('Goodアイコンの表示'),
    value: 'thumb_up',
    groupValue: _type,
    onChanged: _handleRadio,
  ),
  new RadioListTile(
    secondary: Icon(Icons.favorite),
    activeColor: Colors.orange,
    controlAffinity: ListTileControlAffinity.trailing,
    title: Text('Favorite'),
    subtitle: Text('Favoriteアイコンの表示'),
    value: 'favorite',
    groupValue: _type,
    onChanged: _handleRadio,
  ),
],
{{< /highlight >}}

<img src="/images/basic/interactive/02/radio_02.gif" style="min-width:300px;max-width:600px;" alt="RadioListTile"/>

このように、綺麗なレイアウトが可能です。

- ``title``はラジオボタンのタイトル
- ``subtitle``はラジオボタンのサブタイトル
- ``secondary``はアイコンなどをつけたい場合に利用してください。
- ``controlAffinity``ではラジオボタンの配置を変更できます。  
``ListTileControlAffinity.leading``は先頭にラジオボタンを持ってきます。    
``ListTileControlAffinity.trailing``は末尾にラジオボタンを持ってきます。  
``ListTileControlAffinity.platform``は実行しているプラットフォームに対して一般的な配置にラジオボタンを持ってきます。  
