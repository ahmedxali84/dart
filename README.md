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












---------------x--------------------------x----------------------------------------x----------------------------------------x--------------




import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Trivia Quiz Game',
      theme: ThemeData.dark().copyWith(
        primaryColor: Colors.blue,
        scaffoldBackgroundColor: Colors.black,
        textTheme: TextTheme(
          bodyText1: TextStyle(fontSize: 18.0, color: Colors.white),
          bodyText2: TextStyle(fontSize: 18.0, color: Colors.white),
          headline6: TextStyle(fontSize: 24.0, fontWeight: FontWeight.bold),
        ),
        buttonTheme: ButtonThemeData(
          buttonColor: Colors.blue,
          textTheme: ButtonTextTheme.primary,
        ),
      ),
      home: HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Trivia Quiz Game'),
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            colors: [Colors.black, Colors.blue.shade700],
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
          ),
        ),
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              ElevatedButton(
                onPressed: () {
                  // Navigate to quiz screen
                  Navigator.push(
                    context,
                    MaterialPageRoute(builder: (context) => QuizScreen(category: "General Knowledge")),
                  );
                },
                child: Text('Start General Knowledge Quiz', style: TextStyle(fontSize: 20)),
              ),
              SizedBox(height: 20),
              ElevatedButton(
                onPressed: () {
                  // Navigate to quiz screen
                  Navigator.push(
                    context,
                    MaterialPageRoute(builder: (context) => QuizScreen(category: "Sports")),
                  );
                },
                child: Text('Start Sports Quiz', style: TextStyle(fontSize: 20)),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

class QuizScreen extends StatefulWidget {
  final String category;
  QuizScreen({required this.category});

  @override
  _QuizScreenState createState() => _QuizScreenState();
}

class _QuizScreenState extends State<QuizScreen> {
  int _currentQuestionIndex = 0;
  int _score = 0;
  final List<Map<String, Object>> _questions = [
    {
      'question': 'What is the capital of France?',
      'answers': ['Berlin', 'Madrid', 'Paris', 'Rome'],
      'correctAnswer': 'Paris',
    },
    {
      'question': 'Who wrote the play "Romeo and Juliet"?',
      'answers': ['Shakespeare', 'Dickens', 'Hemingway', 'Austen'],
      'correctAnswer': 'Shakespeare',
    },
    {
      'question': 'Which planet is known as the Red Planet?',
      'answers': ['Earth', 'Mars', 'Jupiter', 'Saturn'],
      'correctAnswer': 'Mars',
    },
    {
      'question': 'Who painted the Mona Lisa?',
      'answers': ['Van Gogh', 'Da Vinci', 'Picasso', 'Rembrandt'],
      'correctAnswer': 'Da Vinci',
    },
  ];

  void _answerQuestion(String answer) {
    if (answer == _questions[_currentQuestionIndex]['correctAnswer']) {
      setState(() {
        _score++;
      });
    }
    setState(() {
      if (_currentQuestionIndex < _questions.length - 1) {
        _currentQuestionIndex++;
      } else {
        Navigator.push(
          context,
          MaterialPageRoute(
            builder: (context) => ScoreScreen(score: _score),
          ),
        );
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.category),
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            colors: [Colors.black, Colors.blue.shade700],
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
          ),
        ),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Padding(
              padding: const EdgeInsets.all(20.0),
              child: Text(
                _questions[_currentQuestionIndex]['question'] as String,
                style: TextStyle(fontSize: 24, color: Colors.white),
                textAlign: TextAlign.center,
              ),
            ),
            ...(_questions[_currentQuestionIndex]['answers'] as List<String>)
                .map((answer) => ElevatedButton(
                      onPressed: () => _answerQuestion(answer),
                      child: Text(answer, style: TextStyle(fontSize: 18)),
                    ))
                .toList(),
          ],
        ),
      ),
    );
  }
}

class ScoreScreen extends StatelessWidget {
  final int score;
  ScoreScreen({required this.score});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Your Score'),
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            colors: [Colors.black, Colors.blue.shade700],
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
          ),
        ),
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Text(
                'Your Score: $score',
                style: TextStyle(fontSize: 30, color: Colors.white),
              ),
              SizedBox(height: 20),
              ElevatedButton(
                onPressed: () {
                  // Navigate back to home screen
                  Navigator.pop(context);
                },
                child: Text('Go Back', style: TextStyle(fontSize: 20)),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

