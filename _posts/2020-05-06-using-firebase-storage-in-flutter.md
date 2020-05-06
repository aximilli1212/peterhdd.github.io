---
layout: post
comments: true
title: Using Firebase Storage in Flutter
description: In this guide, we will use Firebase Storage to add, retrieve images, and connect it with Firestore all while using Flutter.
ids: 18
category: Firebase
---

<p class="message"> 
In this article, we will use Firebase Storage to add images and retrieve them, we will also connect the images with Firestore.
</p>

>This is the fifth article related to Firebase in Flutter, you can check the previous articles in the below links:

- [Get Started With Firebase in Flutter](https://petercoding.com/firebase/2020/02/06/get-started-with-firebase-in-flutter/)
- [Using Firebase Queries In Flutter](https://petercoding.com/firebase/2020/02/16/using-firebase-queries-in-flutter/)
- [Using Firebase Auth In Flutter](https://petercoding.com/firebase/2020/02/25/using-firebase-auth-in-flutter/)
- [Using Cloud Firestore In Flutter](https://petercoding.com/firebase/2020/04/04/using-cloud-firestore-in-flutter/)

To know how to download the `google-service.json` file, you can check the first article in the above list. In this article, I will add images to the `assets` folder, so we can then add those images to Firebase Storage, link them with Firestore and then retrieve those images, and we will also use the image picker plugin to get an image from gallery and save it to Firebase Storage.

## Add Firebase Storage To Flutter

As I said before, to check how to create a flutter project and add the `google-service.json` file which is used for android, then please check this article [Get Started With Firebase in Flutter](https://petercoding.com/firebase/2020/02/06/get-started-with-firebase-in-flutter/). Next, you need to add the following dependency to the `pubspec.yaml` file:

```yaml
dependencies:
  firebase_storage: ^3.1.5
  cloud_firestore: ^0.13.4+2
```
Click <kbd>CTRL</kbd> + <kbd>S</kbd> to save, and you have successfully added Cloud Firestore + Firebase Storage to your Flutter application!

-----

Also, since we are not using Firebase Authentication, you need to change the rules for Firebase Storage to public:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if true;
    }
  }
}
```
But only use the above rules in the development phase and not in production.

*Note : I would advise to read the previous articles related to Firebase with Flutter before this one.*

## Adding Images to Flutter

So, since we are going to use images inside the `assets` folder and add them to Firebase Storage, therefore first we can create a folder called `images` under the `assets` folder and inside the `images` folder we can add the images. 

<img src="/assets/images/imagesflutter.png" data-src="/assets/images/imagesflutter.png" class="lazy-loading" data-sizes="auto" alt="signup firebase flutter" data-srcset="/assets/images/imagesflutter.png 300w,
    /assets/images/imagesflutter.png 600w,
    /assets/images/imagesflutter.png 900w">


Then to access them inside the Flutter application, we need to declare them inside the `pubspec.yaml`:

```yaml
  # To add assets to your application, add an assets section, like this:
  assets:
  - assets/images/
```
Now, you can access all images inside the `assets/images/` in your application.

## Implementing Multiple Select in GridView

To display those images inside the Flutter application, we can use a `gridView` widget. So first inside the `main.dart` file, create a `StatelessWidget` that will call a `StatefulWidget`:

```dart
void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Firebase Demo',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: 'Firebase Storage Tutorial'),
    );
  }
}
```

So `main()` method will be called first and, it will call the `runApp()` method which will take a `widget` and make it the root widget. The `build()` method will contain the layout of the application. The `home` property will take a `widget`, in this case we created a `StatefulWidget` called `myHomePage`. Then we need to declare this widget:

```dart
class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);

  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```
So `myHomePage` extends `StatefulWidget`, we `override` the method `createState` and create a `State` class called `_myHomePageState`. In the `State` class we declare the following variables:

```dart
class _MyHomePageState extends State<MyHomePage> {
  var storage = FirebaseStorage.instance;
  List<AssetImage> listOfImage;
  bool clicked = false;
  List<String> listOfStr = List();
  String images;
  bool isLoading = false;
```
We first create an instance of `FirebaseStorage`:

```dart
  var storage = FirebaseStorage.instance;
```
Also, we create a list of images, that will contain our images. The `State` class contains the `initState()` method which is called once when the `StatefulWidget` is loaded, therefore `override` the `initState()` method:

```dart
@override
void initState() {
  super.initState();
  getImages();
}

  void getImages() {
  listOfImage = List();
  for (int i = 0; i < 6; i++) {
    listOfImage.add(
        AssetImage('assets/images/travelimage' + i.toString() + '.jpeg'));
  }
}
```
`getImages()` will retrieve all the images inside the `assets/images` directory and adds them all to the `listOfImage`. Then inside the `build()` method we add the `GridView.builder` widget:

```dart
child: Column(
  children: <Widget>[
    GridView.builder(
      shrinkWrap: true,
      padding: const EdgeInsets.all(0),
      itemCount: listOfImage.length,
      gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 3,
          mainAxisSpacing: 3.0,
          crossAxisSpacing: 3.0),
      itemBuilder: (BuildContext context, int index) {
```
So here the `itemCount` will take the number of images inside the `list`. The `SliverGridDelegateWithFixedCrossAxisCount` will design the `gridView` so on every line we will have 3 images. The `mainAxisSpacing` and the `crossAxisSpacing` is the number of logical pixels between each child.

### Creating The GridView

So now inside the callback `(BuildContext context, int index)`, we return a `GridTile` widget:

```dart
return GridTile(
  child: Material(
    child: GestureDetector(
      child: Stack(children: <Widget>[
        this.images == listOfImage[index].assetName ||
                listOfStr.contains(listOfImage[index].assetName)
            ? Positioned.fill(
                child: Opacity(
                opacity: 0.7,
                child: Image.asset(
                  listOfImage[index].assetName,
                  fit: BoxFit.fill,
                ),
              ))
            : Positioned.fill(
                child: Opacity(
                opacity: 1.0,
                child: Image.asset(
                  listOfImage[index].assetName,
                  fit: BoxFit.fill,
                ),
              )),
        this.images == listOfImage[index].assetName ||
                listOfStr.contains(listOfImage[index].assetName)
            ? Positioned(
                left: 0,
                bottom: 0,
                child: Icon(
                  Icons.check_circle,
                  color: Colors.green,
                ))
            : Visibility(
                visible: false,
                child: Icon(
                  Icons.check_circle_outline,
                  color: Colors.black,
                ),
              )
      ]),
      onTap: () {
        setState(() {
          if (listOfStr
              .contains(listOfImage[index].assetName)) {
            this.clicked = false;
            listOfStr.remove(listOfImage[index].assetName);
            this.images = null;
          } else {
            this.images = listOfImage[index].assetName;
            listOfStr.add(this.images);
            this.clicked = true;
          }
        });
      },
    ),
  ),
);
```

Let's explain step by step here, first the `GridTile` takes a `Material` child, and then the `Material` widget takes a `GestureDetector` widget, which we will use to detects gestures. Then inside the `GestureDetector` widget, we use the `onTap` property:

```dart
onTap: () {
  setState(() {
    if (listOfStr
        .contains(listOfImage[index].assetName)) {
      this.clicked = false;
      listOfStr.remove(listOfImage[index].assetName);
      this.images = null;
    } else {
      this.images = listOfImage[index].assetName;
      listOfStr.add(this.images);
      this.clicked = true;
    }
  });
},
```
So inside the `onTap()` we use the `setState()` which will rebuild the layout by calling the `build()` method. We first check if the `listOfStr` contains the image, if it does then we remove it from the list, and if it doesnt contain then we add it to the list. 

So here the user is tapping on the image, if the image is already tapped then it is inside the list, if the user taps again then we remove it from the list and build again. Now we use the child `Stack` inside the `GestureDetector`. The reason we use `Stack` is because we want to add an icon if the user chose an image. We also use the ternary operator to check if the image is inside the list or not. Check the image below, to see how the layout would be: 

<img src="/assets/images/chooseflutter.jpg" data-src="/assets/images/chooseflutter.jpg" class="lazy-loading" data-sizes="auto" alt="firebase storage flutter" data-srcset="/assets/images/chooseflutter.jpg 300w,
    /assets/images/chooseflutter.jpg 600w,
    /assets/images/chooseflutter.jpg 900w">

So as you can see, when we select an image, we use the `Opacity` widget and decrease the opacity and we also add a check icon.

## Adding Images to Firebase Storage

So to add the selected images to Firebase Storage, we can create a `RaisedButton` to save the images to storage:

```dart
Builder(builder: (context) {
  return RaisedButton(
      child: Text("Save Images"),
      onPressed: () {
        setState(() {
          this.isLoading = true;
        });
        listOfStr.forEach((img) async {
          String imageName = img
              .substring(img.lastIndexOf("/"), img.lastIndexOf("."))
              .replaceAll("/", "");

          final Directory systemTempDir = Directory.systemTemp;
          final byteData = await rootBundle.load(img);

          final file =
              File('${systemTempDir.path}/$imageName.jpeg');
          await file.writeAsBytes(byteData.buffer.asUint8List(
              byteData.offsetInBytes, byteData.lengthInBytes));
          StorageTaskSnapshot snapshot = await storage
              .ref()
              .child("images/$imageName")
              .putFile(file)
              .onComplete;
          if (snapshot.error == null) {
            final String downloadUrl =
                await snapshot.ref.getDownloadURL();
            await Firestore.instance
                .collection("images")
                .add({"url": downloadUrl, "name": imageName});
            setState(() {
              isLoading = false;
            });
            final snackBar =
                SnackBar(content: Text('Yay! Success'));
            Scaffold.of(context).showSnackBar(snackBar);
          } else {
            print(
                'Error from image repo ${snapshot.error.toString()}');
            throw ('This file is not an image');
          }
        });
      });
})
```

We wrap the `RaisedButton` inside a `Builder` because we want to use a `Snackbar` later, which contains `Scaffold.of(context)`, without the `Builder` method then the `Scaffold` widget will not be inside the [context](https://stackoverflow.com/a/51304732/7015400). 

----

So since we might select multiple images, then we iterate inside the `listOfStr` and retrieve the name of the images, then we get the temporary directory inside the phone OS. We also use the `rootBundle` which is a top level property inside the `AssetBundle` and we use `load()` to retrieve a binary resource from the asset bundle as a data stream. After that we create a new file inside the temporary directory, and we write the bytes to the file. Then we can finally save the images to Firebase Storage:

```dart
StorageTaskSnapshot snapshot = await storage
    .ref()
    .child("images/$imageName")
    .putFile(file)
    .onComplete;
```
`ref()` will give you a reference to Firebase Storage, `child()` will create a folder called `images` and inside of the folder we will have all the images, lastly `putFile()` will take the file as an arugment and add it to Firebase Storage. The `onComplete` property returns `Future<StorageTaskSnapshot>` which means it is asynchronous and thats why we use `await` to wait until it adds the image to storage and then continue execution.

<img src="/assets/images/images.png" data-src="/assets/images/images.png" class="lazy-loading" data-sizes="auto" alt="firebase storage flutter" data-srcset="/assets/images/images.png 300w,
    /assets/images/images.png 600w,
    /assets/images/images.png 900w">

## Linking Firebase Storage With Firestore

After `onComplete` property, we can check if the image were successfully added or not:

```dart
if (snapshot.error == null) {
  final String downloadUrl =
      await snapshot.ref.getDownloadURL();
  await Firestore.instance
      .collection("images")
      .add({"url": downloadUrl, "name": imageName});
  setState(() {
    isLoading = false;
  });
  final snackBar =
      SnackBar(content: Text('Yay! Success'));
  Scaffold.of(context).showSnackBar(snackBar);
} else {
  else {
  print(
      'Error from image repo ${snapshot.error.toString()}');
  throw ('This file is not an image');
   }
  });
});
```
If there is no error, then we successfully added the image to Firebase Storage, we can then use `getDownloadUrl()` which returns `Future<dynamic>` and thats why we use `await`. The `getDownloadUrl()` will give us the url of the image inside Firebase Storage. After that we can finally add both the `url` and the `image` to Firestore, inside the `images` collection. We also assign `isLoading` to `false` to stop the `CircularProgressIndicator()` from loading. The `CircularProgressIndicator()` is used inside the `Column` widget:

```dart
isLoading
    ? CircularProgressIndicator()
    : Visibility(visible: false, child: Text("test")),
```

<img src="/assets/images/firestoreImg.png" data-src="/assets/images/firestoreImg.png" class="lazy-loading" data-sizes="auto" alt="firebase storage flutter" data-srcset="/assets/images/firestoreImg.png 300w,
    /assets/images/firestoreImg.png 600w,
    /assets/images/firestoreImg.png 900w">

## Retrieving Images From Firestore

So, to retrieve the images we create another `RaisedButton`:

```dart
RaisedButton(
  child: Text("Get Images"),
  onPressed: () {
    Navigator.push(
      context,
      MaterialPageRoute(builder: (context) => SecondPage()),
    );
  },
),
```
`onPressed` of the button we, navigate to the `SecondPage`. Therefore we create another dart file and call it `second_page.dart`. Inside the `State` class in the we retrieve an instance of Firestore:

```dart
  final Firestore fb = Firestore.instance;
```

Then we use a `FutureBuilder` to get the images and add them in a `listView`:

```dart
body: Container(
  padding: EdgeInsets.all(10.0),
  child: FutureBuilder(
    future: getImages(),
    builder: (context, AsyncSnapshot<QuerySnapshot> snapshot) {
      if (snapshot.connectionState == ConnectionState.done) {
        return ListView.builder(
            shrinkWrap: true,
            itemCount: snapshot.data.documents.length,
            itemBuilder: (BuildContext context, int index) {
              return ListTile(
                contentPadding: EdgeInsets.all(8.0),
                title:
                    Text(snapshot.data.documents[index].data["name"]),
                leading: Image.network(
                    snapshot.data.documents[index].data["url"],
                    fit: BoxFit.fill),
              );
            });
      } else if (snapshot.connectionState == ConnectionState.none) {
        return Text("No data");
      }
      return CircularProgressIndicator();
    },
  ),
),

/// code here

  Future<QuerySnapshot> getImages() {
    return fb.collection("images").getDocuments();
  }
```
So in the `FutureBuilder` we use the method `getImages()` which will return all the documents inside the collection `images`. Then we use the `ConnectionState`, if the `connectionState` is equal to `done`, then the asychronous operation is finished. If it is equal to `none` then the operation finished but didnt return anything. Outside the if/else we return `CircularProgressIndicator();` which will before getting the result from the `Future`.


<img src="/assets/images/fireimage.jpg" data-src="/assets/images/fireimage.jpg" class="lazy-loading" data-sizes="auto" alt="firebase storage flutter" data-srcset="/assets/images/fireimage.jpg 300w,
    /assets/images/fireimage.jpg 600w,
    /assets/images/fireimage.jpg 900w">

## Using Image Picker Plugin

The second way to add images to Firebase Storage, is to get an image from the gallery, so to do that, navigate to the `pubspec.yaml` file and add the following dependency:

```yaml
  image_picker: ^0.6.6+1
```

Then in the `second_page.dart`, add a `RaisedButton` below the `FutureBuilder`:

```dart
RaisedButton(child: Text("Pick Image"), onPressed: getImage)
```
`onPressed` it will call the method `getImage`:

```dart
Future getImage() async {
  var image = await ImagePicker.pickImage(source: ImageSource.gallery);

  setState(() {
    _image = image;
  });
}
```
The `getImage()` method will get an image from gallery, and call `setState` to rebuild the layout. Therefore under the `RaisedButton` we add the image:

```dart
_image == null
    ? Text('No image selected.')
    : Image.file(
        _image,
        height: 300,
      ),
```

<img src="/assets/images/pickImg.jpg" data-src="/assets/images/fireimage.jpg" class="lazy-loading" data-sizes="auto" alt="firebase storage flutter" data-srcset="/assets/images/pickImg.jpg 300w,
    /assets/images/pickImg.jpg 600w,
    /assets/images/pickImg.jpg 900w">

Under the image, we create a `RaisedButton` and save the data to Firebase Storage under folder `image`:

```dart
RaisedButton(
    child: Text("Save Image"),
    onPressed: () async {
      if (_image != null) {
        StorageReference ref = FirebaseStorage.instance.ref();
        StorageTaskSnapshot addImg =
            await ref.child("image/img").putFile(_image).onComplete;
        if (addImg.error == null) {
          print("added to Firebase Storage");
        }
      }
    }),
```
<img src="/assets/images/imageFirebase.png" data-src="/assets/images/imageFirebase.png" class="lazy-loading" data-sizes="auto" alt="firebase storage flutter" data-srcset="/assets/images/pickImg.jpg 300w,
    /assets/images/imageFirebase.png 600w,
    /assets/images/imageFirebase.png 900w">

## Deleting an Image

Now, let's say you have users in your application, each user will have a profile image, if a user gets deleted then you want to delete the image also from Firebase Storage. In that case you can do the following, for example:

```dart
  Firestore.instance
      .collection("images")
      .where("name", isEqualTo: "travelimage4")
      .getDocuments()
      .then((res) {
    res.documents.forEach((result) {
      FirebaseStorage.instance
          .getReferenceFromUrl(result.data["url"])
          .then((res) {
        res.delete().then((res) {
          print("Deleted!");
        });
      });
    });
  });
```
Using `getReferenceFromUrl()` you will get URL pointing to a Firebase Storage location, and then you can call `delete()`.

You can find the source code here: [Firebase Storage Tutorial](https://github.com/PeterHdd/Firebase-Storage-Tutorial)

*I hope you enjoyed this Flutter/Firebase article, in the next article I will use Firebase messaging.*
