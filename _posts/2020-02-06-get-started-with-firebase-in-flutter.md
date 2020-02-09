---
layout: post
comments: true
title: Get Started with Firebase in Flutter
description: In this guide we will explain how to integrate firebase in flutter, create and save data to firebase while using flutter.
ids: 13
category: Firebase
---

<p class="message"> 
You started learning flutter and now you need a backend, you read about Firebase but didn't really understand how to use it.
<br>
<br>
Well in this article I will explain how to integrate Firebase with Flutter, save and retrieve data from the realtime database.
</p>

**Note**: *Flutter is a hybrid framework for mobile applications, but for this article I’m only going to use it on Android device. Also I’m not going to go in depth about Flutter, the article is mostly about using Firebase realtime database in Flutter.*

<img data-sizes="auto" class="center lazy-loading" data-src="/assets/images/download.png" src="/assets/images/download.png" alt="ionic http" data-srcset="/assets/images/download.png 300w,
/assets/images/download.png 600w,
/assets/images/download.png 900w">

## Integrating Firebase in Flutter

So first, you need to have Flutter already installed in your operating system. I’m not going to go into that, but you can check [here](https://flutter.dev/docs/get-started/install) on how to install Flutter. Now open Visual studio code, and execute the following command:

```bash
flutter create firebase_with_flutter
```

<style>
  .example_responsive { width: 300px; height: 250px; }
</style>
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- inside posts -->
<ins class="adsbygoogle example_responsive"
     style="display:block"
     data-ad-client="ca-pub-8689548599050263"
     data-ad-slot="2590272657"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>


This command will create a new project with the name **firebase_with_flutter**. Then to go to the directory of the project, execute the following:

```bash
cd firebase_with_flutter
code .
```

Now, if you execute `flutter run` you will see a new application created on your device. Now in the next step, we start integrating Firebase into the project. So, first open the Firebase console and create a new project, after doing that you can click on the **Android** icon and start adding information related to the project. Now,you need to get the package name which you can find under the following path **android\app\src\main\AndroidManifest.xml**:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
package="com.example.firebase_with_flutter">
```

After following the steps, you can download the Firebase Android config file **google-services.json** and add it under **android/app**. You can check [this answer](https://stackoverflow.com/questions/51783588/where-do-i-place-googleservices-json-in-flutter-app-in-order-to-solve-google-se/51783938#51783938) to know exactly where to add the file. Now, you need to navigate to the **android/build.gradle** and add the google maven repository and the google-services classpath:

```groovy
buildscript {
  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository
  }
  dependencies {
    ...
    // Add this line
    classpath 'com.google.gms:google-services:4.3.3'
  }
}

allprojects {
  ...
  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository
    ...
  }
}
```

Then inside your app **android\app\build.gradle**, add the following:

```groovy
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'com.google.gms.google-services' //this line
```

And inside the dependencies block add the following dependency:

```groovy
implementation 'com.google.firebase:firebase-database:19.2.1'
```
Now you can successfully use Firebase realtime database in your project!

## Save Data to Firebase

To be able to call the Firebase SDK, you can use the following plugins. Therefore inside **pubspec.yaml** add the following:

```yaml
dependencies:   
    firebase_database: ^3.1.1
```

and save by clicking CTRL + S, visual studio code will execute `flutter packages get` and add the plugin to your project. 

**Note**: The *pubspec.yaml* is where you add all your dependencies that you are going to use in your project, it is similar to the `build.gradle` file when creating a native android project.

To save data to the Firebase realtime database, first we need to get an instance of the database and then we can use the `set` method to save the data:

```dart
class _MyHomePageState extends State<MyHomePage> {
  final fb = FirebaseDatabase.instance;
  final myController = TextEditingController();
  final name = "Name";

  @override
  Widget build(BuildContext context) {
    final ref = fb.reference();
    return Scaffold(
        appBar: AppBar(
          title: Text(widget.title),
        ),
        body: Container(
            child: Column(
          children: <Widget>[
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: <Widget>[
                Text(name),
                Flexible(child: TextField(controller: myController)),
              ],
            ),
            RaisedButton(
              onPressed: () {
                ref.child(name).set(myController.text);
              },
              child: Text("Submit"),
            )
          ],
        )));
  }

  @override
  void dispose() {
    // Clean up the controller when the widget is disposed.
    myController.dispose();
    super.dispose();
  }
}
```

So first, we use the `StatefulWidget` since it is a widget that has a mutable state. Then we use the method `createState()` that will return `_MyHomePageState` . Now in this class you can add your logic and create a form. So first get an instance of Firebase by using `FirebaseDatabase.instance` . Then create an instance of `TextEditingController` that will assigned to the property controller inside the `TextField` widget. 

The `TextEditingController` will be used to retrieve the text written inside the TextField . Then, you need to call the method `reference()` that will return a `DatabaseReference` . After that, create a `RaisedButton` and use the `onPressed` property that will take a callback which will be called when the button is tapped or otherwise activated. Inside the callback, you can use the following code `ref.child(name).set(myController.text)`, this will create an attribute in the database with the value of the `TextField`.

<img data-sizes="auto" class="center lazy-loading" data-src="/assets/images/name.PNG" src="/assets/images/name.PNG" alt="ionic http" data-srcset="/assets/images/name.PNG 300w,
/assets/images/name.PNG 600w,
/assets/images/name.PNG 900w">

## Retrieving Data From Firebase

Now to retrieve data, you can use the following code:

```dart
var retrievedName;          

            RaisedButton(
              onPressed: () {
                ref.child("Name").once().then((DataSnapshot data){
                  print(data.value);
                  print(data.key);
                  setState(() {
                    retrievedName = data.value;
                  });
                });
              },
              child: Text("Get"),
            ),
            Text(retrievedName ?? "name"),
```

We can create a new `RaisedButton` and inside the `onPressed` callback, we can use the `once()` method to retrieve the data once. Since `once()` returns a `Future<DataSnapshot>` then it is [asynchronous](https://dart.dev/codelabs/async-await) and you can use the method `then()` which registers a callback to be called when this future completes. 

Since the data is of type `DataSnapshot` , then you can use the property `value` to retrieve the content of this `datasnapshot`. You can also use the property `key` to retrieve the current location of this `dataSnapshot`, so in this case it will retrun the attribute `Name`.


*I hope you enjoyed this Flutter/Firebase article, in the next article I will go more in depth about retrieving and saving different Firebase database structure and will use queries to retrieve data.*