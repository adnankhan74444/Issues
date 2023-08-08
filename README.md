import 'dart:convert';
import 'package:flutter/material.dart';
import 'package:csv/csv.dart'; // Import the CSV parsing library

class Base64Table extends StatefulWidget {
  final String base64Data;

  Base64Table(this.base64Data);

  @override
  _Base64TableState createState() => _Base64TableState();
}

class _Base64TableState extends State<Base64Table> {
  List<List<dynamic>> tableData = [];

  @override
  void initState() {
    super.initState();
    _decodeBase64();
  }

  void _decodeBase64() {
    List<int> decodedData = base64Decode(widget.base64Data);
    String content = utf8.decode(decodedData);
    List<List<dynamic>> parsedCsv = CsvToListConverter().convert(content);
    setState(() {
      tableData = parsedCsv;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Base64 Table'),
      ),
      body: SingleChildScrollView(
        scrollDirection: Axis.horizontal,
        child: DataTable(
          columns: _getTableColumns(),
          rows: _getTableRows(),
        ),
      ),
    );
  }

  List<DataColumn> _getTableColumns() {
    if (tableData.isEmpty) {
      return [];
    }
    return tableData[0].map<DataColumn>((item) {
      return DataColumn(
        label: Text(item.toString()),
      );
    }).toList();
  }

  List<DataRow> _getTableRows() {
    if (tableData.isEmpty) {
      return [];
    }
    List<DataRow> rows = [];
    for (int i = 1; i < tableData.length; i++) {
      List<DataCell> cells = tableData[i].map<DataCell>((item) {
        return DataCell(
          Text(item.toString()),
        );
      }).toList();
      rows.add(DataRow(cells: cells));
    }
    return rows;
  }
}

void main() {
  runApp(MaterialApp(
    home: Base64Table('base64_encoded_data_here'), // Replace with your base64-encoded data
  ));
}
