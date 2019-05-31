+++
title = "ダイアログ・アラート"
date = 2019-03-21T00:00:00+09:00
draft = false
weight = 361
description = "SimpleDialogとAlertDialogの作成方法について解説していきます。お知らせや、注意喚起、メニューなど様々な用途で使うウィジェットで簡単に作成できます。"
+++

## ダイアログ

SimpleDialogとAlertDialogの作成方法について解説していきます。  
お知らせや、注意喚起、メニューなど様々な用途で使うウィジェットで簡単に作成できます。

## SimpleDialog

シンプルダイアログはその名の通りシンプルなダイアログを実装できるので一つずつ見ていきましょう。

{{< highlight dart>}}
enum Answers{
  YES,
  NO
}

class _MainPageState extends State<MainPage> {

  String _value = '';

  void _setValue(String value) => setState(() => _value = value);

  Future _showDialog() async {
    var value = await showDialog<Answers>(
      context: context,
      builder: (BuildContext context) => new SimpleDialog(
        title: new Text('SimpleDialog'),
        children: <Widget>[
          new SimpleDialogOption(child: new Text('Yes'),onPressed: (){Navigator.pop(context, Answers.YES);},),
          new SimpleDialogOption(child: new Text('NO'),onPressed: (){Navigator.pop(context, Answers.NO);},),
        ],
      ),
    );
    switch(value) {
      case Answers.YES:
        _setValue('Yes');
        break;
      case Answers.NO:
        _setValue('NO');
        break;
    }
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('SimpleDialog'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget>[
              new Text(_value, style: TextStyle(fontSize: 50, color: Colors.blueAccent, fontWeight: FontWeight.w600),),
              new RaisedButton(onPressed: _showDialog, child: new Text('ダイアログを開く'),)
            ],
          ),
        ),
      ),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/dialog/01/simple_dialog.gif" style="min-width:300px;max-width:600px;" alt="SimpleDialog"/>

まずは非同期のメソッドを作成します。
{{< highlight dart>}}
  Future _showDialog() async {
  }
{{< /highlight >}}

次にダイアログを作成して、表示する内容を設定します。

{{< highlight dart>}}
  await showDialog<Answers>(
    context: context,
    builder: (BuildContext context) => 
  )
{{< /highlight >}}

``showDialog``には``child``プロパティもあるのですが、これは非推奨(Deprecated)となっているので、Builderで作成してください。
今回は``SimpleDialog``で作成します。

{{< highlight dart>}}
new SimpleDialog(
  title: new Text('SimpleDialog'),
  children: <Widget>[
    new SimpleDialogOption(child: new Text('Yes'),onPressed: (){Navigator.pop(context, Answers.YES);},),
    new SimpleDialogOption(child: new Text('NO'),onPressed: (){Navigator.pop(context, Answers.NO);},),
  ],
),
{{< /highlight >}}

``titlePadding``や``contentPadding``でレイアウトをいじることも可能です。
上の例では``title``と``children``でそれぞれ設問と解答を用意しています。

{{< highlight dart>}}
  children: <Widget>[
    new SimpleDialogOption(child: new Text('Yes'),onPressed: (){Navigator.pop(context, Answers.YES);},),
    new SimpleDialogOption(child: new Text('NO'),onPressed: (){Navigator.pop(context, Answers.NO);},),
  ],
{{< /highlight >}}

見てわかる通り``children``は配列を受け取れるため、``SimpleDialogOption``を複数配置してボタンを用意することが可能です。

{{< highlight dart>}}
new SimpleDialogOption(child: new Text('Yes'),onPressed: (){Navigator.pop(context, Answers.YES);},),
{{< /highlight >}}

``Navigator.pop``は、ダイアログを閉じるために使います。
ちなみに``Navigator.push``は遷移を進める時、``Navigator.pop``は遷移を戻る時に利用する機能です。

これにより、表示したダイアログを戻し、第２引数に書かれている値を選択した時に戻り値として返します。  

{{< highlight dart>}}
var value = await showDialog(
);
switch(value) {
  case Answers.YES:
    _setValue('Yes');
    break;
  case Answers.NO:
    _setValue('NO');
    break;
}
{{< /highlight >}}

すると選択された値が``switch``文で評価され、``case``によって処理を分岐しています。  
このように簡単にダイアログ表示ができるのがSimpleDialogです。

今回は``async``を使ってダイアログ処理をしましたが以下のように``then``を使って書くこともできます。

{{< highlight dart>}}
  void _showDialog() {
    showDialog(
      context: context,
      builder: (BuildContext context) => new SimpleDialog(
        title: new Text('SimpleDialog'),
        children: <Widget>[
          new SimpleDialogOption(child: new Text('Yes'),onPressed: (){Navigator.pop(context, Answers.YES);},),
          new SimpleDialogOption(child: new Text('NO'),onPressed: (){Navigator.pop(context, Answers.NO);},),
        ],
      )
    ).then<void>((value) {
      switch(value) {
      case Answers.YES:
      _setValue('Yes');
      break;
      case Answers.NO:
      _setValue('NO');
      break;
      }
    });
  }
{{< /highlight >}}

Dartでの非同期処理には``then``で書く方式と``async/await``で書く方式があるので必要に応じて使い分けてください。
個人的な感想ですが、本格的に非同期を多用する場合は``async/await``をうまく使うことでネストを減らし可読性の高いコードがかけると思います。

## AlertDialog

アラートダイアログは以下のように実装できます。

{{< highlight dart>}}
enum Answers{
  YES,
  NO
}

class _MainPageState extends State<MainPage> {

  String _value = '';

  void _setValue(String value) => setState(() => _value = value);

  Future _showDialog() async {
    var value = await showDialog(
      context: context,
      builder: (BuildContext context) => new AlertDialog(
        title: new Text('AlertDialog'),
        content: new Text('アラートダイアログです。YesかNoを選択してください。'),
        actions: <Widget>[
          new SimpleDialogOption(child: new Text('Yes'),onPressed: (){Navigator.pop(context, Answers.YES);},),
          new SimpleDialogOption(child: new Text('NO'),onPressed: (){Navigator.pop(context, Answers.NO);},),
        ],
      ),
    );
    switch(value) {
      case Answers.YES:
        _setValue('Yes');
        break;
      case Answers.NO:
        _setValue('NO');
        break;
    }
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('AlertDialog'),
      ),
      body: new Container(
        padding: new EdgeInsets.all(32.0),
        child: new Center(
          child: new Column(
            children: <Widget>[
              new Text(_value, style: TextStyle(fontSize: 50, color: Colors.blueAccent, fontWeight: FontWeight.w600),),
              new RaisedButton(onPressed: _showDialog, child: new Text('ダイアログを開く'),)
            ],
          ),
        ),
      ),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/dialog/01/alert_dialog.gif" style="min-width:300px;max-width:600px;" alt="AlertDialog"/>

SimpleDialogとの違いは、以下の部分です。
新たに``content``によって詳細を定義できるようになったのと、選択するボタンが``actions``に変わります。

{{< highlight dart>}}
  builder: (BuildContext context) => new AlertDialog(
    title: new Text('AlertDialog'),
    content: new Text('アラートダイアログです。YesかNoを選択してください。'),
    actions: <Widget>[
      new SimpleDialogOption(child: new Text('Yes'),onPressed: (){Navigator.pop(context, Answers.YES);},),
      new SimpleDialogOption(child: new Text('NO'),onPressed: (){Navigator.pop(context, Answers.NO);},),
    ],
  ),
{{< /highlight >}}

こちらもSimpleDialogと同じように複数定義することが可能です。
見た目上はSimpleDialog縦に並んでいたのに対して、AlertDialogは横並びになります。
