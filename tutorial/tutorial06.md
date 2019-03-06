+++
title = "無限スクロールListView"
date = 2019-02-09T00:00:00+09:00
draft = false
weight = 26
description = "ListViewを使って無限スクロールするさらにアプリっぽいレイアウトを作成しましょう！これでListViewの簡単な動きもわかってくると思います。"
keywords = "Flutter,アプリ,チュートリアル,tutorial,入門,ListView"
+++

## Step4

今度はListViewを使って無限スクロールするさらにアプリっぽいレイアウトを作成しましょう！  
やっとアプリっぽくなってきましたね。

先ほど作成した、RandomWordStateを使いリストを作成していきます。   
ユーザがスクロールすると、ListViewウィジェットに表示されるリストは無限にスクロールできるようにします。
ListViewのbuilderを利用すると必要に応じてリストビューを遅延構築ができます。


### ListViewの作成

1. 単語を保存するために、_suggestionsリストをRandomWordsStateクラスに追加します。   
また、_biggerFont変数を用意してフォントサイズを指定できるようにしましょう。

    ```dart
    class RandomWordsState extends State<RandomWords> {
      final _suggestions = <WordPair>[];
      final _biggerFont = const TextStyle(fontSize: 18.0);
      // ···
    }
    ```
<div style="background-color:#d1ecf1;padding:20px;color:#0c5460;border-radius:10px;">
<span>
変数やメソッド名の前につけるアンダースコア(_)はDartではPrivateを意味します。<br/>  
今回定義した2つの変数はprivate変数となり、クラス内からしかアクセスできません。<br/>  
Dartをあまり使ったことがない人には馴染みがないと思いますのでここで覚えておきましょう。<br/>
</span>
</div>

2. _buildSuggestionsメソッドの追加   
    次にRandomWordsStateクラスに_buildSuggestions()を追加しましょう。   
    このメソッドでListViewを作成します。

    ListViewクラスのbuilderにあるitemBuilderには無名関数を定義します。  
    引数としてBuildContextと行番号(i)が渡されます。  
    iは0から始まり、呼び出されるたびに加算されていきます。  
    今回の構成ではユーザがスクロールするたびにリストが無限に増加します。 


    ```dart
    Widget _buildSuggestions() {
      return ListView.builder(
          padding: const EdgeInsets.all(16.0),
          itemBuilder: /*1*/ (context, i) {
            if (i.isOdd) return Divider(); /*2*/
    
            final index = i ~/ 2; /*3*/
            if (index >= _suggestions.length) {
              _suggestions.addAll(generateWordPairs().take(10)); /*4*/
            }
            return _buildRow(_suggestions[index]); /*5*/
          });
    }
    ```
 
    1. itemBuilderで一行ごとに処理が呼ばれ、偶数行の場合にListTileを表示し、奇数行のときにDividerを表示します。   
    こうすることで文字と線を交互に表示して、見た目上わかりやすくしています。
    2. 1ピクセルの高さの仕切りをListViewに追加しています。
    3. 行数を2で割ったときの整数値を求めます。   
    これによりDividerで線を入れた行を抜いた状態の英文数を計算しています。 
    4. 利用可能な英文リスト数を超えた場合は、さらに10個の英文を生成しリストに追加します。
    5. 最後に現在の行で表示する英文をリストから取得し、_buildRowメソッドで表示用に整形します。

3. _buildRowメソッドの追加
    
    ```dart
    Widget _buildRow(WordPair pair) {
      return ListTile(
        title: Text(
          pair.asPascalCase,
          style: _biggerFont, 
        ),
      );
    }
    ```

4. RandomWordsStateの書き換え   
    RandomWordsStateクラスのbuildメソッドを以下のように書き換え、_buildSuggestionsメソッドを呼び出すように変更してください。   
    ついでにAppBarの定義も行い、MyAppクラスで定義していたヘッダーも移動させてしまいます。
    ```dart
    @override
    Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: Text('Startup Name Generator'),
        ),
        body: _buildSuggestions(),
      );
    }
    ```

5. MyAppの変更   
    MyAppクラスを以下のように変更します。
    ```dart
    class MyApp extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: 'Startup Name Generator',
          home: RandomWords()
        );
      }
    }
    ```

6. 実行   
ソースを保存しホットリロードが実行されることで、ソースが書き換わります。  
実際に作成されたサンプルを触って英文リストが永遠と作成されるListViewが作成されたのを確認してみてください。  
<img src="https://flutter.ctrnost.com/images/tutorial/06/01_ListView.png" width="600px"  alt="ListView">


## チュートリアル終了
最終的にはこんな感じのソースになっていると思います。  
チュートリアルはここで一旦終了です。  
それではFlutterで開発効率をあげて面白くて役に立つ新しいアプリをいっぱい作っていただければ幸いです。

```dart
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Startup Name Generator',
      home: RandomWords()
    );
  }
}

class RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];
  final _biggerFont = const TextStyle(fontSize: 18.0);
  @override
  Widget build(BuildContext context) {  return Scaffold(
    appBar: AppBar(
      title: Text('Startup Name Generator'),
    ),
    body: _buildSuggestions(),
  );
  }
  Widget _buildSuggestions() {
    return ListView.builder(
        padding: const EdgeInsets.all(16.0),
        itemBuilder: (context, i) {
          if (i.isOdd) return Divider();

          final index = i ~/ 2;
          if (index >= _suggestions.length) {
            _suggestions.addAll(generateWordPairs().take(10));
          }
          return _buildRow(_suggestions[index]);
        });
  }
  Widget _buildRow(WordPair pair) {
    return ListTile(
      title: Text(
        pair.asPascalCase,
        style: _biggerFont,
      ),
    );
  }
}

class RandomWords extends StatefulWidget {
  @override
  RandomWordsState createState() => new RandomWordsState();
}
```
