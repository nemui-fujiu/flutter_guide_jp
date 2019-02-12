+++
title = "スペシャル"
date = 2019-02-07T00:00:00+09:00
draft = false
weight = 34
description = "スペシャルで特殊系なヘッダーが思いついたら、のっけていきます。"
keywords = "Flutter,アプリ,日本語,インストール,install,ヘッダー,AppBar,Special,透過"
+++

スペシャルで特殊系なヘッダーが思いついたら、のっけていきます。



## 透過ヘッダー

---

<img src="http://flutter.ctrnost.com/images/layout/header/special/transparent.png" style="min-width:300px" alt="icon header" />

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context)
  {
    return new MaterialApp(
      title: 'Flutter Demo',
      home: Stack(
        children: <Widget>[
          new Container(
            height: double.infinity,
            width: double.infinity,
            decoration:new BoxDecoration(
              image: new DecorationImage(
                image: new AssetImage("assets/background.png"),
                fit: BoxFit.cover,
              ),
            ),
          ),
          Scaffold(
            backgroundColor: Colors.transparent,
            appBar: new AppBar(
              title: const Text("Standard AppBar"),
              backgroundColor: Colors.transparent,
              elevation: 0.0,
              actions: <Widget>[
                IconButton(
                  icon: Icon(Icons.settings),
                  onPressed: () {
                    // Pressed Action
                  },
                ),
                IconButton(
                  icon: Icon(Icons.menu),
                  onPressed: () {
                    // Pressed Action
                  },
                ),
              ],
            ),
            body: new Container(
              color: Colors.transparent,
            ),
          ),
        ],
      )
    );
  }
}
```

※イメージの読み込み設定をしていない場合は[こちら](/settings/)
