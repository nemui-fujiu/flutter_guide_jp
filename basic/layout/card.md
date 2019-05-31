+++
title = "Card"
date = 2019-02-26T00:00:00+09:00
draft = false
weight = 318
description = "Cardはマテリアルデザインを適応したウィジェットを作成するのに利用します。他のレイアウトと合わせて綺麗なレイアウトを作ることができます。"
+++

## Card

``Card``はマテリアルデザインを適応したウィジェットを作成するのに利用します。   
他のレイアウトと合わせて綺麗なレイアウトを作ることができます。

{{< highlight dart>}}
body: Card(
  margin: const EdgeInsets.all(50.0),
  child: Container(
    margin: const EdgeInsets.all(10.0),
    width: 300,
    height: 100,
    child: Text(
      'Card',
      style: TextStyle(fontSize: 30),
    )
  ),
)
{{< /highlight >}}

<img src="/images/basic/layout/08/card_01.png" style="min-width:300px;max-width:600px;" alt="Card"/>

このように簡単に綺麗なウィジェトを作成できるので、色々な場所で活用してみてください。
