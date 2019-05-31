+++
title = "外部パッケージ"
date = 2019-02-08T00:00:00+09:00
draft = false
weight = 24
description = "外部パッケージの取り込み方を覚えて、便利な外部パッケージを使いこなして、取得した外部パッケージの使い方を覚えましょう。"
keywords = "Flutter,アプリ,チュートリアル,tutorial,入門,外部パッケージ,import,パッケージ"
+++

## Step2

外部パッケージの取り込み方を覚えて、便利な外部パッケージを使いこなしましょう！

1. 次に外部パッケージの取り込み方法を学びましょう。``/pubspec.yaml``で依存関係を管理するので、以下のように編集します。   

    {{< highlight yaml >}}
      dependencies:
         flutter:
           sdk: flutter
         cupertino_icons: ^0.1.2
         english_words: ^3.1.0
    {{< /highlight >}}

    今回取り込むのは``english_words``というパッケージです。
    このパッケージはランダムに英単語を生成するためのライブラリで、デモやテストなどランダムに文字列を取得したいときに利用します。   
    [english_words](https://pub.dartlang.org/packages/english_words)
    
    Flutterで使える外部パッケージは[Flutter Paclages](https://pub.dartlang.org/flutter)で手に入るので、何か作るときは一度パッケージがないか探してみるといいと思います。

2. 通常は``/pubspec.yaml``に変更を加え保存すると同時にパッケージがダウンロードされます。
もしダウンロードされない場合は直接以下コマンドを叩いてパッケージを取得してください。
    {{< highlight bash >}}
    $ flutter packages get
    {{< /highlight >}}

3. ``lib/main.dart``に先ほど取得した外部パッケージをimportします。

    {{< highlight dart >}}
      import 'package:flutter/material.dart';
      import 'package:english_words/english_words.dart';
    {{< /highlight >}}

4. 固定で表示していた文字列をランダムな英語に変更しましょう。

    {{< highlight dart >}}
      @override
      Widget build(BuildContext context) {
        final wordPair = WordPair.random();
        return MaterialApp(
          title: 'Welcome to Flutter',
          home: Scaffold(
            appBar: AppBar(
              title: Text('Welcome to Flutter'),
            ),
            body: Center(
               child: Text(wordPair.asPascalCase),
            ),
          ),
        );
      }
    {{< /highlight >}}

5. 変更したらソースを保存してみてください。
ホットリロードのタイミングで表示される単語が変わるようになったかと思います。
これは、buildメソッド内で「MaterialApp」がレンダリングが必要になるたびに、生成されているためです。
<img src="https://flutter.ctrnost.com/images/tutorial/04/01_english_words.png" width="600px"  alt="English Words">

