+++
title = "DatePicker"
date = 2019-03-06T00:00:00+09:00
draft = false
weight = 329
description = "「DatePicker」は日付を選択するのにモバイルでは一般的なやり方です。今回は、日付、時間それぞれの設定方法についてみていきます。 "
+++

## DatePicker

「DatePicker」は日付を選択するのにモバイルでは一般的なやり方です。  
今回は、日付、時間それぞれの設定方法についてみていきます。

### DatePicker

「DatePicker」は日付を選択するモーダルを表示します。

{{< highlight dart>}}
class _ChangeFormState extends State<ChangeForm> {

  DateTime _date = new DateTime.now();

  Future<Null> _selectDate(BuildContext context) async {
    final DateTime picked = await showDatePicker(
        context: context,
        initialDate: _date,
        firstDate: new DateTime(2016),
        lastDate: new DateTime.now().add(new Duration(days: 360))
    );
    if(picked != null) setState(() => _date = picked);
  }

  Widget build(BuildContext context) {
    return Container(
        padding: const EdgeInsets.all(50.0),
        child: Column(
          children: <Widget>[
            Center(child:Text("${_date}")),
            new RaisedButton(onPressed: () => _selectDate(context), child: new Text('日付選択'),)
          ],
        )
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/interactive/02/datepicker_01.gif" style="min-width:300px;max-width:600px;" alt="DatePicker"/>


- ``context``には、モーダル表示のために``BuildContext``を渡してください。
- ``initialDate``は、最初に表示する初期の日付になります。
- ``firstDate``は、表示できる最小日付です。
- ``lastDate``は、表示できる最大日付です。

``initialDate``は``firstDate``から``lastDate``の範囲内の日付である必要があるので注意してください。

### DateTimePicker

「DateTimePicker」は時間を選択するモーダルを表示します。

{{< highlight dart>}}
class _ChangeFormState extends State<ChangeForm> {

  TimeOfDay _time = new TimeOfDay.now();

  Future<Null> _selectTime(BuildContext context) async {
    final TimeOfDay picked = await showTimePicker(
        context: context,
        initialTime: _time,
    );
    if(picked != null) setState(() => _time = picked);
  }

  Widget build(BuildContext context) {
    return Container(
        padding: const EdgeInsets.all(50.0),
        child: Column(
          children: <Widget>[
            Center(child:Text("${_time}")),
            new RaisedButton(onPressed: () => _selectTime(context), child: new Text('時間選択'),)
          ],
        )
    );
  }
}
{{< /highlight >}}

<img src="/images/basic/interactive/02/datetimepicker_01.gif" style="min-width:300px;max-width:600px;" alt="DateTimePicker"/>

- ``context``には、モーダル表示のために``BuildContext``を渡してください。
- ``initialDate``は、最初に表示する初期の時刻になります。
