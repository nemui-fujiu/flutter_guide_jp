+++
title = "インストール"
date = 2019-02-07T00:00:00+09:00
draft = false
weight = 10
description = "Flutterをインストールしてアプリ開発を始メル準備を始めましょう。Flutterのインストールはとても簡単に行うことができます！"
keywords = "Flutter,アプリ,日本語,インストール,install,入門"
+++

それでは最初にFlutterのインストールから始めましょう。
環境によってインストール方法が異なるため、注意してください。

また、Windowsで開発する場合はiOSアプリの開発を行うことはできません。

## Mac

### インストールに必要な必須条件
- MacOS(64bit)
- ディスクスペースの空き容量が700MB以上あること
- 以下ツールがインストールされていること
  - bash
  - curl
  - git 2.x
  - mkdir
  - rm
  - unzip
  - which 

### 1. Flutterの入手

公式サイトからflutterをダウンロードしましょう。  
[https://flutter.io/docs/get-started/install/macos](https://flutter.io/docs/get-started/install/macos)

ダウンロードしたファイルを解凍して好きなところに配置してください。

{{< highlight bash >}}
$ cd ~/development
$ unzip ~/Downloads/flutter_macos_v1.0.0-stable.zip
{{< /highlight >}}
※ unzip対象はダウンロードしたFlutterのファイルです。

解凍できたら、PATHを通してコマンドが利用できるようにしましょう。

{{< highlight bash >}}
$ export PATH="$PATH:`pwd`/flutter/bin"
{{< /highlight >}}

実際にコマンドを実行して動作するのを確認できたら完了です。

{{< highlight bash >}}
$ flutter --version
Flutter 1.0.0 • channel stable • https://github.com/flutter/flutter.git
Framework • revision 5391447fae (10 weeks ago) • 2018-11-29 19:41:26 -0800
Engine • revision 7375a0f414
Tools • Dart 2.1.0 (build 2.1.0-dev.9.4 f9ebf21297)
{{< /highlight >}}

このままだと、一時的な登録となってしまうので、永続的な登録をするのであれば、``$HOME/.bash_profile``に登録しましょう。

{{< highlight bash >}}
export PATH="$PATH:[PATH_TO_FLUTTER_GIT_DIRECTORY]/flutter/bin"
{{< /highlight >}}
※ 「[PATH_TO_FLUTTER_GIT_DIRECTORY]」を実際のパスと置き換えてください。

### 2. Flutterの実行環境を整える。

FlutterはAndoird、iOSの2つのアプリを一括で作るため、両方の環境が必要になります。
実行環境を整えるのに便利なコマンドがFlutterにはあるのでそちらを利用して整えていきましょう。  
コマンドは以下になります。

{{< highlight bash >}}
$ fluttter doctor
{{< /highlight >}}

実際のコマンドを叩くとFlutterでの開発に必要な情報を収集して診断してくれます。

問題がある場合は太字で表示されるので、それぞれ解決していきましょう。

### 3. iOSセットアップ

1. Xcode9.0以降をインストールしてください。
2. 以下のように設定することで最新のXcodeが指定できます。
   
    {{< highlight bash >}}
    $ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
    {{< /highlight >}}   
    別のバージョンを利用したい場合は、パスを設定してください。  
3. Xcodeを開いて確認するか以下コマンドにて、仕様許諾に同意してください。

    {{< highlight bash >}}
    $ sudo xcodebuild -license
    {{< /highlight >}}

4. シミュレータを設定する。   
   以下のようにコマンドで起動するか、Spotlightなどでシミュレータを起動させます。
    {{< highlight bash >}}
    $ open -a Simulator
    {{< /highlight >}}

5. シミュレータを起動して**[ハードウェア]>[デバイス]**メニューの設定を確認して、シミュレータが64ビットデバイス(iPhone5s以降)を使用していることを確認してください。

### 4. Androidの設定

1. [Android Studio](https://developer.android.com/studio/)をインストールしてください。

2. Android Studioを起動して、「Android Studioセットアップウィザード」を実行します。   
   Flutterに必要な、最新のSDKをインストールしてください。

3. エミュレータを設定する。   
   エミュレータを起動するには**[Android Studio] > [Tools] > [Android] > [AVD Manager]**から**[Create Virtual Device]**を選択して新規に作成してください。   
   エミュレータを作成するときは**x86**または**x86_64**のイメージが推奨されています。


## 実機インストール

実機インストールについてはまた別の手順が必要なため、改めて設定してください。

## 参考

[Flutter Install](https://flutter.io/docs/get-started/install)

[Flutter](https://www.youtube.com/watch?time_continue=3&v=fq4N0hgOWzU)
