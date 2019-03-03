+++
title = "Stack"
date = 2019-02-26T00:00:00+09:00
draft = false
weight = 317
description = "Stackはウィジェットを重ねることができるウィジェットです。テキストと画像、ボタンを重ねるなど、ウィジェットを複数重ねた複雑なレイアウトを簡単に作ることも可能です。"
+++

## Stack

Stackはウィジェットを重ねることができるウィジェットです。  
テキストと画像、ボタンを重ねるなど、ウィジェットを複数重ねた複雑なレイアウトを簡単に作ることも可能です。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            appBar: AppBar(
              title: Text('Stack'),
            ),
            body: Stack(
              children: <Widget>[
                Container(
                  width: 100,
                  height: 100,
                  color:Colors.green
                ),
                Container(
                  width: 50,
                  height: 80,
                  color: Colors.orange,
                )
              ],
            )
        )
    );
  }
}
```

<img src="/images/basic/layout/07/stack_01.png" style="min-width:300px;max-width:600px;" alt="Stack"/>

このように、ウィジェットを重ねることが可能です。

### Positioned

``Positioned``を使うことでウィジェットを好きなところに配置することが可能です。

```dart
  children: <Widget>[
    Positioned(
      left: 20.0,
      top: 20.0,
      width: 300.0,
      height: 300.0,
      child: Container(color: Colors.blue,),
    ),
    Positioned(
      left: 10.0,
      top: 10.0,
      width: 100.0,
      height: 100.0,
      child: Container(color: Colors.green,),
    ),
    Positioned(
      left: 120.0,
      top: 120.0,
      width: 100.0,
      height: 100.0,
      child: Container(color: Colors.orange,),
    ),
  ],
```

<img src="/images/basic/layout/07/stack_02.png" style="min-width:300px;max-width:600px;" alt="Stack Positioned"/>

位置の指定は、``left``,``right``,``top``,``bottom``にて配置したいところへ指定できます。

### Aligment

```dart
body: Stack(
  alignment: Alignment.bottomRight,
  children: <Widget>[
    SizedBox(
      width: 400.0,
      height: 400.0,
      child: Container(color: Colors.orange,),
    ),
    Positioned(
      left: 20.0,
      top: 20.0,
      width: 300.0,
      height: 300.0,
      child: Container(color: Colors.blue,),
    ),
    Positioned(
      left: 10.0,
      top: 10.0,
      width: 100.0,
      height: 100.0,
      child: Container(color: Colors.green,),
    ),
    Positioned(
      left: 120.0,
      top: 120.0,
      width: 100.0,
      height: 100.0,
      child: Container(color: Colors.orange,),
    ),
    Text('Test')
  ],
)
```

<img src="/images/basic/layout/07/stack_03.png" style="min-width:300px;max-width:600px;" alt="Stack Alignment"/>

``SizedBox``は指定した幅と高さ持つことができるウィジェットです。  
``alignment``は、positionedウィジェット以外のウィジェットに対して場所の指定ができます。    
位置の指定は以下の通りになります。

||||
|---|---|---|
|topLeft|topCenter|topRight|
|centerLeft|center|centerRight|
|bottomLeft|bottomCenter|bottomRight|
