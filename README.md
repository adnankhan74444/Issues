yimport 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Two-Column List')),
        body: MyTwoColumnList(),
      ),
    );
  }
}

class MyTwoColumnList extends StatelessWidget {
  final List<int> items = List.generate(10, (index) => index + 1);

  @override
  Widget build(BuildContext context) {
    return SingleChildScrollView(
      child: Column(
        children: [
          for (int i = 0; i < items.length; i += 2)
            Row(
              children: [
                Expanded(
                  child: Container(
                    margin: EdgeInsets.all(8.0),
                    color: Colors.blueGrey,
                    height: 100.0,
                    alignment: Alignment.center,
                    child: Text(items[i].toString()),
                  ),
                ),
                if (i + 1 < items.length)
                  Expanded(
                    child: Container(
                      margin: EdgeInsets.all(8.0),
                      color: Colors.blueGrey,
                      height: 100.0,
                      alignment: Alignment.center,
                      child: Text(items[i + 1].toString()),
                    ),
                  ),
              ],
            ),
        ],
      ),
    );
  }
}
