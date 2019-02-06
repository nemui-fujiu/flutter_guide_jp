# インストール

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

```
$ cd ~/development
$ unzip ~/Downloads/flutter_macos_v1.0.0-stable.zip
```
※ unzip対象はダウンロードしたFlutterのファイルです。

解凍できたら、PATHを通してコマンドが利用できるようにしましょう。

```
$ export PATH="$PATH:`pwd`/flutter/bin"
```

実際にコマンドを実行して動作するのを確認できたら完了です。

```
$ flutter --version
Flutter 1.0.0 • channel stable • https://github.com/flutter/flutter.git
Framework • revision 5391447fae (10 weeks ago) • 2018-11-29 19:41:26 -0800
Engine • revision 7375a0f414
Tools • Dart 2.1.0 (build 2.1.0-dev.9.4 f9ebf21297)
```

このままだと、一時的な登録となってしまうので、永続的な登録をするのであれば、```.bash_profile```に登録しましょう。

```~/.bash_profile
export PATH="$PATH:{※解凍したディレクトリ}/flutter/bin"
```
※ 「{※解凍したディレクトリ}」を実際のパスと置き換えてください。

### 2. Flutterの実行環境を整える。

FlutterはAndoird、iOSの2つのアプリを一括で作るため、両方の環境が必要になります。
実行環境を整えるのに便利なコマンドがFlutterにはあるのでそちらを利用して整えていきましょう。  
コマンドは以下になります。

```
$ fluttter doctor
```

実際のコマンドを叩くとFlutterでの開発に必要な情報を収集して診断してくれます。




## 参考
[Flutter Install](https://flutter.io/docs/get-started/install)

[Flutter](https://www.youtube.com/watch?time_continue=3&v=fq4N0hgOWzU)