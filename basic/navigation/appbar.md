+++
title = "AppBar"
date = 2019-03-09T00:00:00+09:00
draft = false
weight = 351
description = "一番基本のヘッダーになります。アプリで実装するよくあるヘッダー機能を網羅しているので、アプリヘッダーの実装が簡単に行えます。"
+++

## AppBar

一番基本のヘッダーになります。  
アプリで実装するよくあるヘッダー機能を網羅しているので、アプリヘッダーの実装が簡単に行えます。  

{{< highlight dart>}}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          leading: Icon(Icons.menu),
          title: const Text('AppBar'),
          backgroundColor: Colors.orange,
          centerTitle: true,
          actions: <Widget>[
            IconButton(
              icon: Icon(Icons.face, color: Colors.white,),
            ),
            IconButton(
              icon: Icon(Icons.email, color: Colors.white,),
            ),
            IconButton(
              icon: Icon(Icons.favorite, color: Colors.white,),
            ),
          ],
        ),
      ),
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/navigation/01/appbar_01.png" style="min-width:300px;max-width:600px;" alt="AppBar"/>

- ``leading``左端にアイコンの設定などを行います。
- ``title``タイトル
- ``centerTitle``タイトルをセンター寄せにするかを選択します。
- ``backgroundColor``背景色の変更ができます。
- ``actions``アクションボタンを配置するために利用します。   
``IconButton``や``PopupMenuButton``などを利用することでヘッダーにボタンを配置することができます。   

IconButtonについては[こちら](/basic/interactive/form/button/#icon_button)   
PopupMenuButtonについては[こちら](/basic/interactive/form/button/#popup_menu_button)






