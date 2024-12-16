# dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData.dark(),
      home: WeatherScreen(),
    );
  }
}

class WeatherScreen extends StatefulWidget {
  @override
  _WeatherScreenState createState() => _WeatherScreenState();
}

class _WeatherScreenState extends State<WeatherScreen>
    with TickerProviderStateMixin {
  // Animation controllers for fade, scale, and rotation
  late AnimationController _fadeController;
  late AnimationController _scaleController;
  late AnimationController _rotateController;
  late Animation<double> _fadeAnimation;
  late Animation<double> _scaleAnimation;
  late Animation<double> _rotateAnimation;

  // For hover effects
  bool _isHovered = false;

  @override
  void initState() {
    super.initState();

    // Fade animation (appears on screen)
    _fadeController = AnimationController(
      duration: Duration(seconds: 1),
      vsync: this,
    );
    _fadeAnimation = Tween<double>(begin: 0, end: 1).animate(
      CurvedAnimation(parent: _fadeController, curve: Curves.easeInOut),
    );

    // Scale animation (text grows/shrinks)
    _scaleController = AnimationController(
      duration: Duration(seconds: 1),
      vsync: this,
    );
    _scaleAnimation = Tween<double>(begin: 1, end: 1.2).animate(
      CurvedAnimation(parent: _scaleController, curve: Curves.easeInOut),
    );

    // Slow rotation animation (slower sun rotation)
    _rotateController = AnimationController(
      duration: Duration(seconds: 10), // Increased duration for slow motion
      vsync: this,
    );
    _rotateAnimation = Tween<double>(begin: 0, end: 2 * 3.14159).animate(
      CurvedAnimation(parent: _rotateController, curve: Curves.linear),
    );

    // Start fade-in animation when the widget loads
    _fadeController.forward();
    _rotateController.repeat(); // Repeat rotation for the weather icon
  }

  @override
  void dispose() {
    _fadeController.dispose();
    _scaleController.dispose();
    _rotateController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Weather App'),
        backgroundColor: Colors.black,
      ),
      body: Stack(
        children: [
          // Background Gradient with Clouds
          Container(
            decoration: BoxDecoration(
              gradient: LinearGradient(
                begin: Alignment.topLeft,
                end: Alignment.bottomRight,
                colors: [Colors.blue, Colors.black], // Blue to black gradient
              ),
            ),
          ),
          // Cloud images in the background
          Positioned(
            top: 50,
            left: 20,
            child: Image.asset(
              'assets/cloud1.png', // Add cloud image in assets folder
              width: 150,
              height: 80,
              color: Colors.white.withOpacity(0.3),
            ),
          ),
          Positioned(
            top: 150,
            right: 40,
            child: Image.asset(
              'assets/cloud2.png', // Another cloud image
              width: 180,
              height: 100,
              color: Colors.white.withOpacity(0.3),
            ),
          ),
          Positioned(
            bottom: 50,
            left: 100,
            child: Image.asset(
              'assets/cloud3.png', // Another cloud image
              width: 200,
              height: 120,
              color: Colors.white.withOpacity(0.3),
            ),
          ),
          // Main weather card
          Center(
            child: MouseRegion(
              onEnter: (_) {
                setState(() {
                  _isHovered = true; // When hover begins, change hover state
                  _scaleController.forward(); // Scale animation on hover
                });
              },
              onExit: (_) {
                setState(() {
                  _isHovered = false; // When hover ends, revert hover state
                  _scaleController.reverse(); // Reverse scale animation
                });
              },
              child: AnimatedOpacity(
                opacity: _fadeAnimation.value, // Apply fade-in effect
                duration: Duration(seconds: 1),
                child: Container(
                  width: 300,
                  height: 200,
                  decoration: BoxDecoration(
                    color: Colors.black87, // Dark background color
                    borderRadius: BorderRadius.circular(20), // Rounded corners
                    border: Border.all(
                      color: Colors.transparent, // Transparent border
                      width: 2,
                    ),
                    boxShadow: [
                      BoxShadow(
                        color: Colors.blueAccent
                            .withOpacity(0.5), // Blue shadow color
                        offset: Offset(0, 4),
                        blurRadius: 10,
                      ),
                    ],
                  ),
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    crossAxisAlignment: CrossAxisAlignment.center,
                    children: [
                      RotationTransition(
                        turns:
                            _rotateAnimation, // Apply rotation animation to the icon
                        child: Icon(
                          Icons.wb_sunny,
                          color: Colors.yellow,
                          size: 60,
                        ),
                      ),
                      SizedBox(height: 10),
                      ScaleTransition(
                        scale:
                            _scaleAnimation, // Apply scale animation to the temperature
                        child: Text(
                          '25°C',
                          style: TextStyle(
                            fontSize: 40,
                            fontWeight: FontWeight.bold,
                            color: Colors.white,
                          ),
                        ),
                      ),
                      SizedBox(height: 5),
                      Text(
                        'Sunny',
                        style: TextStyle(
                          fontSize: 20,
                          color: Colors.white,
                        ),
                      ),
                    ],
                  ),
                ),
              ),
            ),
          ),
        ],
      ),
    );
  }
}
