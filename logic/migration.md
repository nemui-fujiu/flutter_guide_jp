+++
title = "永続化データのマイグレーション"
date = 2019-09-31T00:00:00+09:00
draft = false
weight = 511
description = "sqfliteを使ったデータ永続化をする上で、アプリの更新時にマイグレーションが発生した場合にonUpgradeをどのように対処するかを詳しくみていきましょう。"
+++

sqfliteを使ったデータ永続化をする上で、アプリの更新時にマイグレーションが発生した場合にどのように対処するかを詳しくみていきましょう。

sqfliteによるデータ操作を確認したい人は[こちら](/logic/sqlite)を確認してください。

## マイグレーション

マイグレーションとは、プログラムやデータを変換することを言います。   
データベースに関するマイグレーションというと、データ構造を変更する場合に、更新したり、元に戻したりすることをさします。

## onCreate と onUpgrade

sqfliteでは``onCreate()``と``onUpgrade()``がマイグレーションとして使えます。

{{< highlight dart >}}
final Future<Database> database = openDatabase(
  join(await getDatabasesPath(), 'memo_database.db'),
  version: 1,
  onCreate: (Database db, int version) async {
    await db.execute("CREATE TABLE memo(id INTEGER PRIMARY KEY, text TEXT, priority INTEGER)");
  },
  onUpgrade: (Database db, int oldVersion, int newVersion) async {
    await db.execute("ALTER TABLE memo ADD COLUMN create_at TIMESTAMP DEFAULT (datetime(CURRENT_TIMESTAMP,'localtime'));");
  }
);

{{< /highlight >}}

onCreateは初回起動時に必ず実行されるSQLになります。   
migrationをして新たにSQLを発行したい場合は、onUpgradeに書いてください。   
上記の例の様にonUpgradeを書くとversionが1から2に上がった時に1度だけ実行されます。  

ですがこのままでは、さらにもう一つversionをあげたくなった場合に正しく動作しないため、onUpgradeの引数になっている、oldVersionと、newVersionを正しく判定してmigrationを行います。   
やり方は特に指定はなく、アプリ作者の想定通りにmigrationが実行できる様になっていれば問題ありません。   

以下にサンプルとしてversionの更新を意識したmigrationを記載しました。  

{{< highlight dart >}}
  const scripts = {
    '2' : ['ALTER TABLE memo ADD COLUMN create_at TIMESTAMP;'],
    '3' : ['ALTER TABLE memo ADD COLUMN update_at TIMESTAMP;'],
  };
  final database = openDatabase(
    join(await getDatabasesPath(), 'memo_database.db'),
    version: 3,
    onCreate: (db, version) {
      return db.execute(
        "CREATE TABLE memo(id INTEGER PRIMARY KEY, text TEXT, priority INTEGER)",
      );
    },
    onUpgrade: (Database db, int oldVersion, int newVersion) async {
      for (var i = oldVersion + 1; i <= newVersion; i++) {
        var queries = scripts[i.toString()];
        for (String query in queries) {
          await db.execute(query);
        }
      }
    }
  );
{{< /highlight >}}

この例では、scriptsにmigrationしたいSQLをversionごとに管理できる様に定義しています。   
構造としてはMapとListで構成しています。

{{< highlight dart >}}
  const scripts = {
    '2' : ['ALTER TABLE memo ADD COLUMN create_at TIMESTAMP;'],
    '3' : ['ALTER TABLE memo ADD COLUMN update_at TIMESTAMP;'],
  };
{{< /highlight >}}

最初の更新バージョンを1と仮定し、更新時に2のバージョンになったらcreate_atが追加されます。  
このとき複数のSQLを実施したい場合は、この配列に複数のSQLを書くことで、実行できる様にしてあります。

また、バージョン1からアプリを更新しておらず、バージョン3になるユーザも考慮して、以前インストールしたバージョン(oldVersion) + 1から今回インストールするバージョン(newVersion)までのSQLを実行する様にロジックを書いています。

{{< highlight dart >}}
  for (var i = oldVersion + 1; i <= newVersion; i++) {
    var queries = scripts[i.toString()];
    for (String query in queries) {
      await db.execute(query);
    }
  }
{{< /highlight >}}

この様にすることでmigrationが綺麗に実行されるはずです。

[Storage Classes and Datatypes](https://www.sqlite.org/datatype3.html)

## 参考

[Persist data with SQLite](https://flutter.dev/docs/cookbook/persistence/sqlite)
