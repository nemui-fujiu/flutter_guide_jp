+++
title = "アプリの実行"
date = 2019-02-08T00:00:00+09:00
draft = false
weight = 22
description = "アプリを実行してエミュレータで動作確認をしてみましょう。ホットリロード機能のおかげで開発もサクサク進みます。"
keywords = "Flutter,アプリ,チュートリアル,tutorial,入門,動作確認,ホットリロード,Hot reload"
+++

それではアプリを実行してみましょう。

## 起動

1. Android Studioのメインツールバーの以下部分で、エミュレータを選択してください。
<img src="https://flutter.ctrnost.com/images/tutorial/02/01_emulator.png" width="400px" alt="choice emulator">
2. エミュレータが立ち上がったらアプリケーションを実行してみましょう。
<img src="https://flutter.ctrnost.com/images/tutorial/02/02_app_build.png" width="400px" alt="ranable app">

## ホットリロード

ホットリロードとは、アプリを再起動せず、修正を反映させる機能のことを言います。
Flutterはデフォルトでホットリロードが有効になっているので、ソースの変更を保存するとエミュレータですぐに確認することが可能です。

1. ``lib/main.dart``を開いてください。
2. 以下部分を編集します。

    ```dart
    Text(
      'You have pushed the button this many times:',
    ),
    ```
    ↓
    ```dart
    Text(
      'ボタンを押した回数:',
    ),
    ```
    
3. ソースを保存するとエミュレータの文字列が書き換わったのがわかると思います。   
Before:   
<img src="https://flutter.ctrnost.com/images/tutorial/02/03_before.png" width="400px" alt="before">   
After:  
<img src="https://flutter.ctrnost.com/images/tutorial/02/04_after.png" width="400px" alt="after">
