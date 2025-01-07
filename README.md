# **Onboarding Documentation for Flutter Development at Digital Natives**

---

## **1. Setting Up Your Environment**

### **1.1. Install Flutter**
#### Steps:
1. **Download the Flutter SDK**: Get it from [Flutter.dev](https://flutter.dev).
2. **Extract the SDK**: Place the extracted folder in a location like:
   - **Windows**: `C:\flutter`
   - **macOS/Linux**: `/Users/<username>/flutter`
3. **Add to PATH**:
   - **Windows**: Edit environment variables and add `C:\flutter\bin`.
   - **macOS/Linux**: Add to `.zshrc` or `.bashrc`:
     ```bash
     export PATH="$PATH:/path/to/flutter/bin"
     ```
4. **Verify Installation**:
   ```bash
   flutter doctor
   ```
   - **Text-based diagram placeholder**:
     ```
     Terminal Output:
     [✓] Flutter (Channel stable, 3.x.x, ...)
     [✓] Android toolchain
     [✓] Xcode (if on macOS)
     ```

---

### **1.2. Install Required Software**
#### Tools:
1. **Android Studio**:
   - Install from [Android Studio](https://developer.android.com/studio).
   - Install **Android SDK**, **Emulator**, and **HAXM** (if Intel CPU).
2. **VS Code**:
   - Install the Flutter and Dart extensions.
3. **Xcode** (macOS): Install via App Store for iOS.

---

## **2. Creating a Project**

1. Create a project:
   ```bash
   flutter create my_project
   ```
2. Navigate to the project:
   ```bash
   cd my_project
   ```
3. Open in IDE.

### **Text-based project diagram:**
```text
my_project/
|-- android/           # Native Android code
|-- ios/               # Native iOS code
|-- lib/               # Flutter codebase
    |-- main.dart      # Entry point
|-- pubspec.yaml       # Dependency file
|-- test/              # Unit tests
```

---

## **3. Folder Structure**

### **3.1. Model-View-Presenter (MVP)**
This structure separates UI, logic, and data models.

#### Text-based diagram:
```text
lib/
|-- models/             # Data models (e.g., User, Product)
|-- views/              # UI Widgets (e.g., screens, buttons)
|-- presenters/         # Business logic (state handling)
|-- services/           # External services (API/Firebase)
|-- utils/              # Helper functions
|-- main.dart           # Entry point
```

#### Example:
- **Models**:
   ```dart
   class Product {
     final String id;
     final String name;
     final double price;

     Product({required this.id, required this.name, required this.price});
   }
   ```
- **Views**:
   ```dart
   class ProductView extends StatelessWidget {
     @override
     Widget build(BuildContext context) {
       return Scaffold(
         body: Text("Product Name"),
       );
     }
   }
   ```
- **Presenters**:
   ```dart
   class ProductPresenter {
     void loadProducts() {
       // Logic to fetch and update products
     }
   }
   ```

---

### **3.2. Model-View-ViewModel (MVVM)**
The MVVM structure integrates well with state management tools like Provider or Riverpod.

#### Text-based diagram:
```text
lib/
|-- models/             # Data models
|-- views/              # UI components
|-- viewmodels/         # State and logic for views
|-- services/           # External services
|-- utils/              # Helpers
|-- main.dart           # Entry point
```

#### Example:
- **ViewModel**:
   ```dart
   class CounterViewModel with ChangeNotifier {
     int _counter = 0;

     int get counter => _counter;

     void increment() {
       _counter++;
       notifyListeners();
     }
   }
   ```

---

### **3.3. Feature-Based Structure**
Organizes by feature, excellent for scaling.

#### Text-based diagram:
```text
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
```

---

## **4. State Management Options**

### **4.1. Provider**
A lightweight and reactive solution.

#### Text-based diagram:
```text
View --> ViewModel (via Provider) --> Notifies UI
```

#### Example:
- **ViewModel**:
   ```dart
   class CounterProvider with ChangeNotifier {
     int _counter = 0;
     int get counter => _counter;

     void increment() {
       _counter++;
       notifyListeners();
     }
   }
   ```

---

### **4.2. Riverpod**
Improved state management with dependency injection.

#### Text-based diagram:
```text
StateProvider --> Consumers access using `ref.watch`
```

#### Example:
- **Provider**:
   ```dart
   final counterProvider = StateProvider<int>((ref) => 0);
   ```

---

### **4.3. Bloc (Business Logic Component)**
Ideal for complex state and event handling.

#### Text-based diagram:
```text
Event --> Bloc --> State --> UI updates
```

#### Example:
- **Bloc**:
   ```dart
   class CounterBloc extends Bloc<CounterEvent, int> {
     CounterBloc() : super(0);

     @override
     Stream<int> mapEventToState(CounterEvent event) async* {
       if (event is IncrementEvent) {
         yield state + 1;
       }
     }
   }
   ```

---

## **5. API vs Firebase**

### **5.1. API**
1. Use APIs for flexibility.
2. Example folder structure:
   ```text
   lib/
   |-- services/
       |-- api_service.dart
   ```

#### Example Code:
   ```dart
   import 'dart:convert';
   import 'package:http/http.dart' as http;

   Future<void> fetchData() async {
     final response = await http.get(Uri.parse('https://api.example.com/data'));
     print(json.decode(response.body));
   }
   ```

---

### **5.2. Firebase**
1. Best for real-time data and authentication.
2. Example folder structure:
   ```text
   lib/
   |-- services/
       |-- firebase_service.dart
   ```

#### Example Code:
   ```dart
   import 'package:firebase_auth/firebase_auth.dart';

   Future<void> signIn(String email, String password) async {
     await FirebaseAuth.instance.signInWithEmailAndPassword(email: email, password: password);
   }
   ```

---

## **6. Common Issues and Solutions**

### **6.1. Flutter Doctor Errors**
#### Diagram:
```text
Error Message --> Suggested Resolution
```

- Example:
  - Error: `Android licenses not accepted`
  - Solution:
    ```bash
    flutter doctor --android-licenses
    ```

### **6.2. Emulator Issues**
- Ensure HAXM is installed (Intel).
- Diagram:
  ```text
  Issue: Emulator not starting --> Fix: Configure AVD manager
  ```


