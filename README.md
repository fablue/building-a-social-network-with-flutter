# How I built a social netork like Instagram in a few lines of code using flutter and why i fell in love with it
## What I wanted to build and why I tried Flutter
As of last year I had very fun building my own little social network called Lime. So far you can post images and text messages  to Lime, give posts a "Like" and comment/chat on them. The clue: All messages are tighly bound to the location it was posted. So you are only able to see posts which have been posted in an at maximum 30km radius. Here is what it looks like:

<br><br>
### GIF: Lime native Android App
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/lime-preview.gif?raw=true" width="350">
<br><br>

<br><br>
### Screenshot: Lime native Android App
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/lime-preview.png?raw=true" width="350">
<br><br>

But the App had one big problem: I didnt develop an iOS Version! So because I do not own an Apple Computer nor do I own an iPhone I decided to look around for any cool Framework which would allow me to develop an App for both platforms. This way I would be able to build and debug it on my linux machine and my Android smartphone, which sounded great to me. 
Since I am a great fan of Dart as a programming language, I found Flutter. And hell: I remember seeing the first code example of a layout and immediatelly leaving the website shaking my head. So I visited all the other well known Frameworks like Xamarin, ReactNative or NativeScript and those are all great Frameworks but nothing could catch me entirly. So I cloned Flutter, gave it a chance by reading the docs and tried building my first layouts. The reason: I like Dart and I like IntelliJ and I love the idea that Flutter runs inside a VM and draws everything by its own. Since then I needed 10 hours of work (reading the docs included) to built the following and here is how I did it. 

<br><br>
### GIF: Lime Flutter version
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/lime-f-preview.gif?raw=true" width="350">
<br><br>

<br><br>
### Screenshot: Lime Flutter version
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/lime-f-preview.png?raw=true" width="350">
<br><br>

Disclaimer: I assume, that you already know how to setup Flutter and that you have a very basic understanding of how the Framework works. There is a lot of great stuff to read for you at https://flutter.io :heart:

## Step 1: Build the basic layout
The first step is done by rebuilding the basic layout of Lime. We can obviously see that the App consists of:
- A toolbar (which we will miss in this Article since i did not build it yet :smiley:
- A ViewPager(Android)/PageView(Flutter) containg three Pages
- A Navigationbar at the bottom of the App to provide a navigtion for the App. 

Luckily Flutter provides a very useful skeleton for building this type of Layout: [Scaffold](https://docs.flutter.io/flutter/material/Scaffold-class.html) 

But first things first: 
Using [MaterialApp](https://docs.flutter.io/flutter/material/MaterialApp-class.html) should the root of our Application to provide the material design we all love :+1:


```dart
class LimeApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Lime',
      home: new MainPage(),
    );
  }
```

LimeApp will be the root of our Application which we wont touch so far. So lets have a look how we should build our MainPage to look and feel like we want it to have. 

Since we have to do some controlling inside the MainPage we will declare it as [StatefulWidget](https://docs.flutter.io/flutter/widgets/StatefulWidget-class.html)

```dart
class MainPage extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    return new _MainPageState();
  }
}
``` 

So now we should talk about the _MainPageState. As written somewhere above: Please make sure to know what Widgets and their States are and how to basically build layouts using Flutter!

### 1.1 Creating a PageView with three children
Since we know that we need to have 3 pages: "trends", "feed" and "community" inside our app we will implement those pages later, but we will implment the Scaffold supporting those pages. 
We will place color placeholders inside them.


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
```

This is how your app should look like right know
<br><br>
#### Screenshot: Building the PageView
<img src="https://github.com/fablue/building-a-social-network-with-flutter/blob/master/t1.gif?raw=true" width="250">
<br><br>


### 1.2 Creating the bottom navigation
Creating the bottom navigation is amazingly simple using the Scaffold :flushed:
All we have to do is provide a [BottomNavigationBar](https://docs.flutter.io/flutter/material/BottomNavigationBar-class.html). Because there is no flame/fire icon in the standard icon set are we using something else for now. Dont worry getting different icons is pretty simple, but wont be covered in this article. 

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
              title: null
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

That was extremly simple, wasnt it? :relaxed: <br>
But now we have to provide some controlling for the navigation :cold_sweat: <br>


