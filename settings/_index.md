+++
title = "設定"
date = 2019-02-07T00:00:00+09:00
draft = false
weight = 999
description = "Flutterで必要な設定周りをまとめて記載しておきます、必要に応じて確認してもらえればと思います。"
keywords = "Flutter,アプリ,日本語,Settings,設定,pubspec.yml,yaml"
+++

## 設定


### 画像の読み込み

画像の読み込みをしたい場合はassetsの定義が必要です。   
``pubspec.yml``にassetsを設定するとImage.assetで読み込めるようになります。

```yaml
flutter:
  assets:
  - assets/logo.png
```

```dart
Image.asset(
    'assets/logo.png',
    fit: BoxFit.contain,
    height: 32,
),
```
