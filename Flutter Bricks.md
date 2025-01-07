# **Comprehensive Guide to Mason and Bricks in Flutter**

---

## **1. What is Mason and Bricks?**
Mason is an open-source Dart package created by Felix Angelov that simplifies project scaffolding by generating files and directories from reusable templates called "bricks." It enhances developer productivity by maintaining consistency in code and reducing repetitive tasks.

Bricks are predefined templates that can include variables and logic for customization, making it easy to generate repetitive structures like widgets, tests, and more.

---

## **2. Prerequisites**

### **2.1. Dart and Flutter Environment**
- Ensure Dart and Flutter are installed and configured on your system. 
- Validate the installation using:
  ```bash
  flutter doctor
  ```

### **2.2. IDE Setup**
- Recommended IDE: **Visual Studio Code** or **Android Studio**.
- Install the Flutter and Dart extensions/plugins.

### **2.3. Homebrew (Optional)**
For macOS/Linux users, Homebrew can simplify installation and updates.
- Install Homebrew from [Homebrew's website](https://brew.sh/).

---

## **3. Installing Mason**

### **3.1. Using Dart CLI**
Run the following command to globally activate Mason CLI:
```bash
dart pub global activate mason_cli
```

### **3.2. Using Homebrew (macOS/Linux)**
1. Add the Mason tap:
   ```bash
   brew tap felangel/mason
   ```
2. Install Mason:
   ```bash
   brew install mason
   ```
3. Update Mason when necessary:
   ```bash
   brew update && brew upgrade mason
   ```

Verify installation using:
```bash
mason --version
```

---

## **4. Setting Up Mason in a Project**

### **4.1. Initialize Mason**
Navigate to your project directory and run:
```bash
mason init
```
This creates a `mason.yaml` file where all bricks are registered.

### **4.2. Adding a Brick**
To add a local brick:
```yaml
bricks:
  widget:
    path: ./widget/
```

To add a remote brick from GitHub:
```yaml
bricks:
  greeting:
    git:
      url: https://github.com/felangel/mason.git
      path: bricks/greeting
```

Fetch registered bricks using:
```bash
mason get
```

---

## **5. Creating a Custom Brick**

### **5.1. Create a New Brick**
Run the following command:
```bash
mason new widget
```
This creates a directory structure:
```text
.
├── mason-lock.json
├── mason.yaml
└── widget
    ├── __brick__
    │   └── HELLO.md
    ├── brick.yaml
    ├── CHANGELOG.md
    ├── LICENSE
    └── README.md
```

### **5.2. Define Variables**
Edit `brick.yaml` to define variables for your brick:
```yaml
vars:
  name:
    type: string
    description: The widget name
    default: MyWidget
  isStateless:
    type: boolean
    description: Stateless or Stateful widget
    default: true
```

### **5.3. Add Template Files**
Replace the default `HELLO.md` with a Dart file inside `__brick__`:
```text
__brick__/
  {{name.snakeCase()}}.dart
```

### **5.4. Write the Template**
Use Mustache syntax for dynamic content in your template:
```dart
import 'package:flutter/material.dart';

{{#isStateless}}
class {{name.pascalCase()}} extends StatelessWidget {
  const {{name.pascalCase()}}({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("{{name}}")),
    );
  }
}
{{/isStateless}}
{{^isStateless}}
class {{name.pascalCase()}} extends StatefulWidget {
  const {{name.pascalCase()}}({Key? key}) : super(key: key);

  @override
  State<{{name.pascalCase()}}> createState() => _{{name.pascalCase()}}State();
}

class _{{name.pascalCase()}}State extends State<{{name.pascalCase()}}>{
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("{{name}}")),
    );
  }
}
{{/isStateless}}
```

---

## **6. Using a Brick**

### **6.1. List Bricks**
To see available bricks:
```bash
mason list
```

### **6.2. Generate Files**
Generate files using the `make` command:
```bash
mason make widget
```
Provide values for the variables when prompted or pass them as arguments:
```bash
mason make widget -o ./lib/widgets --name HomePage --isStateless true
```

---

## **7. Using Remote Bricks**

### **7.1. BrickHub.dev**
Search for bricks:
```bash
mason search <query>
```
Example:
```bash
mason search state
```
Install a brick:
```bash
mason add bloc
```

### **7.2. Remove a Brick**
To remove an unused brick:
```bash
mason remove <brickname>
```

---

## **8. Best Practices**

1. **Consistency**: Use bricks to standardize repetitive code across projects.
2. **Reusability**: Create modular and reusable templates.
3. **Documentation**: Document brick usage and variables for team reference.
4. **Validation**: Use conditional logic to validate inputs and ensure correct file generation.
5. **Version Control**: Maintain and version your bricks for backward compatibility.

---

## **9. Troubleshooting Common Issues**

### **9.1. Brick Not Found**
- **Problem**: Mason cannot locate a brick.
- **Solution**: Ensure the brick is registered in `mason.yaml` and run:
  ```bash
  mason get
  ```

### **9.2. Variable Errors**
- **Problem**: Missing or invalid variable input.
- **Solution**: Validate the variable types and provide defaults in `brick.yaml`.

### **9.3. Directory Issues**
- **Problem**: Files generated in the wrong location.
- **Solution**: Use the `-o` flag to specify the output directory.

---

## **10. Resources**

- [Mason CLI Documentation](https://pub.dev/packages/mason_cli)
- [BrickHub.dev](https://brickhub.dev)
- [Mustache Syntax Guide](https://mustache.github.io/mustache.5.html)

---

This document now provides a detailed guide to using Mason and Bricks in Flutter, covering installation, setup, customization, and best practices. Expand further based on specific project needs!
