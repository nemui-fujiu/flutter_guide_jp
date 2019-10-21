+++
title = "SQLiteでのデータ永続化"
date = 2019-07-12T00:00:00+09:00
draft = false
weight = 510
description = "Flutterを使ってDBへデータを保存しましょう。基本であるSQLiteでのデータ永続化をしてみましょう。"
featured_images = "/images/logic/db/db_sqlite.svg"
+++

<img src="/images/logic/db/db_sqlite_top.svg" width="90%" alt="body layout">

## データ保存

今回はFlutterでのデータ保存の方法を解説していきます。  
利用するのはSQLiteです。  
AndroidやiOSでアプリ開発をしたことがある方は馴染みがあると思いますが、アプリ開発をしていない人にとってはあまり聞きなれないかもしれません。  
SQLiteはアプリ向けのデータベースです。  

データベースについてやSQLite、SQLについてはここでは説明しませんので、ご自身で学習をおこなってください。

[データベース](https://ja.wikipedia.org/wiki/%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9)  
[SQLite](https://ja.wikipedia.org/wiki/SQLite)  
[SQL](https://ja.wikipedia.org/wiki/SQL)  

## SQLiteファイルの保存場所

実装をする前にまず知っておいて欲しいのはファイルの保存場所についてです。  
AndroidやiOSはそれぞれディレクトリ構成や名称に違いがあり、アプリケーションから操作できるディレクトリ名、それぞれの役割についても違います。  
このまま利用しようとすると、OSを判定して、それぞれ適切な場所にSQLiteの情報を作成する必要が出てきます。  
そのため、プラグインを使って解決します。

## 依存関係の追加

プラグインを追加するために``pubspec.yml``を編集しましょう。

- sqfliteパッケージはSQLiteデータベースとやり取りするためのクラスと関数を提供してくれます。
- pathパッケージは、データベースをディスクを格納する場所を定義する機能を提供してくれます。

{{< highlight yaml >}}
dependencies:
  flutter:
    sdk: flutter
  sqflite:
  path:
{{< /highlight >}}


## データモデルを定義する。

メモを保存するためのテーブルを作成する前に、保存する必要があるテーブルデータを定義してみましょう。

{{< highlight dart >}}
class Memo {
  final int id;
  final String text;
  final int priority;

  Memo({this.id, this.text, this.priority});
}
{{< /highlight >}}

## データベースに接続する。

データベースへの接続を定義しましょう。

1. ``getDatabasesPath()``でデータベースファイルを保存するパスを取得します。
2. ``openDatabase()``でデータベースに接続します。

{{< highlight dart >}}
final Future<Database> database = openDatabase(
  join(await getDatabasesPath(), 'memo_database.db'),
);
{{< /highlight >}}

## Memoテーブルの作成をする。

``openDatabase()``の第二引数に``onCreate()``を定義することで、SQLiteのテーブルを作成することができます。

{{< highlight dart >}}
final Future<Database> database = openDatabase(
  join(await getDatabasesPath(), 'memo_database.db'),
  onCreate: (db, version) {
    return db.execute(
      "CREATE TABLE memo(id INTEGER PRIMARY KEY, text TEXT, priority INTEGER)",
    );
  },
  version: 1,
);
{{< /highlight >}}

``openDatabase()``は他にも以下のようなことができます。

- ``onConfigure``では、SQLiteの設定を行うことができます。
- ``onCreate``では初期定義を行います。基本的にはこの中でテーブルを作成してください。
- ``onUpgrade``ではデータ定義の更新を行います。アプリリリース後にデータ定義を変更したいときに利用します。
- ``onDowngrade``ではデータ定義の更新を取り消すときに利用します。
- ``readOnly``ではデータベースを読み込み専用のデータとして利用したいときにtrueを設定してください。

※マイグレーションについては、[こちら](/logic/migration)を確認してください。

SQLiteで利用できるデータ型は以下を確認してください。
[Storage Classes and Datatypes](https://www.sqlite.org/datatype3.html)

## データの挿入

先ほど作ったMemoテーブルにデータを登録しましょう。

{{< highlight dart >}}

class Memo {
  final int id;
  final String text;
  final int priority;

  Memo({this.id, this.text, this.priority});

  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': text,
      'priority': priority,
    };
  }
}

Future<void> insertMemo(Memo memo) async {
  final Database db = await database;
  await db.insert(
    'memo',
    memo.toMap(),
    conflictAlgorithm: ConflictAlgorithm.replace,
  );
}

final todo = Memo(
  id: 0, 
  text: 'Flutterで遊ぶ', 
  priority: 1,
);

await insertMemo(todo);
{{< /highlight >}}

先ほど作ったMemoデータモデルにメソッドを追加します。

{{< highlight dart >}}
Map<String, dynamic> toMap() {
  return {
    'id': id,
    'name': text,
    'priority': priority,
  };
}
{{< /highlight >}}

これによりMemo型からMapに変換できるようになりました。

{{< highlight dart >}}
Future<void> insertMemo(Memo memo) async {
  final Database db = await database;
  await db.insert(
    'memo',
    memo.toMap(),
    conflictAlgorithm: ConflictAlgorithm.replace,
  );
}
{{< /highlight >}}

そして保存の処理はこのように書きます。
``openDatabase()``で作成したインスタンスに対して、``insert()``メソッドを使ってデータを保存します。


{{< highlight dart >}}
  await db.insert(
    'memo',
    memo.toMap(),
    conflictAlgorithm: ConflictAlgorithm.replace,
  );
{{< /highlight >}}

``insert()``では、対象のテーブル名、保存するデータのMap、コンフリクト時のアルゴリズムを指定しています。   
コンフリクト時のアルゴリズムとは、SQLiteが持っている機能で、INSERTやUPDATEのときにConflictが発生したらどのように振る舞うかを定義しておくことができる機能です。  
この機能は多くのSQLでは取り扱っていないSQLite特有の非標準の拡張です。   

以下のような種類があります。  

|ConflictAlgorithm|SQLに付与されるコマンド|詳細|
|---|---|---|
|rollback|OR ROLLBACK|SQLステートメントを中止して現在のトランザクションをロールバックします。|
|abort|OR ABORT|SQLステートメントを中止してSQLステートメントによって行われた変更を取り消します。同じトランザクション内の前の変更は保存され、トランザクションもアクティブなままになります。SQLiteではデフォルトの動作としてこのような動きをするので注意が必要です。|
|fail|OR FAIL|SQLステートメントを中止しますが以前の変更を取り消すこともトランザクションを終了することもしません。|
|ignore|OR IGNORE|SQLステートメントを中止しますが後続行の処理を、何も問題がないように継続します。|
|replace|OR REPLACE|SQLステートメントと競合している対象レコードを削除し、SQLステートメントを実行します。|

詳細な仕様については以下を確認してください。  
[Conflict](https://sqlite.org/lang_conflict.html)

``insert()``メソッドを利用せずに直接SQLを書きたい場合は以下のように書くことができます。

{{< highlight dart >}}
await db.rawInsert('INSERT INTO memo (id, text, priority) VALUES (?, ? , ?)', [memo.id, memo.text, memo.priority]);
{{< /highlight >}}

ただし先ほどのようにConflictAlgorithmを設定することはできません。

## データの取得

先ほど作成したデータを取得できるようにしましょう。

{{< highlight dart >}}
Future<List<Memo>> getMemos() async {
  final Database db = await database;
  final List<Map<String, dynamic>> maps = await db.query('memo');
  return List.generate(maps.length, (i) {
    return Memo(
      id: maps[i]['id'],
      text: maps[i]['text'],
      priority: maps[i]['priority'],
    );
  });
}
{{< /highlight >}}

まずは以下のようにqueryを定義します。

{{< highlight dart >}}
final List<Map<String, dynamic>> maps = await db.query('memo');
{{< /highlight >}}

今回は全件取得するqueryのため何も条件をつけていませんが、通常であれば検索条件が必要です。  
検索条件は以下のものが設定できます。  
SQLを理解していれば内容は全て理解できると思うので説明は省略します。  

{{< highlight dart >}}
{
  bool distinct,
  List<String> columns,
  String where,
  List<dynamic> whereArgs,
  String groupBy,
  String having,
  String orderBy,
  int limit,
  int offset
}
{{< /highlight >}}

条件の指定の例だけは載せておきます。

{{< highlight dart >}}
final id = 1;
await db.query('memo', where: 'id = ?', whereArgs: [id]);
{{< /highlight >}}

複数の条件を指定したい場合は通常のSQLと同じでANDやORで繋いでください。  
また、このような書き方以外にも以下のようにも書くことができます。  

{{< highlight dart >}}
await db.rawQuery('SELECT * FROM memo WHERE id = ?', [id]);
{{< /highlight >}}

そして実行した戻り値をMemo型のリストに詰め直して返して完了です。

{{< highlight dart >}}
  return List.generate(maps.length, (i) {
    return Memo(
      id: maps[i]['id'],
      text: maps[i]['text'],
      priority: maps[i]['priority'],
    );
  });
{{< /highlight >}}

LIKE句を使いたい場合は以下のように書きます。

{{< highlight dart >}}
final id = 1;
await db.query('memo', where: 'id LIKE ?', whereArgs: ['$id%']); // 1から始まるidにマッチする。
{{< /highlight >}}

## データの更新

データの更新は以下のように行います。

{{< highlight dart >}}
Future<void> updateMemo(Memo memo) async {
  // Get a reference to the database.
  final db = await database;
  await db.update(
    'memo',
    memo.toMap(),
    where: "id = ?",
    whereArgs: [memo.id],
    conflictAlgorithm: ConflictAlgorithm.fail,
  );
}
{{< /highlight >}}

以下のように書くことで、データの更新が行えます。

{{< highlight dart >}}
  await db.update(
    'memo',
    memo.toMap(),
    where: "id = ?",
    whereArgs: [memo.id],
    conflictAlgorithm: ConflictAlgorithm.fail,
  );
{{< /highlight >}}

書き方はINSERTの時とほぼ一緒で、``where``と``whereArgs``によって更新対象条件の絞り込みを行うことができます。

## データの削除

データ削除は以下のように行います。

{{< highlight dart >}}
Future<void> deleteMemo(int id) async {
  final db = await database;
  await db.delete(
    'memo',
    where: "id = ?",
    whereArgs: [id],
  );
}
{{< /highlight >}}

以下のように書くことで、データの削除が行えます。

{{< highlight dart >}}
  await db.delete(
    'memo',
    where: "id = ?",
    whereArgs: [id],
  );
{{< /highlight >}}

書き方は UPDATEの時とほぼ一緒です、``where``と``whereArgs``によって更新対象条件の絞り込みを行うことができます。  
ただし、SQLiteの仕様上、ConflictAlgorithmを指定することはできません。

## 最後に

データの活用はアプリの機能の幅を広げるために必須なので、書き方をしっかり覚えておきましょう。 
ただ、SQLの取り扱いはセキュリティの状態を左右するためセキュリティ知識も必要です。  
<div style="background-color:#ffeeba;padding:20px;color:#0c5460;border-radius:10px;">
<span>
INSERT、SELECT、UPDATE、DELETEそれぞれについて説明しましたが、SQLに値を渡すときは"id = ${memo.id}"のように渡すようなことはしないでください。
必ずプリペアードステートメントで記載してください。<br/>
"id = ${memo.id}"のような文字列補間を行うとSQLインジェクション攻撃が行えてしまうため、セキュリティ状態が極めて悪いアプリとなってしまいます。
</span>
</div>

## 例

{{< highlight dart >}}
import 'dart:async';

import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';

void main() async {
  final database = openDatabase(
    join(await getDatabasesPath(), 'memo_database.db'),
    onCreate: (db, version) {
      return db.execute(
        "CREATE TABLE memo(id INTEGER PRIMARY KEY, text TEXT, priority INTEGER)",
      );
    },
    version: 1,
  );

  Future<void> insertMemo(Memo memo) async {
    final Database db = await database;
    await db.insert(
      'memo',
      memo.toMap(),
      conflictAlgorithm: ConflictAlgorithm.replace,
    );
  }

  Future<List<Memo>> getMemos() async {
    final Database db = await database;
    final List<Map<String, dynamic>> maps = await db.query('memo');
    return List.generate(maps.length, (i) {
      return Memo(
        id: maps[i]['id'],
        text: maps[i]['text'],
        priority: maps[i]['priority'],
      );
    });
  }

  Future<void> updateMemo(Memo memo) async {
    // Get a reference to the database.
    final db = await database;
    await db.update(
      'memo',
      memo.toMap(),
      where: "id = ?",
      whereArgs: [memo.id],
      conflictAlgorithm: ConflictAlgorithm.fail,
    );
  }

  Future<void> deleteMemo(int id) async {
    final db = await database;
    await db.delete(
      'memo',
      where: "id = ?",
      whereArgs: [id],
    );
  }

  var memo = Memo(
    id: 0,
    text: 'Flutterで遊ぶ',
    priority: 1,
  );

  await insertMemo(memo);

  print(await getMemos());

  memo = Memo(
    id: memo.id,
    text: memo.text,
    priority: memo.priority + 1,
  );
  await updateMemo(memo);

  // Print Fido's updated information.
  print(await getMemos());

  // Delete Fido from the database.
  await deleteMemo(memo.id);

  // Print the list of dogs (empty).
  print(await getMemos());
}

class Memo {
  final int id;
  final String text;
  final int priority;

  Memo({this.id, this.text, this.priority});

  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': text,
      'priority': priority,
    };
  }
  @override
  String toString() {
    return 'Memo{id: $id, tet: $text, priority: $priority}';
  }
}
{{< /highlight >}}



## 参考

[Persist data with SQLite](https://flutter.dev/docs/cookbook/persistence/sqlite)
