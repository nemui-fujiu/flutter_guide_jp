+++
title = "Statefulウィジェット"
date = 2019-02-09T00:00:00+09:00
draft = false
weight = 25
description = "FlutterにはStatelessとStatefulの2種類のWidgetがあります。Statefulウィジェットを利用して値を保持できるようになり作成できるアプリの幅を広げましょう！"
keywords = "Flutter,アプリ,チュートリアル,tutorial,入門,StatefulWidget,StatelessWidget"
+++

## Step3

さてここではStatefulWidgetを利用していきましょう。  
FlutterにはStatelessとStatefulの2種類のWidgetがあります。   
今回はそれぞれの違いを理解しつつ、Statefulウィジェットを利用して値を保持できるようなアプリを作れるようになりましょう！

### StatefulWidget & StatelessWidget

StatelessWidgetで扱うあたいはすべて不変的でありプロパティを変更することはできません。  
すべての値はfinalな値となります。
StatefulWidgetではウィジェットの生存期間中に変更される値を維持することができ、実装するには、少なくとも2つのクラスが必要です。

1) Stateクラスのインスタンスを作成するStatefulWidgetクラス  
2) Stateクラス

StatefulWidgetクラスそれ自体は不変ですが、Stateクラスでウィジェットの存続期間中は値を保持します。

<img src="https://flutter.ctrnost.com/images/tutorial/05/01_Stateless_Stateful.png" width="600px"  alt="Stateless&Stateful" />

### 状態保持

1. 最小の状態保持クラスを作成します。  
``lib/main.dart``の末尾に以下を追加して下ください。

    {{< highlight dart >}}
    class RandomWordsState extends State<RandomWords> {
    }
    {{< /highlight >}}

    ``State<RandomWords>``と書くことで汎用のStateクラスのジェネリクスにRandomWordsを記載し、RandomWordsウィジェットの状態を維持できるようにします。
    この時点では、Stateクラスの必須メソッドが実装されていないためIDEではエラーが出ていると思いますが、あとで解消するのでそのままにして次へ進んでください。

2. RandomWordsウィジェットの作成
StatefulWidgetを継承してウィジェットを作成します。

    {{< highlight dart >}}
    class RandomWords extends StatefulWidget {
      @override
      RandomWordsState createState() => new RandomWordsState();
    }
    {{< /highlight >}}

3. RandomWordsStateクラスにbuildメソッドを追加

    {{< highlight dart >}}
    class RandomWordsState extends State<RandomWords> {
      @override
      Widget build(BuildContext context) {
        final wordPair = WordPair.random();
        return Text(wordPair.asPascalCase);
      }
    }
    {{< /highlight >}}

4. MyAppの修正   
``MyApp``クラスを以下のように修正しましょう。

    {{< highlight dart >}}
    class MyApp extends StatelessWidget {
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
              child: RandomWords(),
            ),
          ),
        );
      }
    }
    {{< /highlight >}}

5. 実行
ソースを保存しホットリロードが実行されることで、ソースが書き換わります。
ただし、今回見た目上の変化はありません。
今回の作業がいきていくるのは次のListView作成になってからです。
<img src="https://flutter.ctrnost.com/images/tutorial/04/01_english_words.png" width="600px"  alt="English Words">


