import 'package:flutter/material.dart';

class HoverWidgetExample extends StatefulWidget {
  @override
  _HoverWidgetExampleState createState() => _HoverWidgetExampleState();
}

class _HoverWidgetExampleState extends State<HoverWidgetExample> {
  List<FocusNode> focusNodes = [];

  @override
  void initState() {
    super.initState();
    // Initialize focusNodes list and add listeners
    for (int i = 0; i < 5; i++) {
      var focusNode = FocusNode();
      focusNode.addListener(() {
        _handleFocusChange(i);
      });
      focusNodes.add(focusNode);
    }
  }

  @override
  void dispose() {
    // Dispose focus nodes and remove listeners when the widget is disposed
    for (var focusNode in focusNodes) {
      focusNode.removeListener(_handleFocusChange);
      focusNode.dispose();
    }
    super.dispose();
  }

  void _handleFocusChange(int index) {
    // Handle focus changes for a specific widget here
    print('Focus changed for widget at index $index');
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: HoverWidget(
          onHover: (isHovered) {
            // Handle hover state changes here
          },
          child: Column(
            children: List.generate(5, (index) {
              return InkWell(
                onTap: () {
                  // Handle tap event here
                  focusNodes[index].requestFocus();
                },
                child: ListTile(
                  title: TextField(
                    focusNode: focusNodes[index], // Associate FocusNode to the TextField
                    decoration: InputDecoration(labelText: 'Item $index'),
                  ),
                ),
              );
            }),
          ),
        ),
      ),
    );
  }
}
