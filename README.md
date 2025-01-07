Onboarding Documentation for Flutter Development at Digital Natives
1. Setting Up Your Environment
1.1. Install Flutter
Steps:
Download the Flutter SDK: Get it from Flutter.dev.
Extract the SDK: Place the extracted folder in a location like:
Windows: C:\flutter
macOS/Linux: /Users/<username>/flutter
Add to PATH:
Windows: Edit environment variables and add C:\flutter\bin.
macOS/Linux: Add to .zshrc or .bashrc:
bash
Copy code
export PATH="$PATH:/path/to/flutter/bin"
Verify Installation:
bash
Copy code
flutter doctor
Text-based diagram placeholder:
less
Copy code
Terminal Output:
[✓] Flutter (Channel stable, 3.x.x, ...)
[✓] Android toolchain
[✓] Xcode (if on macOS)
1.2. Install Required Software
Tools:
Android Studio:
Install from Android Studio.
Install Android SDK, Emulator, and HAXM (if Intel CPU).
VS Code:
Install the Flutter and Dart extensions.
Xcode (macOS): Install via App Store for iOS.
2. Creating a Project
Create a project:
bash
Copy code
flutter create my_project
Navigate to the project:
bash
Copy code
cd my_project
Open in IDE.
Text-based project diagram:
text
Copy code
my_project/
|-- android/           # Native Android code
|-- ios/               # Native iOS code
|-- lib/               # Flutter codebase
    |-- main.dart      # Entry point
|-- pubspec.yaml       # Dependency file
|-- test/              # Unit tests
3. Folder Structure
3.1. Model-View-Presenter (MVP)
This structure separates UI, logic, and data models.

Text-based diagram:
text
Copy code
lib/
|-- models/             # Data models (e.g., User, Product)
|-- views/              # UI Widgets (e.g., screens, buttons)
|-- presenters/         # Business logic (state handling)
|-- services/           # External services (API/Firebase)
|-- utils/              # Helper functions
|-- main.dart           # Entry point
Example:
Models:
dart
Copy code
class Product {
  final String id;
  final String name;
  final double price;

  Product({required this.id, required this.name, required this.price});
}
Views:
dart
Copy code
class ProductView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Text("Product Name"),
    );
  }
}
Presenters:
dart
Copy code
class ProductPresenter {
  void loadProducts() {
    // Logic to fetch and update products
  }
}
3.2. Model-View-ViewModel (MVVM)
The MVVM structure integrates well with state management tools like Provider or Riverpod.

Text-based diagram:
text
Copy code
lib/
|-- models/             # Data models
|-- views/              # UI components
|-- viewmodels/         # State and logic for views
|-- services/           # External services
|-- utils/              # Helpers
|-- main.dart           # Entry point
Example:
ViewModel:
dart
Copy code
class CounterViewModel with ChangeNotifier {
  int _counter = 0;

  int get counter => _counter;

  void increment() {
    _counter++;
    notifyListeners();
  }
}
3.3. Feature-Based Structure
Organizes by feature, excellent for scaling.

Text-based diagram:
text
Copy code
lib/
|-- features/
    |-- auth/
        |-- models/
        |-- views/
        |-- controllers/
    |-- home/
        |-- models/
        |-- views/
        |-- controllers/
|-- shared/              # Shared utilities
|-- main.dart
4. State Management Options
4.1. Provider
A lightweight and reactive solution.

Text-based diagram:
text
Copy code
View --> ViewModel (via Provider) --> Notifies UI
Example:
ViewModel:
dart
Copy code
class CounterProvider with ChangeNotifier {
  int _counter = 0;

  int get counter => _counter;

  void increment() {
    _counter++;
    notifyListeners();
  }
}
4.2. Riverpod
Improved state management with dependency injection.

Text-based diagram:
text
Copy code
StateProvider --> Consumers access using `ref.watch`
Example:
Provider:
dart
Copy code
final counterProvider = StateProvider<int>((ref) => 0);
4.3. Bloc (Business Logic Component)
Ideal for complex state and event handling.

Text-based diagram:
text
Copy code
Event --> Bloc --> State --> UI updates
Example:
Bloc:
dart
Copy code
class CounterBloc extends Bloc<CounterEvent, int> {
  CounterBloc() : super(0);

  @override
  Stream<int> mapEventToState(CounterEvent event) async* {
    if (event is IncrementEvent) {
      yield state + 1;
    }
  }
}
5. API vs Firebase
5.1. API
Use APIs for flexibility.
Example folder structure:
text
Copy code
lib/
|-- services/
    |-- api_service.dart
Example Code:
dart
Copy code
import 'dart:convert';
import 'package:http/http.dart' as http;

Future<void> fetchData() async {
  final response = await http.get(Uri.parse('https://api.example.com/data'));
  print(json.decode(response.body));
}
5.2. Firebase
Best for real-time data and authentication.
Example folder structure:
text
Copy code
lib/
|-- services/
    |-- firebase_service.dart
Example Code:
dart
Copy code
import 'package:firebase_auth/firebase_auth.dart';

Future<void> signIn(String email, String password) async {
  await FirebaseAuth.instance.signInWithEmailAndPassword(email: email, password: password);
}
6. Common Issues and Solutions
6.1. Flutter Doctor Errors
Diagram:
text
Copy code
Error Message --> Suggested Resolution
Example:
Error: Android licenses not accepted
Solution:
bash
Copy code
flutter doctor --android-licenses
6.2. Emulator Issues
Ensure HAXM is installed (Intel).
Diagram:
text
Copy code
Issue: Emulator not starting --> Fix: Configure AVD manager
