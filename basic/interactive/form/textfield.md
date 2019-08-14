+++
title = "テキスト入力"
date = 2019-03-02T00:00:00+09:00
draft = false
weight = 324
description = "今回はテキスト入力について解説していきます。Flutterでのテキスト入力の方法と操作にはいくつかの方法があります。実際に利用する要件に合わせて使い分けてください。"
+++


## テキスト入力

今回はテキスト入力について解説していきます。   
Flutterでのテキスト入力の方法と操作にはいくつかの方法があります。実際に利用する要件に合わせて使い分けてください。  

### TextField

「TextField」は文字列を入力するためのシンプルな入力フォームとなります。

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

  String _text = '';

  void _handleText(String e) {
    setState(() {
      _text = e;
    });
  }

  Widget build(BuildContext context) {
    return Container(
        padding: const EdgeInsets.all(50.0),
        child: Column(
          children: <Widget>[
            Text(
              "$_text",
              style: TextStyle(
                  color: Colors.blueAccent,
                  fontSize: 30.0,
                  fontWeight: FontWeight.w500
              ),
            ),
            new TextField(
              enabled: true,
              // 入力数
              maxLength: 10,
              maxLengthEnforced: false,
              style: TextStyle(color: Colors.red),
              obscureText: false,
              maxLines:1 ,
              //パスワード
              onChanged: _handleText,
            ),
          ],
        )
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/interactive/02/textfield_01.png" style="min-width:300px;max-width:600px;" alt="TextField"/>


- ``enabled``は活性、非活性の切り替えに利用します。
- ``maxLenth``は入力可能な文字数です。  
- ``maxLengthEnforced``は入力可能な文字数の制限を超える場合の挙動を制御します。falseにしておくと最大文字数を超えても入力することが可能ですが、見た目はエラー状態となります。  
<img src="/images/basic/interactive/02/textfield_02.png" style="min-width:300px;max-width:600px;" alt="TextField maxLengthEnforced false"/>
- ``obscureText``をtrueにするとこのように、パスワード入力のように入力している値をマスクしてくれます。
<img src="/images/basic/interactive/02/textfield_03.png" style="min-width:300px;max-width:600px;" alt="TextField obscureText true"/>
- ``maxLines``は入力できる行数の最大行数を設定できます。これによりHTMLで言う所のテキストエリア表示が可能です。
- ``decoration``を追加することで、表示を整形することも可能です。  

{{< highlight dart>}}
new TextField(
  enabled: true,
  // 入力数
  maxLength: 10,
  maxLengthEnforced: false,
  style: TextStyle(color: Colors.black),
  obscureText: false,
  maxLines:1 ,
  decoration: const InputDecoration(
    icon: Icon(Icons.face),
    hintText: 'お名前を教えてください',
    labelText: '名前 *',
  ),
  //パスワード
  onChanged: _handleText,
),
{{< /highlight >}}
<img src="/images/basic/interactive/02/textfield_04.png" style="min-width:300px;max-width:600px;" alt="TextField obscureText true"/>

- ``inputFormatters``を利用すると、入力値に対するフォーマットを行うことが可能です。  
フォーマッターは「TextInputFormatter」を継承して自作もできますし、以下のような元から存在するフォーマッターを利用することもできます。
  - WhitelistingTextInputFormatter
  - BlacklistingTextInputFormatter

#### WhitelistingTextInputFormatter

「WhitelistingTextInputFormatter」または「BlacklistingTextInputFormatter」を使う場合は以下のimportが必要です。
{{< highlight dart>}}
import 'package:flutter/services.dart';
{{< /highlight >}}

{{< highlight dart>}}
new TextField(
  enabled: true,
  // 入力数
  maxLength: 10,
  maxLengthEnforced: false,
  style: TextStyle(color: Colors.black),
  obscureText: false,
  maxLines:1 ,
  inputFormatters: <TextInputFormatter> [
    WhitelistingTextInputFormatter.digitsOnly,
  ],
  decoration: const InputDecoration(
    icon: Icon(Icons.face),
    hintText: '年齢を入力してください',
    labelText: '年齢 *',
  ),
  //パスワード
  onChanged: _handleText,
),
{{< /highlight >}}

<img src="/images/basic/interactive/02/textfield_05.gif" style="min-width:300px;max-width:600px;" alt="TextField WhitelistingTextInputFormatter"/>

このように、``WhitelistingTextInputFormatter``は入力可能な文字を制限することが可能で、デフォルトで``digitsOnly``が用意されています。  
他にも制御をしたい場合は以下のように正規表現を渡すことによって独自に文字列を制限することが可能です。
{{< highlight dart>}}
WhitelistingTextInputFormatter(RegExp(r'\d+'))
{{< /highlight >}}

#### BlacklistingTextInputFormatter

また同じ要領で``BlacklistingTextInputFormatter``を使うことで入力を制限することも可能です。  
デフォルトで``singleLineFormatter``が用意されています。
独自に実装したい場合は、以下のように実装することで、独自に文字列を制限することが可能です。
{{< highlight dart>}}
BlacklistingTextInputFormatter(RegExp(r'\n'))
{{< /highlight >}}

``BlacklistingTextInputFormatter``は、リストに該当する文字列があった場合に、文字列を置換することも可能です。  
その場合は、以下のように第２引数に置換後の文字列を渡してください。
{{< highlight dart>}}
BlacklistingTextInputFormatter(RegExp(r'[0-9]'), replacementString:'-')
{{< /highlight >}}
※数値の入力を全てハイフンへ変換しています。

### TextEditingController

「TextEditingController」はテキスト入力を制御するのに利用します。  
「TextField」や、「TextFormField」に対して「TextEditingController」を使って、複雑な制御を行うことができます。   
「TextField」の制御を「TextEditingController」を利用して行います。

{{< highlight dart>}}
class _ChangeFormState extends State<ChangeForm> {

  final TextEditingController _textEditingController = new TextEditingController();

  String _text = '';
  
  @override
    void initState() {
      super.initState();
      _textEditingController.addListener(_printLatestValue);
  }
  @override
  void dispose() {
    _textEditingController.dispose();
    super.dispose();
  }

  void _handleText(String e) {
    setState(() {
      _text = e;
    });
  }
  void _printLatestValue() {
    print("入力状況: ${_textEditingController.text}");
  }

  Widget build(BuildContext context) {
    return Container(
        padding: const EdgeInsets.all(50.0),
        child: Column(
          children: <Widget>[
            Text(
              "$_text",
              style: TextStyle(
                  color:Colors.blueAccent,
                  fontSize: 30.0,
                  fontWeight: FontWeight.w500
              ),
            ),
            new TextField(
              enabled: true,
              maxLength: 10,
              maxLengthEnforced: false,
              obscureText: false,
              controller: _textEditingController,
              onChanged: _handleText,
              onSubmitted: _submission,
            )
          ],
        )
    );
  }
  void _submission(String e) {
    print(_textEditingController.text);
    _textEditingController.clear();
    setState(() {
      _text = '';
    });
  }
}
{{< /highlight >}}

<img src="/images/basic/interactive/02/textfield_06.gif" style="min-width:300px;max-width:600px;" alt="TextEditingController"/>

このように「TextEditingController」を使うことで、保存後に「TextField」の中身をクリアすることも可能です。

``initState()``メソッドで``addListener``を設定し、処理を追加することで入力途中での文言をプリントしています。  
このようにリスナー登録することで複雑な制御が可能です。  

{{< highlight dart>}}
@override
void initState() {
  super.initState();
  _textEditingController.addListener(_printLatestValue);
}
_printLatestValue() {
  print("入力状況: ${_textEditingController.text}");
}
{{< /highlight >}}

「_ChangeFormState」クラスが破棄されるタイミングで確実にリソースが破棄されるように以下を記載しておきましょう。

{{< highlight dart>}}
@override
void dispose() {
  _textEditingController.dispose();
  super.dispose();
}
{{< /highlight >}}

### TextFormField

「TextFormField」はフォーム入力で必要な色々な機能を提供してくれます。

{{< highlight dart>}}
class _ChangeFormState extends State<ChangeForm> {

  final _formKey = GlobalKey<FormState>();

  String _name = '';
  String _email = '';

  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      child: Container(
        padding: const EdgeInsets.all(50.0),
        child: Column(
          children: <Widget>[
            new TextFormField(
              enabled: true,
              maxLength: 20,
              maxLengthEnforced: false,
              obscureText: false,
              autovalidate: false,
              decoration: const InputDecoration(
                hintText: 'お名前を教えてください',
                labelText: '名前 *',
              ),
              validator: (String value) {
                return value.isEmpty ? '必須入力です' : null;
              },
              onSaved: (String value) {
                this._name = value;
              },
            ),
            new TextFormField(
              maxLength: 100,
              autovalidate: true,
              decoration: const InputDecoration(
                hintText: '連絡先を教えてください。',
                labelText: 'メールアドレス *',
              ),
              validator: (String value) {
                return !value.contains('@') ? 'アットマーク「＠」がありません。' : null;
              },
              onSaved: (String value) {
                this._email = value;
              },
            ),
            RaisedButton(
              onPressed: _submission,
              child: Text('保存'),
            )
          ],
        )
      )
    );
  }
  void _submission() {
    if (this._formKey.currentState.validate()) {
      this._formKey.currentState.save();
      Scaffold
          .of(context)
          .showSnackBar(SnackBar(content: Text('Processing Data')));
      print(this._name);
      print(this._email);
    }
  }
}
{{< /highlight >}}

<img src="/images/basic/interactive/02/textfield_07.gif" style="min-width:300px;max-width:600px;" alt="TextFormField"/>

- 「TextFormField」では、``validator``が利用可能です。  
``validator()``メソッドに実装を書くことで入力チェックを可能にしています。  
- ``autovalidate``を設定すると文字列入力と同時に``validator()``メソッドが呼ばれるようになります。  
これには少し欠点があり、初回表示時にもチェックがかかっているので、表示時点で項目がエラー表示されてしまうので注意が必要です。
falseの場合にどのタイミングで``validator()``メソッドが実行されるのかについては後述します。
- ``onSaved()``メソッドは、処理全体が保存されるタイミングで呼び出されます。
- 「Form」クラスと「GlobalKey」クラスを使うことで、フォームをユニークなグループにします。  
今回以下のように保存処理を実行しています。

{{< highlight dart>}}
void _submission() {
  if (this._formKey.currentState.validate()) {
    this._formKey.currentState.save();
    Scaffold
        .of(context)
        .showSnackBar(SnackBar(content: Text('Processing Data')));
    print(this._name);
    print(this._email);
  }
}
{{< /highlight >}}

以下が実行されることで、``_formKey``のフォームに登録されているすべての``validator()``が実行されその結果を返します。

{{< highlight dart>}}
this._formKey.currentState.validate()
{{< /highlight >}}

以下を実行されることで、``_formKey``のフォームに登録されているすべての``save()``が実行されます。
{{< highlight dart>}}
this._formKey.currentState.save();
{{< /highlight >}}



## 参考

[TextField](https://docs.flutter.io/flutter/material/TextField-class.html)  
[TextEditingController](https://docs.flutter.io/flutter/material/TextEditingController-class.html)  
[TextFormField](https://docs.flutter.io/flutter/material/TextFormField-class.html)  
[Cookbook](https://flutter.dev/docs/cookbook/forms/text-input)  
[validation](https://flutter.dev/docs/cookbook/forms/validation)  
[TextInputFormatter](https://docs.flutter.io/flutter/services/TextInputFormatter-class.html)    
[Form](https://docs.flutter.io/flutter/widgets/Form-class.html)  

