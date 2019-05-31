+++
title = "チェックボックス"
date = 2019-03-06T00:00:00+09:00
draft = false
weight = 325
description = "チェックボックスはOnとOffの切り替えを行うことができます。色の変更や、CheckboxListTileを使ったレイアウトなどがあるので、色々試してみてください。  "
+++

## CheckBox

チェックボックスはOnとOffの切り替えを行うことができます。  
色の変更や、ListTileを使ったレイアウトなどがあるので、色々試してみてください。  

### CheckBox

「CheckBox」はWebページでおなじみのチェックボックスと同じものを作成することができます。

{{< highlight dart>}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Form',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Form'),
        ),
        body: Center(
          child: ChangeForm(),
        ),
      ),
    );
  }
}

class ChangeForm extends StatefulWidget {
  @override
  _ChangeFormState createState() => _ChangeFormState();
}

class _ChangeFormState extends State<ChangeForm> {

  bool _flag = false;

  void _handleCheckbox(bool e) {
    setState(() {
      _flag = e;
    });
  }

  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.all(50.0),
      child: Column(
        children: <Widget>[
          Center(
            child: new Icon(
              Icons.thumb_up,
              color: _flag ? Colors.orange[700] : Colors.grey[500],
              size: 100.0,
            ),
          ),
          new Checkbox(
            activeColor: Colors.blue,
            value: _flag,
            onChanged: _handleCheckbox,
          ),
        ],
      )
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/interactive/02/checkbox_01.gif" style="min-width:300px;max-width:600px;" alt="CheckBox"/>

このように、On、Offを切り替える時に利用します。

- ``value``はチェックボックスの値です。
- ``activeColor``はチェックがOnになっている時の見た目です。
- ``onChanged``はチェックボックスの値が変更された時に動作します。

### CheckboxListTile

「CheckboxListTile」は「ListTile」の一種で、標準的なレイアウトを提供してくれます。

{{< highlight dart>}}
  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.all(10.0),
      child: Column(
        children: <Widget>[
          new CheckboxListTile(
            activeColor: Colors.blue,
            title: Text('チェックボックス'),
            subtitle: Text('チェックボックスのサブタイトル'),
            secondary: new Icon(
              Icons.thumb_up,
              color: _flag ? Colors.orange[700] : Colors.grey[500],
            ),
            controlAffinity: ListTileControlAffinity.leading,
            value: _flag,
            onChanged: _handleCheckbox,
          ),
        ],
      )
    );
  }
{{< /highlight >}}

<img src="/images/basic/interactive/02/checkbox_02.gif" style="min-width:300px;max-width:600px;" alt="CheckboxListTile"/>

このように、綺麗なレイアウトが可能です。

- ``title``はチェックボックスのタイトル
- ``subtitle``はチェックボックスのサブタイトル
- ``secondary``はアイコンなどをつけたい場合に利用してください。
- ``controlAffinity``ではチェックボックスの配置を変更できます。  
``ListTileControlAffinity.leading``は先頭にチェックボックスを持ってきます。    
``ListTileControlAffinity.trailing``は末尾にチェックボックスを持ってきます。  
``ListTileControlAffinity.platform``は実行しているプラットフォームに対して一般的な配置にチェックボックスを持ってきます。  




