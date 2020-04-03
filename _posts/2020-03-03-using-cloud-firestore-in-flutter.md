---
layout: post
comments: true
title: Using Cloud Firestore in Flutter
description: In this guide, we will understand what is cloud firestore, read, write data and use queries with firestore in flutter.
ids: 17
category: Firebase
---

<p class="message"> 
In this article, we will add Cloud Firestore to a Flutter application, perform different read and write operation and use some queries to retrieve data.
</p>

>This is the fourth article related to Firebase in Flutter, you can check the previous articles in the below links:

- [Get Started With Firebase in Flutter](https://petercoding.com/firebase/2020/02/06/get-started-with-firebase-in-flutter/)
- [Using Firebase Queries In Flutter](https://petercoding.com/firebase/2020/02/16/using-firebase-queries-in-flutter/)
- [Using Firebase Auth In Flutter](https://petercoding.com/firebase/2020/02/25/using-firebase-auth-in-flutter/)

To know how to download the `google-service.json` file, you can check the first article in the above list. In the last two articles, I created a form using Flutter performed queries for the realtime database and authenticated users with Firebase, but in this article I'm not going to create a form, its mostly going to be code snippet related to Firestore and explaining each one.

## Introduction

### What is Cloud Firestore?

Both Cloud Firestore and realtime database are nosql database, their are no `joins`, their are no columns or tables, and you don't have to worry about [duplicating](https://firebase.googleblog.com/2013/04/denormalizing-your-data-is-normal.html) your data. The main difference between the two is that Cloud Firestore contains collections and inside these collections you have documents that may contain subcollections or fields mapped to a value while realtime database can be considered as a big json that will contain all the data. The other also important difference to take into consideration is the queries, in realtime database as you can tell from previous articles we can only use one `orderByChild().equalTo()` (we cannot chain) while in Cloud Firestore as we will see later in the article we can chain queries.

### Important Notes To Remember

- <b>Queries are shallow</b>, which means if you have a collection, and are retrieving data then you will only get data from the documents under that collection and not from subcollections.

- <b>Document size is limited to 1mb</b>
- <b>You are charged for every read, write, delete done on a document</b>

## Adding Firestore To Flutter

As I said before, to check how to create a flutter project and add the `google-service.json` file which is used for android, then please check this article [Get Started With Firebase in Flutter](https://petercoding.com/firebase/2020/02/06/get-started-with-firebase-in-flutter/). Next, you need to add the following dependency to the `pubspec.yaml` file:

```
dependencies:
  cloud_firestore: ^0.13.4+2
```
Click <kbd>CTRL</kbd> + <kbd>S</kbd> to save, and you have sucessfully added Cloud Firestore to your Flutter application!

## Adding Data To Firestore

There are two ways to add data to the Cloud Firestore, first way is to specifiy the document name and the second way Cloud Firestore will generate a random id, let us see both cases. So first in your `State` class you need to get an instance of Cloud Firestore:

```dart
final firestoreInstance = Firestore.instance;
```

Now, you can add the data:

```dart
void _onPressed() {
  firestoreInstance.collection("users").add(
  {
    "name" : "john",
    "age" : 50,
    "email" : "example@example.com",
    "address" : {
      "street" : "street 24",
      "city" : "new york"
    }
  }).then((value){
    print(value.documentID);
  });
}
```
As, you can see in the above, on press of a button, we create a user collection and we use the `add()` method which will generate a random id. Since the `add()` method returns `Future<DocumentReference>` therefore we can use the method `then()` which will contain a callback to be called when the `Future` finishes. The variable `value` which is a parameter passed to the callback is of type `DocumentReference` therefore we can use the property `documentID` to retrieve the auto generated id.

<img src="/assets/images/Group.jpg" data-src="/assets/images/Group.jpg" class="lazy-loading" data-sizes="auto" alt="firestore" data-srcset="/assets/images/Group.jpg 300w,
    /assets/images/Group.jpg 600w,
    /assets/images/Group.jpg 900w">

As you can see, we added a map, string, int and we can also add an array.

*Note: If you have alot of data, then do not use all the data inside a `map` inside a collection, instead create a subcollection if the data is related to the top level collection, if not then just create another top level collection. Remember a document has a size limit of 1mb.*

-------

Now let's say you are using [Firebase Authentication in Flutter](https://petercoding.com/firebase/2020/02/25/using-firebase-auth-in-flutter/), instead of using an auto generated id, you can use the `userId` as the document id that way it will be easier to retrieve the data:

```dart
void _onPressed() async{
  var firebaseUser = await FirebaseAuth.instance.currentUser();
  firestoreInstance.collection("users").document(firebaseUser.uid).setData(
  {
    "name" : "john",
    "age" : 50,
    "email" : "example@example.com",
    "address" : {
      "street" : "street 24",
      "city" : "new york"
    }
  }).then((_){
    print("success!");
  });
}
```
Here, since we use the `userId` as the document id, therefore we use the method `setData()` to add data to the document. If a document already exists and you want to update it, then you can use the optional named parameter `merge` and set it to `true`:

```dart
void _onPressed() async{
  var firebaseUser = await FirebaseAuth.instance.currentUser();
  firestoreInstance.collection("users").document(firebaseUser.uid).setData(
  {
   "username" : "userX",
  },merge : true).then((_){
      print("success!");
  });
}
```
This way the existing data inside the document will not be overwritten.

<img src="/assets/images/mergesetfirestore.png" data-src="/assets/images/mergesetfirestore.png" class="lazy-loading" data-sizes="auto" alt="firestore" data-srcset="/assets/images/mergesetfirestore.png 300w,
    /assets/images/mergesetfirestore.png 600w,
    /assets/images/mergesetfirestore.png 900w">

## Update Data In Firestore

To update fields inside a document, you can do the following:

```dart
void _onPressed()async{
    var firebaseUser = await FirebaseAuth.instance.currentUser();
    firestoreInstance
        .collection("users")
        .document(firebaseUser.uid)
        .updateData({"age": 60}).then((_) {
      print("success!");
    });
}
```
So, here we update the `age` to 60, we can also add a new field while updating existing field:

```dart
void _onPressed()async{
    var firebaseUser = await FirebaseAuth.instance.currentUser();
    firestoreInstance
        .collection("users")
        .document(firebaseUser.uid)
        .updateData({"age": 60,"familyName": "Haddad"}).then((_) {
      print("success!");
    });
}
```
You can also update field, add a new field, update the map **and** add a new field to the map:

```dart
void _onPressed() async{
  var firebaseUser = await FirebaseAuth.instance.currentUser();
  firestoreInstance.collection("users").document(firebaseUser.uid).updateData({
    "age": 60,
    "familyName": "Haddad",
    "address.street": "street 50",
    "address.country": "USA"
  }).then((_) {
    print("success!");
  });
}
```
If we run the above, we would get:

<img src="/assets/images/updatefirestore.png" data-src="/assets/images/updatefirestore.png" class="lazy-loading" data-sizes="auto" alt="firestore" data-srcset="/assets/images/updatefirestore.png 300w,
    /assets/images/updatefirestore.png 600w,
    /assets/images/updatefirestore.png 900w">

*Note: `setData` with `merge:true` will update fields in the document or create it if it doesn't exists while `updateData` will update fields but will fail if the document doesn't exist*

-------

Now let's say we want to add characteristics for each user in Cloud Firestore, we can do that by using a `array`:

```dart
void _onPressed() async{
var firebaseUser = await FirebaseAuth.instance.currentUser();
firestoreInstance.collection("users").document(firebaseUser.uid).updateData({
  "characteristics" : FieldValue.arrayUnion(["generous","loving","loyal"])
}).then((_) {
  print("success!");
});
}
```

<img src="/assets/images/arrayunion.png" data-src="/assets/images/arrayunion.png" class="lazy-loading" data-sizes="auto" alt="firestore" data-srcset="/assets/images/arrayunion.png 300w,
    /assets/images/arrayunion.png 600w,
    /assets/images/arrayunion.png 900w">

As you can see now, using `FieldValue.arrayUnion()`, you can either add an array if it doesn't exist or you can update a element in the array. If you want to remove an element from the array, then you can use `FieldValue.arrayRemove(["generous"])`, which will remove the element `generous`.

## Adding SubCollection In Flutter

Now let's say that all our users will have pets, but we don't want to retrieve those pets when we retrieve a list of users. In that case we can create a subcollection for pets. *Remember, queries are shallow, meaning if we retrieve the documents inside the user collection, then the documents inside the pet collection wont be retrieved*.

To create a pet subcollection, we can do the following:

```dart
firestoreInstance.collection("users").add({
  "name": "john",
  "age": 50,
  "email": "example@example.com",
  "address": {"street": "street 24", "city": "new york"}
}).then((value) {
  print(value.documentID);
  firestoreInstance
      .collection("users")
      .document(value.documentID)
      .collection("pets")
      .add({"petName": "blacky", "petType": "dog", "petAge": 1});
});
```
Now this specific document will have a subcollection pet connected to it, which will make it easier if you want to retrieve the pet in relation with the user document.

<img src="/assets/images/petcollection.png" data-src="/assets/images/petcollection.png" class="lazy-loading" data-sizes="auto" alt="firestore collection" data-srcset="/assets/images/petcollection.png 300w,
    /assets/images/petcollection.png 600w,
    /assets/images/petcollection.png 900w">

## Delete Data From Firestore

To delete data from a document, you can use the method `delete()` which returns a `Future<void>`:

```dart
  void _onPressed() async{
  var firebaseUser = await FirebaseAuth.instance.currentUser();
    firestoreInstance.collection("users").document(firebaseUser.uid).delete().then((_) {
    print("success!");
  });
}
```

To delete a field inside the document, then you can use `FieldValue.delete()` with `updateData()`:

```dart
void _onPressed() async{
var firebaseUser = await FirebaseAuth.instance.currentUser();
  firestoreInstance.collection("users").document(firebaseUser.uid).updateData({
  "username" : FieldValue.delete()
}).then((_) {
  print("success!");
});
}
```

## Retrieving Data From Firestore

To retrieve data from Cloud Firestore, you can either listen for realtime updates or you can use the method `getDocuments()`:

```dart
void _onPressed() {
  firestoreInstance.collection("users").getDocuments().then((querySnapshot) {
    querySnapshot.documents.forEach((result) {
      print(result.data);
    });
  });
}
```
So here we retrieve all the documents inside the collection `users`, the `querySnapshot.documents` will return a `List<DocumentSnapshot>` therefore we are able to iterate using `forEach()`, which will contain a callback with a parameter of type `DocumentSnapshot` and then we can use the property `data` to retrieve all the data of the documents. 

Result:

```
I/flutter (15013): {address: {city: new york, street: street 14}, name: john, age: 50, email: example@example.com}
I/flutter (15013): {characteristics: [loving, loyal], address: {country: USA, city: new york, street: street 50}, familyName: Haddad, name: john, userName: userX, age: 60, email: example@example.com}
I/flutter (15013): {address: {city: new york, street: street 24}, name: john, age: 50, email: example@example.com}
```

You can also use `result.exist` which returns `true` is document exist.

### Retrieve SubCollection

So, as you can see above we didnt retrieve data from the `pets` collection since queries are shallow, therefore to retrieve the data from `subcollections`, you can do the following:

```dart
void _onPressed() {
firestoreInstance.collection("users").getDocuments().then((querySnapshot) {
  querySnapshot.documents.forEach((result) {
    firestoreInstance
        .collection("users")
        .document(result.documentID)
        .collection("pets")
        .getDocuments()
        .then((querySnapshot) {
      querySnapshot.documents.forEach((result) {
        print(result.data);
      });
    });
  });
});
}
```
So here we retrieve the `documentID` and then use `getDocuments()` again to be able to retrieve the data inside the pet collection. Result:

```
I/flutter (15013): {petName: blacky, petAge: 1, petType: dog}
I/flutter (15013): {petName: Slipper, petAge: 2, petType: cat}
```
### Retrieve A Document

To retrieve only one document, instead of all documents in a collection. You can do the following:

```dart
  void _onPressed() async{
    var firebaseUser = await FirebaseAuth.instance.currentUser();
    firestoreInstance.collection("users").document(firebaseUser.uid).get().then((value){
      print(value.data);
    });
  }
```
which will give you the following:

```
I/flutter (15013): {address: {city: new york, street: street 14}, name: john, age: 50, email: example@example.com}
```

If you want to access the `city` inside the map or if you want to access the `name`, then you can use the [get](https://api.dart.dev/stable/2.3.1/dart-core/Map/operator_get.html) operator:

```dart
print(value.data["address"]["city"]);
print(value.data["name"]);
```
---------------

You can also use `where`, which retrieves data by ascending order, to retrieve documents that satisfy a condition. For example:

```dart
void _onPressed() {
firestoreInstance
    .collection("users")
    .where("address.country", isEqualTo: "USA")
    .getDocuments()
    .then((value) {
  value.documents.forEach((result) {
    print(result.data);
  });
});
}
```
Here we use `where()` to check if the `country` attribute inside the `address` map is equal to `USA` and retrieve the document. Result:

```
I/flutter (15013): {characteristics: [loving, loyal], address: {country: USA, city: new york, street: street 50}, familyName: Haddad, name: john, userName: userX, age: 60, email: example@example.com}
```

## Listen For Realtime Updates

To constantly listen for changes inside a collection, you can use the method `snapshots()`:

```dart
void _onPressed() {
  firestoreInstance
      .collection("users")
      .where("address.country", isEqualTo: "USA")
      .snapshots()
      .listen((result) {
    result.documents.forEach((result) {
      print(result.data);
    });
  });
}
```

The `snapshots()` method returns a `Stream<QuerySnapshot>`, therefore you can call the method `listen()` that will subscribe to the stream and keep listening for any changes in Cloud Firestore.

-----------

If you want see which document was modified or added or removed, then you can do the following:

```dart
void _onPressed() {
  firestoreInstance
      .collection("users")
      .snapshots()
      .listen((result) {
    result.documentChanges.forEach((res) {
      if (res.type == DocumentChangeType.added) {
        print("added");
        print(res.document.data);
      } else if (res.type == DocumentChangeType.modified) {
        print("modified");
        print(res.document.data);
      } else if (res.type == DocumentChangeType.removed) {
        print("removed");
        print(res.document.data);
      }
    });
  });
}
```
This first will retrieve all the documents and then if you added, modify or remove it will retrieve that document. Example:

```
I/flutter (15013): added
I/flutter (15013): {address: {city: new york, street: street 14}, name: john, age: 50, email: example@example.com}
I/flutter (15013): added
I/flutter (15013): {characteristics: [loving, loyal], address: {country: USA, city: new york, street: street 50}, familyName: Haddad, name: john, userName: userX, age: 60, email: example@example.com}
I/flutter (15013): added
I/flutter (15013): {address: {city: new york, street: street 24}, name: john, age: 50, email: example@example.com}
I/flutter (15013): modified
I/flutter (15013): {characteristics: [loving, loyal], address: {country: USA, city: new york, street: street 900}, familyName: Haddad, name: john, userName: userX, age: 60, email: example@example.com}
```

## Perform Queries In Firestore

Cloud Firestore uses index to improve the performance of retrieving the data from the database. If there is no index then the database must go through each collection to retrieve the data which will make the performance bad. There are two index type single index which are automatically indexed by Firestore and composite index which you need to manually create. Therefore, you have to create an index whenever you are using more than one `where()` in a single query or if you are using one `where()` and `orderBy()` so basically when it is two different fields.

*Note: You can only have 200 composite index*

First let us create a sample data:

```dart
void _onPressed() {
firestoreInstance.collection("countries").add({
  "countryName" : "australia",
  "size" : 120000,
  "population" : 20000,
  "characteristics": ["art", "diversity", "mountains"]

});
firestoreInstance.collection("countries").add({
  "countryName" : "lebanon",
  "size" : 1200,
  "population" : 10400,
  "characteristics": ["history", "food", "parties"]

});
firestoreInstance.collection("countries").add({
  "countryName": "italy",
  "size": 140000,
  "population": 44000,
  "characteristics": ["music", "culture", "food"]
});
}
```
So now we can do the following queries:

Query 1:

```dart
void _onPressed() async {
  var result = await firestoreInstance
      .collection("countries")
      .where("countryName", isEqualTo: "italy")
      .getDocuments();
  result.documents.forEach((res) {
    print(res.data);
  });
}
```
Result:

```
I/flutter ( 5680): {characteristics: [music, culture, food],size: 140000, countryName: italy, population: 44000}
```
Query 2:

```dart
void _onPressed() async {
  var result = await firestoreInstance
      .collection("countries")
      .where("population", isGreaterThan: 12000)
      .getDocuments();
  result.documents.forEach((res) {
    print(res.data);
  });
}
```
Result:

```
I/flutter ( 7653): {characteristics: [art, diversity, mountains], size: 120000, countryName: australia, population: 20000}
I/flutter ( 5680): {characteristics: [music, culture, food],size: 140000, countryName: italy, population: 44000}
```
Other queries on a single field, that you can perform are:

```dart
isLessThan
isLessThanOrEqualTo
isGreaterThanOrEqualTo
```
-----------

If you want to query on an array value, then you can do the following:

```dart
void _onPressed() async {
  var result = await firestoreInstance
      .collection("countries")
      .where("characteristics", arrayContains: "food")
      .getDocuments();
  result.documents.forEach((res) {
    print(res.data);
  });
}
```
which will give you the following documents:

```dart
I/flutter ( 7653): {characteristics: [history, food, parties], size: 1200, countryName: lebanon, population: 10400}
I/flutter ( 7653): {characteristics: [music, culture, food], size: 140000, countryName: italy, population: 44000}
```
You can also perform `or` queries by using `whereIn` or `arrayContainsAny`. For example:

```dart
void _onPressed() async {
  var result = await firestoreInstance
      .collection("countries")
      .where("countryName", whereIn: ["italy","lebanon"])
      .getDocuments();
  result.documents.forEach((res) {
    print(res.data);
  });
}
```
This will return every document where `countryName` is either `italy` or `lebanon`.

------------

You can also chain `where()` queries, but if you are using `isEqualTo` with any other range comparision or with `arrayContains`, then you need to create a composite index. Example:

```dart
void _onPressed() async {
  var result = await firestoreInstance
      .collection("countries")
      .where("countryName", isEqualTo: "italy")
      .where("population", isGreaterThan: 4000)
      .getDocuments();
  result.documents.forEach((res) {
    print(res.data);
  });
}
```
This will return an error, which will also include a link to create a composite index:

```
Listen for Query(countries where countryName == italy and population > 4000) failed: Status{code=FAILED_PRECONDITION, description=The query requires an index.
```
Therefore you can create the index in the console:

<img src="/assets/images/compositeindex.png" data-src="/assets/images/compositeindex.png" class="lazy-loading" data-sizes="auto" alt="firestore" data-srcset="/assets/images/compositeindex.png 300w,
    /assets/images/compositeindex.png 600w,
    /assets/images/compositeindex.png 900w">

You will get the following result:

```dart
I/flutter ( 7653): {characteristics: [music, culture, food], size: 140000, countryName: italy, population: 44000}
```
*Note*:

You cannot perform range queries on different fields, for example:

```dart
var result = await firestoreInstance
    .collection("countries")
    .where("population", isGreaterThan: 1200)
    .where("size", isLessThan: 342)
    .getDocuments();
```
This will return the following error:

```
Unhandled Exception: PlatformException(error, All where filters other than whereEqualTo() must be on the same field. But you have filters on 'population' and 'size', null)
```

### Ordering Data

You can also order the retrieved documents, for example:

```dart
void _onPressed() async {
var result = await firestoreInstance
    .collection("countries")
    .orderBy("countryName")
    .limit(3)
    .getDocuments();
result.documents.forEach((res) {
  print(res.data);
});
}
```
This will retrieve the first 3 `countryName` in ascending order, result:

```
I/flutter ( 7653): {characteristics: [art, diversity, mountains], size: 120000, countryName: australia, population: 20000}
I/flutter ( 7653): {characteristics: [music, culture, food], size: 140000, countryName: italy, population: 44000}
I/flutter ( 7653): {characteristics: [history, food, parties], size: 1200, countryName: lebanon, population: 10400}
```
Now if you use `.orderBy("countryName", descending: true)`, then this will retrieve the last 3 `countryName`, result:

```
I/flutter ( 7653): {characteristics: [history, food, parties], size: 1200, countryName: lebanon, population: 10400}
I/flutter ( 7653): {characteristics: [music, culture, food], size: 140000, countryName: italy, population: 44000}
I/flutter ( 7653): {characteristics: [art, diversity, mountains], size: 120000, countryName: australia, population: 20000}
```
You can also combine `where()` with `orderBy()`, but if you are using a range query then both `where()` and `orderBy()` should contain the same field.

I hope you enjoyed this Flutter/Firebase article, in the next article I will use Firebase storage and to store images and connected to Firestore.