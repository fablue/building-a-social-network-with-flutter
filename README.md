# How I built a social network like Instagram in a few lines of code using flutter and why I fell in love with it
## What I wanted to build and why I tried Flutter
As of last year I had very fun building my own little social network called Lime. So far you can post images and text messages  to Lime,
 give posts a "Like" and comment/chat on them. The clue: All messages are tightly bound to the location where it was posted.
 That means you are only able to see posts which have been created in a maximum radius of 30km. Here is what it currently looks like:

<br><br>
### GIF: Lime native Android App
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/lime-preview.gif?raw=true" width="350">
<br><br>

<br><br>
### Screenshot: Lime native Android App
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/lime-preview.png?raw=true" width="350">
<br><br>

But the App had one big problem: I didn't develop an iOS Version(so far)!
So because I do not own an Apple Computer nor do I own an iPhone I decided to look for any cool
 Framework which would allow me to develop Lime for both platforms. This way I would be able to build
 and debug it on my linux machine and my Android smartphone, which sounded great to me :relaxed:.
Since I am a great fan of Dart as a programming language, I found Flutter. And hell: I remember seeing the first code
example of a layout and immediately leaving the website shaking my head. I visited all the other, well known, Frameworks
like Xamarin, ReactNative or NativeScript and those are all great Frameworks but nothing could catch me entirely.
At the end I cloned Flutter, gave it a chance by reading the docs and tried building my first layouts and quickly fell in love with it :heart:
But why did I try Flutter when my first impression wasn't that great at all?

Well there are many reasons:
- First of all I like Dart and I prefer it MUCH over javascript since I am most used to writing Java code
- Secondly IntelliJ is my IDE of choice  :heart:
- And the Game-Changer (for me): Flutter runs inside a VM and draws everything itself.
This should bring performance and a whole lot of possibilities to the framework. And yep: I was right!

Since I started building Lime with Flutter it took me around 10 hours of work (reading the docs included) to built the following and here is how its done.
<br><br>
### GIF: Lime Flutter version
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/lime-f-preview.gif?raw=true" width="350">
<br><br>

<br><br>
### Screenshot: Lime Flutter version
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/lime-f-preview.png?raw=true" width="350">
<br><br>

Disclaimer: I assume, that you already know how to setup Flutter and that you have a very basic understanding of how the Framework works. There is a lot of great stuff to read for you at https://flutter.io :heart:

## What we need to do
### Step 1: Build the basic layout
We will rebuild the basic layout of lime including the ViewPager and the bottom navigation.

### Step 2: Build a list which supports pagination for data loading
Since Lime includes multiple lists, displaying large data-sets, should we implement a way to handle
the data loading and pagination logic in a somewhat elegant way. I will show how I did it. You
can judge it if you want :relaxed:

### Step 3: Build the layout of a post
We will rebuild the layout of a post. We will see how easy even non-trivial layouts can
be built using Flutter and how fast it is done.

### Step 4: Build fading images
We want the images to fade after loading. I will show how you can use Flutters modular system of
widgets to build such a behaviour in very few lines of code

### Step 5: Integrate first interaction: Make a post "likeable" and animate the action.
Once a user hits the little ghost the post is liked. I will show how to execute a simple REST-API call,
and how to animate the state change. We will learn about callbacks to handle state changes of
parent widgets.

### Step 6: Profit. Didn't figure that one out, yet :sweat_smile:

## Step 1: Build the basic layout
The first step is done by rebuilding the basic layout of Lime. We can obviously see that the App consists of:
- A toolbar (which we will miss in this Article since i did not build it yet :smiley:
- A ViewPager(Android)/PageView(Flutter) containing three Pages
- A navigation bar at the bottom of the app controlling the ViewPager/PageView

Luckily Flutter provides a very useful skeleton for building this type of Layout:
[Scaffold](https://docs.flutter.io/flutter/material/Scaffold-class.html)



But first things first: 
A [MaterialApp](https://docs.flutter.io/flutter/material/MaterialApp-class.html) Widget
should be the root of our Application to provide the material design we all love :+1:


```dart
class LimeApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Lime',
      home: new MainPage(),
    );
  }
 }
```

LimeApp will be the entry point of our Application which we wont touch so far.
 So lets have a look how we should build our MainPage to look and feel like we want it to have.

Since we have to do some controlling inside the MainPage we will declare it as
 [StatefulWidget](https://docs.flutter.io/flutter/widgets/StatefulWidget-class.html)

```dart
class MainPage extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    return new _MainPageState();
  }
}
``` 

Now we should talk about the _MainPageState. As written somewhere above:
Please make sure to know what Widgets and their States are and how to basically build layouts using Flutter!

### 1.1 Creating a PageView with three children
The App has to display three different pages to the user: trends, feed and community.
We will use a [PageView](https://docs.flutter.io/flutter/widgets/PageView-class.html) and place
some Placeholders inside it for now.

```dart
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
        body: new PageView(
          children: [
            new Container(color: Colors.red),
            new Container(color: Colors.blue),
            new Container(color: Colors.grey)
          ]
        )
    );
  }
 }
```
We can replace the simple colored containers later with our more sophisticated widgets.

This is how the App should look like right know.
<br><br>
#### Screenshot: Building the PageView
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/t1.gif?raw=true" width="250">
<br><br>


### 1.2 Creating the bottom navigation
Creating the bottom navigation is amazingly simple using the Scaffold :flushed:
All we have to do is provide a
[BottomNavigationBar](https://docs.flutter.io/flutter/material/BottomNavigationBar-class.html).
Because there is no flame/fire icon in the standard icon set are we using something else for now.
Don't worry getting different icons is pretty simple, but wont be covered in this article.

Here is how it looks like:

```dart
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
        body: new PageView(
          children: [
            new Container(color: Colors.red),
            new Container(color: Colors.blue),
            new Container(color: Colors.grey)
          ]
        ),
      bottomNavigationBar: new BottomNavigationBar(
        items: [
          new BottomNavigationBarItem(
              icon: new Icon(Icons.add),
              title: new Text("trends")
          ),
          new BottomNavigationBarItem(
              icon: new Icon(Icons.location_on),
              title: new Text("feed")
          ),
          new BottomNavigationBarItem(
              icon: new Icon(Icons.people),
              title: new Text("community")
          )
        ]
      )
    );
  }
}
```

#### Screenshot: Building the bottom navigation
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/t2.png?raw=true" width="250">
<br><br>

That was extremely simple, wasn't it? :relaxed: <br>
But now we have to provide some controlling for the navigation :cold_sweat: <br>
What we have to do is using a [PageController](https://docs.flutter.io/flutter/widgets/PageController-class.html)

```dart
class _MainPageState extends State<MainPage> {

  /// This controller can be used to programmatically
  /// set the current displayed page
  PageController _pageController;

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
        body: new PageView(
          children: [
       	  ...
          ],

          /// Specify the page controller
          controller: _pageController
        ),
      bottomNavigationBar: new BottomNavigationBar(
        items: [
        ...
        ],

        /// Will be used to scroll to the next page
        /// using the _pageController
        onTap: navigationTapped,
      )
    );
  }

  /// Called when the user presses on of the
  /// [BottomNavigationBarItem] with corresponding
  /// page index
  void navigationTapped(int page){

    // Animating to the page.
    // You can use whatever duration and curve you like
    _pageController.animateToPage(
        page,
        duration: const Duration(milliseconds: 300),
        curve: Curves.ease
    );
  }

  @override
  void initState() {
    super.initState();
    _pageController = new PageController();
  }

  @override
  void dispose(){
    super.dispose();
    _pageController.dispose();
  }
}
```

As you can easily see controlling the PageView is simple and fun! :yum: <br>
Here is what we did:

- Create a PageController inside the .initState()
- Delegate the controller to the PageView as Param
- Handle the onTap event of BottomNavigationBar
- Animate to the page you want using a custom Duration and a custom Curve
- Call .dispose() on the PageController once the State gets disposed! 

So we are facing one last problem: Updating the bottom navigation to indicate
the correct page. Therefore a simple integer is introduced in the _MainPageState indicating which page is currently displayed. 

```dart
class _MainPageState extends State<MainPage> {

  /// This controller can be used to programmatically
  /// set the current displayed page
  PageController _pageController;
  
  /// Indicating the current displayed page
  /// 0: trends
  /// 1: feed
  /// 2: community
  int _page = 0;
  ...
```

To implement this information in the view we have to simply update the BottomNavigationBar as the following

```dart
      bottomNavigationBar: new BottomNavigationBar(
        items: [
   	...
        ],

        /// Will be used to scroll to the next page
        /// using the _pageController
        onTap: navigationTapped,
        currentIndex: _page
      )
```

Last step: Listen to the page changes and call .setState(). We will start by 
creating a new method called onPageChanged(int page) inside the _MainPageState


```dart
  void onPageChanged(int page){
    setState((){
      this._page = page;
    });
  }
```
To make sure, that this method gets called, add it as callback to the PageView:

```dart
new PageView(
          children: [
            new Container(color: Colors.red),
            new Container(color: Colors.blue),
            new Container(color: Colors.grey)
          ],

          /// Specify the page controller
          controller: _pageController,
          onPageChanged: onPageChanged
        )
```

That's all: Basic Layout is done. Everything looks like we want it to be. :sunglasses:
Here is all we have so far:

<br><br>
#### Resume: Basic layout so far, so good :relaxed:
```dart
import 'package:flutter/material.dart';
import 'package:flutter/widgets.dart';

void main() {
  runApp(new LimeApp());
}



class LimeApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Lime',
      home: new MainPage(),
    );
  }
}

class MainPage extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    return new _MainPageState();
  }
}


class _MainPageState extends State<MainPage> {

  /// This controller can be used to programmatically
  /// set the current displayed page
  PageController _pageController;

  /// Indicating the current displayed page
  /// 0: trends
  /// 1: feed
  /// 2: community
  int _page = 0;

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
        body: new PageView(
          children: [
            new Container(color: Colors.red),
            new Container(color: Colors.blue),
            new Container(color: Colors.grey)
          ],

          /// Specify the page controller
          controller: _pageController,
          onPageChanged: onPageChanged
        ),
      bottomNavigationBar: new BottomNavigationBar(
        items: [
          new BottomNavigationBarItem(
              icon: new Icon(Icons.add),
              title: new Text("trends")
          ),
          new BottomNavigationBarItem(
              icon: new Icon(Icons.location_on),
              title: new Text("feed")
          ),
          new BottomNavigationBarItem(
              icon: new Icon(Icons.people),
              title: new Text("community")
          )
        ],

        /// Will be used to scroll to the next page
        /// using the _pageController
        onTap: navigationTapped,
        currentIndex: _page
      )
    );
  }

  /// Called when the user presses on of the
  /// [BottomNavigationBarItem] with corresponding
  /// page index
  void navigationTapped(int page){

    // Animating to the page.
    // You can use whatever duration and curve you like
    _pageController.animateToPage(
        page,
        duration: const Duration(milliseconds: 300),
        curve: Curves.ease
    );
  }


  void onPageChanged(int page){
    setState((){
      this._page = page;
    });
  }

  @override
  void initState() {
    super.initState();
    _pageController = new PageController();
  }

  @override
  void dispose(){
    super.dispose();
    _pageController.dispose();
  }


}
```

#### GIF: Basic layout
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/t3.gif?raw=true" width="250">
<br><br>


## Step 2: Build a list which supports pagination for data loading
Any of Lime's three pages (trends, feed and community) is displaying a almost endless scrolling list.
The plan is to build one basic layout which handles data loading, pagination and refresh for us.
It should use a generic interface or function to load specific data and one to adapt a Widget from given loaded data.

Here is how it looks like:
```dart
typedef Future<List<T>> PageRequest<T> (int page, int pageSize);
typedef Widget WidgetAdapter<T>(T t);
```

### PageRequest
A given PageRequest takes page and pageSize as arguments, while page represents the page index (0 first page, 1 second Page, ...) and
pageSize the exact count of items to load, and returns a List of generic items asynchronously.

### WidgetAdapter
Once our LoadingListView (that is how I called it) successfully loaded items using some kind of PageRequest, the WidgetAdapter is used to
build Widgets from a generic item if needed.

Providing implementations of those two type definitions to our LoadingListView will give us the freedom to reuse the LoadingListView
for almost anything needed in Lime  :heavy_check_mark:

So lets build it! :bangbang:
The LoadingListView should be pretty straight forward.

```dart

class LoadingListView<T> extends StatefulWidget {

  /// Abstraction for loading the data.
  /// This can be anything: An API-Call,
  /// loading data from a certain file or database,
  /// etc. It will deliver a list of objects (of type T)
  final PageRequest<T> pageRequest;

  /// Used for building Widgets out of
  /// the fetched data
  final WidgetAdapter<T> widgetAdapter;

  /// The number of elements requested for each page
  final int pageSize;

  /// The number of "left over" elements in list which
  /// will trigger loading the next page
  final int pageThreshold;

  /// [PageView.reverse]
  final bool reverse;



  final Indexer<T> indexer;

  LoadingListView(this.pageRequest, {
    this.pageSize: 50,
    this.pageThreshold:10,
    @required this.widgetAdapter,
    this.reverse: false,
    this.indexer
  });

  @override
  State<StatefulWidget> createState() {
    return new _LoadingListViewState();
  }
}
```