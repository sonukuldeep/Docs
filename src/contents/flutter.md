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

# [Flutter](https://api.flutter.dev/index.html)

Flutter is a popular open-source framework for building natively compiled applications for mobile, web, and desktop from a single codebase. Flutter provides a wide range of widgets that you can use to create user interfaces for your apps. Here's a list of some commonly used widgets in Flutter:

## Daddy of all widgets

### StatelessWidget [Docs](https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html)

A widget that does not require mutable state.

A stateless widget is a widget that describes part of the user interface by building a constellation of other widgets that describe the user interface more concretely. The building process continues recursively until the description of the user interface is fully concrete (e.g., consists entirely of RenderObjectWidgets, which describe concrete RenderObjects).

### StatefulWidget

A widget that has mutable state.

State is information that (1) can be read synchronously when the widget is built and (2) might change during the lifetime of the widget. It is the responsibility of the widget implementer to ensure that the State is promptly notified when such state changes, using State.setState.

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

### Button

[Different button shapes](https://stackoverflow.com/questions/49991444/create-a-rounded-button-button-with-border-radius-in-flutter)

### Container

[Docs](https://api.flutter.dev/flutter/widgets/Container-class.html)

```js
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

[Docs](https://api.flutter.dev/flutter/widgets/Text-class.html)

```js
Text(
  'This is a long text that may overflow if it doesn\'t fit on the screen. You can use properties like maxLines and overflow to handle this.',
  style: TextStyle(fontSize: 16.0),
  textAlign: TextAlign.center,
  maxLines: 2,
  overflow: TextOverflow.ellipsis,
)
```

### Image

[Docs](https://api.flutter.dev/flutter/widgets/Image-class.html)

```js
const Image(
  image: NetworkImage('https://flutter.github.io/assets-for-api-docs/assets/widgets/owl.jpg'),
)
// also
Image.network(
  'https://flutter.github.io/assets-for-api-docs/assets/widgets/owl.jpg',
  width: 200.0,
  height: 150.0,
  fit: BoxFit.cover,
)

// also
// Image.asset is used to load an image from a local asset.
Image.asset('assets/images/flutter_logo.png')
// also
// Here's a simple example of displaying an image from raw byte data:
Image.memory(Uint8List.fromList(byteData))
```

### Column

[Docs](https://api.flutter.dev/flutter/widgets/Column-class.html)

```js
Column(
  crossAxisAlignment: CrossAxisAlignment.start,
  mainAxisSize: MainAxisSize.min,
  children: <Widget>[
    const Text('We move under cover and we move as one'),
    const Text('Through the night, we have one shot to live another day'),
    const Text('We cannot let a stray gunshot give us away'),
    const Text('We will fight up close, seize the moment and stay in it'),
    const Text('It’s either that or meet the business end of a bayonet'),
    const Text('The code word is ‘Rochambeau,’ dig me?'),
    Text('Rochambeau!', style: DefaultTextStyle.of(context).style.apply(fontSizeFactor: 2.0)),
  ],
)
```

### Row

[Docs](https://api.flutter.dev/flutter/widgets/Row-class.html)

```js
const Row(
  children: <Widget>[
    Expanded(
      child: Text('Deliver features faster', textAlign: TextAlign.center),
    ),
    Expanded(
      child: Text('Craft beautiful UIs', textAlign: TextAlign.center),
    ),
    Expanded(
      child: FittedBox(
        child: FlutterLogo(),
      ),
    ),
  ],
)
```

### ListView

[Docs](https://api.flutter.dev/flutter/widgets/ListView-class.html)

```js
final List<String> entries = <String>['A', 'B', 'C'];
final List<int> colorCodes = <int>[600, 500, 100];

Widget build(BuildContext context) {
  return ListView.builder(
    padding: const EdgeInsets.all(8),
    itemCount: entries.length,
    itemBuilder: (BuildContext context, int index) {
      return Container(
        height: 50,
        color: Colors.amber[colorCodes[index]],
        child: Center(child: Text('Entry ${entries[index]}')),
      );
    }
  );
}
```

### Stack

[Docs](https://api.flutter.dev/flutter/widgets/Stack-class.html)

```js
Stack(
  children: <Widget>[
    Container(
      width: 100,
      height: 100,
      color: Colors.red,
    ),
    Container(
      width: 90,
      height: 90,
      color: Colors.green,
    ),
    Container(
      width: 80,
      height: 80,
      color: Colors.blue,
    ),
  ],
)
```

### AppBar

[Docs](https://api.flutter.dev/flutter/material/AppBar-class.html)

```js
appBar: AppBar(
        title: const Text('AppBar Demo'),
        actions: <Widget>[
          IconButton(
            icon: const Icon(Icons.add_alert),
            tooltip: 'Show Snackbar',
            onPressed: () {
              ScaffoldMessenger.of(context).showSnackBar(
                  const SnackBar(content: Text('This is a snackbar')));
            },
          ),
          IconButton(
            icon: const Icon(Icons.navigate_next),
            tooltip: 'Go to the next page',
            onPressed: () {
              Navigator.push(context, MaterialPageRoute<void>(
                builder: (BuildContext context) {
                  return Scaffold(
                    appBar: AppBar(
                      title: const Text('Next page'),
                    ),
                    body: const Center(
                      child: Text(
                        'This is the next page',
                        style: TextStyle(fontSize: 24),
                      ),
                    ),
                  );
                },
              ));
            },
          ),
        ],
      ),
```

### Tab-bar

[Docs](https://api.flutter.dev/flutter/material/TabBar-class.html)

```js
import 'package:flutter/material.dart';

/// Flutter code sample for [TabBar].

void main() => runApp(const TabBarApp());

class TabBarApp extends StatelessWidget {
  const TabBarApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(useMaterial3: true),
      home: const TabBarExample(),
    );
  }
}

class TabBarExample extends StatelessWidget {
  const TabBarExample({super.key});

  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      initialIndex: 1,
      length: 3,
      child: Scaffold(
        appBar: AppBar(
          title: const Text('TabBar Sample'),
          bottom: const TabBar(
            tabs: <Widget>[
              Tab(
                icon: Icon(Icons.cloud_outlined),
              ),
              Tab(
                icon: Icon(Icons.beach_access_sharp),
              ),
              Tab(
                icon: Icon(Icons.brightness_5_sharp),
              ),
            ],
          ),
        ),
        body: const TabBarView(
          children: <Widget>[
            Center(
              child: Text("It's cloudy here"),
            ),
            Center(
              child: Text("It's rainy here"),
            ),
            Center(
              child: Text("It's sunny here"),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Card

[Docs](https://api.flutter.dev/flutter/material/Card-class.html)

```js
import 'package:flutter/material.dart';

/// Flutter code sample for [Card].

void main() {
  runApp(const CardExamplesApp());
}

class CardExamplesApp extends StatelessWidget {
  const CardExamplesApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
          colorSchemeSeed: const Color(0xff6750a4), useMaterial3: true),
      home: Scaffold(
        appBar: AppBar(title: const Text('Card Examples')),
        body: const Column(
          children: <Widget>[
            Spacer(),
            ElevatedCardExample(),
            FilledCardExample(),
            OutlinedCardExample(),
            Spacer(),
          ],
        ),
      ),
    );
  }
}

/// An example of the elevated card type.
///
/// The default settings for [Card] will provide an elevated
/// card matching the spec:
///
/// https://m3.material.io/components/cards/specs#a012d40d-7a5c-4b07-8740-491dec79d58b
class ElevatedCardExample extends StatelessWidget {
  const ElevatedCardExample({super.key});

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: Card(
        child: SizedBox(
          width: 300,
          height: 100,
          child: Center(child: Text('Elevated Card')),
        ),
      ),
    );
  }
}

/// An example of the filled card type.
///
/// To make a [Card] match the filled type, the default elevation and color
/// need to be changed to the values from the spec:
///
/// https://m3.material.io/components/cards/specs#0f55bf62-edf2-4619-b00d-b9ed462f2c5a
class FilledCardExample extends StatelessWidget {
  const FilledCardExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Card(
        elevation: 0,
        color: Theme.of(context).colorScheme.surfaceVariant,
        child: const SizedBox(
          width: 300,
          height: 100,
          child: Center(child: Text('Filled Card')),
        ),
      ),
    );
  }
}

/// An example of the outlined card type.
///
/// To make a [Card] match the outlined type, the default elevation and shape
/// need to be changed to the values from the spec:
///
/// https://m3.material.io/components/cards/specs#0f55bf62-edf2-4619-b00d-b9ed462f2c5a
class OutlinedCardExample extends StatelessWidget {
  const OutlinedCardExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Card(
        elevation: 0,
        shape: RoundedRectangleBorder(
          side: BorderSide(
            color: Theme.of(context).colorScheme.outline,
          ),
          borderRadius: const BorderRadius.all(Radius.circular(12)),
        ),
        child: const SizedBox(
          width: 300,
          height: 100,
          child: Center(child: Text('Outlined Card')),
        ),
      ),
    );
  }
}
```

## Glass glow effect

Stack is not important but take a look at BackdropFilter

```dart
Stack(
  children: [
    Align(
      alignment: const AlignmentDirectional(8.5, -0.3),
      child: Container(
        height: 300,
        width: 300,
        decoration: const BoxDecoration(
            shape: BoxShape.circle, color: Colors.deepPurple),
      ),
    ),
    Align(
      alignment: const AlignmentDirectional(-8.5, -0.3),
      child: Container(
        height: 300,
        width: 300,
        decoration: const BoxDecoration(
            shape: BoxShape.circle, color: Color(0xFF673A87)),
      ),
    ),
    Align(
      alignment: const AlignmentDirectional(0, -1.8),
      child: Container(
        height: 300,
        width: 300,
        decoration: const BoxDecoration(color: Color(0xFFFFAB40)),
      ),
    ),
    // this is where blur magic happens
    BackdropFilter(
      filter: ImageFilter.blur(sigmaX: 100.0, sigmaY: 100.0),
      child: Container(
        decoration:
            const BoxDecoration(color: Colors.transparent),
      ),
    ),
  ],
),
```

## Scaffold appBar and body

```dart
home: Scaffold(
        backgroundColor: Colors.blue[200],
        appBar: AppBar(
          title: const Text("Hello world"),
          centerTitle: true,
          backgroundColor: Colors.purple,
          elevation: 5,
          leading: const Icon(Icons.menu),
          actions: [
            IconButton(
                onPressed: () {
                  debugPrint('clicked');
                },
                icon: const Icon(Icons.logout))
          ],
        ),
        body: Center(
            child: Container(
          height: 300,
          width: 300,
          decoration: BoxDecoration(
              color: Colors.green[200],
              borderRadius: BorderRadius.circular(20)),
          padding: const EdgeInsets.all(20),
          child: const Icon(
            Icons.favorite,
            color: Colors.white,
            size: 60,
          ),
        )),
      ),
```

## Row, column and expanded widget

### Column

flex option effects row height

```dart
home: Scaffold(
        body: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Expanded(
              child: Container(
                width: 200,
                color: Colors.deepOrange,
              ),
            ),
            Expanded(
              flex: 2,
              child: Container(
                width: 200,
                color: Colors.pink,
              ),
            ),
            Expanded(
              child: Container(
                width: 200,
                color: Colors.blueAccent,
              ),
            ),
          ],
        ),
      ),
```

### Row

flex option effects column width

```dart
home: Scaffold(
        body: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Expanded(
              child: Container(
                color: Colors.deepOrange,
              ),
            ),
            Expanded(
              flex: 2,
              child: Container(
                color: Colors.pink,
              ),
            ),
            Expanded(
              child: Container(
                color: Colors.blueAccent,
              ),
            ),
          ],
        ),
      ),
```

## ListView

If the elements are larger than the screen size the widgets will overflow which will cause render overflow. In such cases _ListView_ can be used to make the screen scrollable hence all widgets will fit and scroll

```dart
home: Scaffold(
        body: ListView(
          children: [
            Container(
              height: 350,
              color: Colors.deepOrange,
            ),
            Container(
              height: 350,
              color: Colors.pink,
            ),
            Container(
              height: 350,
              color: Colors.blueAccent,
            ),
          ],
        ),
      ),
```

### ListView builder

List view builder, as the name suggests, helps in building a ListView from a list

```dart
List<String> names = ["Tom", "jerry", "pluto"];
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        body: ListView.builder(
          itemCount: names.length,
          itemBuilder: (context, index) => ListTile(
            title: Text(names[index]),
          ),
        ),
      ),
    );
```

## GridView

As the name suggest it helps create grid

```dart
  home: Scaffold(
        body: GridView.builder(
          itemCount: 64,
          gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 4),
          itemBuilder: (context, index) => Container(
            color: Colors.indigo,
            margin: const EdgeInsets.all(2),
          ),
        ),
      )
```

## Stack

Stack works similar to column or row widget but instead it stacks all the children one on top of another. The last child is drawn on top.

```dart
body: Stack(
          alignment: AlignmentDirectional.center,
          children: [
            Container(
              width: 300,
              height: 300,
              color: Colors.pink,
            ),
            Container(
              width: 200,
              height: 200,
              color: Colors.blueAccent,
            ),
            Container(
              width: 100,
              height: 100,
              color: Colors.orange,
            ),
          ],
        ),
```

## GestureDetector

Fire a function on tap on double tap etc

```dart
home: Scaffold(
        body: Center(
          child: GestureDetector(
            onTap: () => debugPrint("Hello world"),
            child: Container(
              width: 300,
              height: 300,
              decoration: BoxDecoration(
                color: Colors.amberAccent,
                borderRadius: BorderRadius.circular(8),
              ),
              child: const Center(
                child: Text(
                  "Hello, world",
                  style: TextStyle(fontSize: 20, color: Colors.purple),
                ),
              ),
            ),
          ),
        ),
      ),
```

## navigation

There are few different ways to do this. 1st ways is the preferred method

> Using bottom navigation method

// main.dart

```dart
return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: const FirstPage(),
      routes: {
        'firstpage': (context) => const FirstPage(),
        'homepage': (context) => const HomePage(),
        'settingspage': (context) => const Settingspage(),
        'profilepage': (context) => const ProfilePage(),
      },
    );
```

// first_page.dart

```dart
class FirstPage extends StatefulWidget {
  const FirstPage({super.key});

  @override
  State<FirstPage> createState() => _FirstPageState();
}

class _FirstPageState extends State<FirstPage> {
  final List _pages = const [HomePage(), Settingspage(), ProfilePage()];
  int _selectedindex = 0;

  void _navigatorBottonpage(int index) {
    setState(() {
      _selectedindex = index;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: const Text("First page"),
          centerTitle: true,
        ),
        bottomNavigationBar: BottomNavigationBar(
          currentIndex: _selectedindex,
          onTap: _navigatorBottonpage,
          items: const [
            // home
            BottomNavigationBarItem(icon: Icon(Icons.home), label: "Home"),
            // settings
            BottomNavigationBarItem(
                icon: Icon(Icons.settings), label: "Settings"),
            // profile
            BottomNavigationBarItem(icon: Icon(Icons.person), label: "Profile"),
          ],
        ),
        body: _pages[_selectedindex]);
  }
}
```

> Using drawer
> // main.dart

```dart
return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: const FirstPage(),
      routes: {
        'firstpage': (context) => const FirstPage(),
        'homepage': (context) => const HomePage(),
        'settingspage': (context) => const Settingspage(),
      },
    );
```

// first_page.dart

```dart
      appBar: AppBar(
        title: const Text("First page"),
        centerTitle: true,
      ),
      drawer: Drawer(
        backgroundColor: Colors.white,
        child: Column(
          children: [
            const DrawerHeader(
              child: Icon(
                Icons.favorite,
                size: 48,
              ),
            ),
            //home page list tite
            ListTile(
              leading: const Icon(Icons.home),
              title: const Text("H O M E"),
              onTap: () {
                // pop drawer first
                Navigator.pop(context);
                Navigator.pushNamed(context, 'homepage');
              },
            ),
            //settings page list tite
            ListTile(
              leading: const Icon(Icons.settings),
              title: const Text("S E T T I N G S"),
              onTap: () {
                // pop drawer first
                Navigator.pop(context);
                Navigator.pushNamed(context, 'settingspage');
              },
            )
          ],
        ),
      ),
```

> Using routes

// main.dart

```dart
return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: const FirstPage(),
      routes: {
        'firstpage': (context) => const FirstPage(),
        'secondpage': (context) => const SecondPage(),
      },
    );
```

// first_page.dart

```dart
return Scaffold(
      appBar: AppBar(
        title: const Text("First page"),
        centerTitle: true,
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(context, 'secondpage');
          },
          child: const Text("Page two"),
        ),
      ),
    );
```

> Using material page route
> // First_page.dart

```dart
return Scaffold(
      appBar: AppBar(
        title: const Text("First page"),
        centerTitle: true,
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => const SecondPage(),
              ),
            );
          },
          child: const Text("Page two"),
        ),
      ),
    );
```

## Theme

[Docs](https://docs.flutter.dev/cookbook/design/themes)

Set theme

```dart
MaterialApp(
  title: appName,
  theme: ThemeData(
    useMaterial3: true,

    // Define the default brightness and colors.
    colorScheme: ColorScheme.fromSeed(
      seedColor: Colors.purple,
      // ···
      brightness: Brightness.dark,
    ),

    // Define the default `TextTheme`. Use this to specify the default
    // text styling for headlines, titles, bodies of text, and more.
    textTheme: TextTheme(
      displayLarge: const TextStyle(
        fontSize: 72,
        fontWeight: FontWeight.bold,
      ),
      // ···
      titleLarge: GoogleFonts.oswald(
        fontSize: 30,
        fontStyle: FontStyle.italic,
      ),
      bodyMedium: GoogleFonts.merriweather(),
      displaySmall: GoogleFonts.pacifico(),
    ),
  ),
  home: const MyHomePage(
    title: appName,
  ),
);
```

Apply

```dart
Container(
  padding: const EdgeInsets.symmetric(
    horizontal: 12,
    vertical: 12,
  ),
  color: Theme.of(context).colorScheme.primary,
  child: Text(
    'Text with a background color',
    // ···
    style: Theme.of(context).textTheme.bodyMedium!.copyWith(
          color: Theme.of(context).colorScheme.onPrimary,
        ),
  ),
),
```
