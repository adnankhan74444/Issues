import 'package:flutter/material.dart';

class MyWidget extends StatefulWidget {
  final List<FocusNode> focusNodes;

  MyWidget({required this.focusNodes});

  @override
  _MyWidgetState createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  @override
  void initState() {
    super.initState();
    // Add a listener to each FocusNode instance to handle focus changes.
    for (var focusNode in widget.focusNodes) {
      focusNode.addListener(_handleFocusChange);
    }
  }

  @override
  void dispose() {
    // Remove the listeners to avoid memory leaks.
    for (var focusNode in widget.focusNodes) {
      focusNode.removeListener(_handleFocusChange);
      focusNode.dispose();
    }
    super.dispose();
  }

  void _handleFocusChange() {
    // Handle the focus change event here.
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: List.generate(9, (index) {
        return InkWell(
          onTap: () {
            // Request focus on the specified FocusNode when InkWell is tapped.
            widget.focusNodes[index].requestFocus();
          },
          child: Text('Item $index'),
        );
      }),
    );
  }
}

void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: MyWidget(
        focusNodes: List.generate(9, (index) => FocusNode()),
      ),
    ),
  ));
}
