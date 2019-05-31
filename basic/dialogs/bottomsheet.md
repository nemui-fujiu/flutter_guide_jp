+++
title = "BottomSheet"
date = 2019-03-21T00:00:00+09:00
draft = false
weight = 362
description = "BottomSheetは、SnackBarのように画面下部から伸びてくる領域を作成できます。一覧項目のメニューや、音楽再生のコントローラ、ヘルプなど色々な使い方ができます。"
+++

## BottomSheet

BottomSheetは、SnackBarのように画面下部から伸びてくる領域を作成できます。  
一覧項目のメニューや、音楽再生のコントローラ、ヘルプなど色々な使い方ができます。

{{< highlight dart>}}
enum Answers{
  OK,
  CLOSE
}

class _MainPageState extends State<MainPage> {

  String _value = '';

  void _setValue(String value) => setState(() => _value = value);

  void _showBottom() async {
    var value = await showModalBottomSheet<Answers>(
      context: context,
      builder: (BuildContext context){
        return new Container(
          height: 250,
          padding: new EdgeInsets.all(10.0),
          child: new Column(
            children: <Widget>[
              new Text('複数行の内容を'),
              new Text('記載することができるので'),
              new Text('ヘルプなど'),
              new Text('ユーザ補助として'),
              new Text('つかえます。'),
              new RaisedButton(onPressed: () => Navigator.pop(context, Answers.OK), child: new Text('OK'),),
              new RaisedButton(onPressed: () => Navigator.pop(context, Answers.CLOSE), child: new Text('Close'),)
            ],
          ),
        );
      }
    );
    switch(value) {
      case Answers.OK:
        _setValue('OK');
        break;
      case Answers.CLOSE:
        _setValue('CLOSE');
        break;
    }
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('showModalBottomSheet'),
      ),
      body: new Center(
        child: new Column(
          children: <Widget>[
            new Text(_value, style: TextStyle(fontSize: 50, color: Colors.blueAccent, fontWeight: FontWeight.w600),),
            new RaisedButton(onPressed: _showBottom, child: new Text('Click me'),)
          ],
        ),
      ),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/dialog/02/bottom_sheet.gif" style="min-width:300px;max-width:600px;" alt="showModalBottomSheet"/>

BottomSheetを作るにはまず、``showModalBottomSheet``に``context``を渡して、``builder``に内容を書きます。
この時``showModalBottomSheet``のジェネリクスには戻り値の型を定義してください。   

{{< highlight dart>}}
showModalBottomSheet<Answers>(
  context: context,
  builder: (BuildContext context){
  }
);
{{< /highlight >}}

内容に関しては自由に作成できるので、単純なリスト以外にも好きに配置して、表現することが可能です。

{{< highlight dart>}}
return new Container(
  height: 200,
  padding: new EdgeInsets.all(10.0),
  child: new Column(
    children: <Widget>[
      new Text('複数行の内容を'),
      new Text('記載することができるので'),
      new Text('ヘルプなど'),
      new Text('ユーザ補助として'),
      new Text('つかえます。'),
      new RaisedButton(onPressed: () => Navigator.pop(context, Answers.OK), child: new Text('OK'),),
      new RaisedButton(onPressed: () => Navigator.pop(context, Answers.CLOSE), child: new Text('Close'),)
    ],
  ),
);
{{< /highlight >}}

``showModalBottomSheet``は``showDialog``と同じく``then``で書く方式と``async/await``があります。  
今回は``async/await``の書き方で戻り値を受け取っています。
ダイアログの閉じ方も、同じです。

また、GlobalKeyを使って``showBottomSheet``を利用すると以下のようになります。

{{< highlight dart>}}
class _MainPageState extends State<MainPage> {

  final GlobalKey<ScaffoldState> _scaffoldKey = new GlobalKey<ScaffoldState>();

  void _showBottom(){
    _scaffoldKey.currentState
        .showBottomSheet<Null>((BuildContext context) {
          return new Container(
            height: 200,
            padding: new EdgeInsets.all(10.0),
            child: new Column(
              children: <Widget>[
                new Text('複数行の内容を'),
                new Text('記載することができるので'),
                new Text('ヘルプなど'),
                new Text('ユーザ補助としても'),
                new Text('つかえます。'),
                new RaisedButton(onPressed: () => Navigator.pop(context), child: new Text('Close'),)
              ],
            ),
          );
        }
    );
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      key: _scaffoldKey,
      appBar: new AppBar(
        title: new Text('showModalBottomSheet'),
      ),
      body: new Center(
        child: new Column(
          children: <Widget>[
            new RaisedButton(onPressed: _showBottom, child: new Text('Click me'),)
          ],
        ),
      ),
    );
  }
}
{{< /highlight >}}

``_scaffoldKey``をScaffoldクラスに渡し``_scaffoldKey.currentState``を使って``showBottomSheet``表示することができます。
