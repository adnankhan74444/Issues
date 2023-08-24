dimport 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Rotating Widget in Dialog')),
        body: Center(child: RotatingWidgetButton()),
      ),
    );
  }
}

class RotatingWidgetButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {
        showDialog(
          context: context,
          builder: (context) => AlertDialog(
            content: RotatingWidget(),
          ),
        );
      },
      child: Text('Open Dialog'),
    );
  }
}

class RotatingWidget extends StatefulWidget {
  @override
  _RotatingWidgetState createState() => _RotatingWidgetState();
}

class _RotatingWidgetState extends State<RotatingWidget>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  bool _isRotating = false;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      vsync: this,
      duration: Duration(seconds: 1),
    );
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        if (!_isRotating) {
          _controller.repeat();
        } else {
          _controller.stop();
        }
        setState(() {
          _isRotating = !_isRotating;
        });
      },
      child: RotationTransition(
        turns: _controller,
        child: Container(
          width: 100,
          height: 100,
          color: Colors.blue,
          child: Center(
            child: Text(
              'Tap to Rotate',
              style: TextStyle(color: Colors.white),
            ),
          ),
        ),
      ),
    );
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
}
