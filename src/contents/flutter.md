---
author: kuldeep
datetime: 2023-09-10
title: Flutter
slug: "flutter"
featured: true
draft: false
tags:
  - flutter mobile-app
ogImage: ""
description: Flutter is a popular open-source framework for building natively compiled applications for mobile, web, and desktop from a single codebase. Flutter provides a wide range of widgets that you can use to create user interfaces for your apps.
---

# Flutter

Flutter is a popular open-source framework for building natively compiled applications for mobile, web, and desktop from a single codebase. Flutter provides a wide range of widgets that you can use to create user interfaces for your apps. Here's a list of some commonly used widgets in Flutter:

## Table Of Contents

## Must used widgets

1. **Container**: A basic rectangular box that can contain other widgets. It's often used for styling and layout.

2. **Text**: Displays a paragraph of text with optional styling.

3. **Image**: Used to display images, including network images and local assets.

4. **Row** and **Column**: Used for arranging widgets horizontally (Row) or vertically (Column) in a linear layout.

5. **ListView**: A scrollable list of widgets. There are various types of ListViews, such as `ListView`, `ListView.builder`, and `ListView.separated`.

6. **Stack**: Allows you to place widgets on top of each other, often used for complex layouts.

7. **AppBar**: A material design app bar that typically appears at the top of the screen and contains an app's title, actions, and navigation.

8. **TabBar** and **TabView**: Used to create tabbed interfaces.

9. **Card**: A material design card with rounded corners, often used to display content in a contained format.

10. **TextField**: A widget for accepting user input, such as text or numbers.

11. **Button**: Widgets like `ElevatedButton`, `TextButton`, and `OutlinedButton` for creating various types of buttons.

12. **Icon**: Displays icons using Flutter's built-in icon library or custom icons.

13. **Drawer**: A slide-out navigation panel that is often used in conjunction with the AppBar for navigation.

14. **BottomNavigationBar**: A navigation bar typically placed at the bottom of the screen for switching between different sections of an app.

15. **Dialog**: Used to create modal dialogs or pop-up windows.

16. **ProgressIndicator**: Widgets like `CircularProgressIndicator` and `LinearProgressIndicator` for showing progress.

17. **Switch** and **Checkbox**: Widgets for toggling between two states.

18. **Slider** and **RangeSlider**: Widgets for selecting values from a continuous range.

19. **DatePicker** and **TimePicker**: Widgets for selecting dates and times.

20. **WebView**: Used to embed web content within your Flutter app.

These are just a few of the commonly used widgets in Flutter. Flutter provides a rich set of widgets for various purposes, and you can also create custom widgets to suit your specific needs.

## Widget in detail

### Container

```dart
Container(
  width: 200.0,
  height: 100.0,
  margin: EdgeInsets.all(16.0),
  padding: EdgeInsets.symmetric(vertical: 8.0, horizontal: 16.0),
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(10.0),
    boxShadow: [
      BoxShadow(
        color: Colors.grey,
        blurRadius: 5.0,
        offset: Offset(0, 3),
      ),
    ],
  ),
  child: Center(
    child: Text(
      'Styled Container',
      style: TextStyle(color: Colors.white, fontSize: 18.0),
    ),
  ),
)
```

### Text

```dart
Text(
  'This is a long text that may overflow if it doesn\'t fit on the screen. You can use properties like maxLines and overflow to handle this.',
  style: TextStyle(fontSize: 16.0),
  textAlign: TextAlign.center,
  maxLines: 2,
  overflow: TextOverflow.ellipsis,
)
```
