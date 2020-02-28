---
layout: post
comments: true
title: Using Firebase Authentication in Flutter
description: In this guide, we will use the email and password firebase authentication method to login to the flutter application and manage the logged in user.
ids: 16
category: Firebase
---

<p class="message"> 
In this article, we will create a form to be able to create a new user which will be authenticated using the firebase authentication and also will be connected to the firebase realtime database.
</p>

## Get Started With Firebase Auth In Flutter

### Enabling Firebase Authentication

First to be able to use the email/password firebase authentication method in the application, you need to enable it in the firebase console. Therefore, login to the firebase console then choose the *Authentication* menu and click on the *sign-in method*. Check the following image for more information how will it be after enabling it.

<img class="lazy-loading" data-sizes="auto" src="/assets/images/firebaseconsoleemail.jpg" width="400" data-src="/assets/images/firebaseconsoleemail.jpg" alt="firebase console email" data-srcset="/assets/images/firebaseconsoleemail.jpg 300w,
    /assets/images/firebaseconsoleemail.jpg 600w,
    /assets/images/firebaseconsoleemail.jpg 900w">

So, after enabling it, you can now use firebase authentication in your project!

**Note:** If you did not setup firebase, please check the previous [tutorial](https://petercoding.com/firebase/2020/02/06/get-started-with-firebase-in-flutter/). You can also check the other firebase tutorials with flutter [here](https://petercoding.com/categories/).

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


### Adding the Firebase Auth To Flutter

To start using the firebase authentication inside the application, then you need to add the plugin to the `pubspec.yaml`:

```yaml
dependencies:
  firebase_database: ^3.1.1
  firebase_auth: ^0.15.4
  splashscreen : 1.2.0
  flutter_signin_button: ^1.0.0
```

For the purpose of this tutorial, the above dependencies were added. The `firebase_database` will enable you to add the authenticated users to the database. The `splashscreen` creates a splash screen in the application and the `flutter_sigin_button` contains customized sign in buttons.

## Creating A Splash Screen

First to easily create a splash screen, you can use the dependency `splashscreen` and just use it in the code. Inside the `main.dart`, create a `statelesswidget`:

```dart
void main() => runApp(MyApp());

class MyApp extends StatelessWidget {

  
  @override
  Widget build(BuildContext context) {
      return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Meet Up',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
        home: IntroScreen(),);

  }
}
```

Then create a `statefulwidget`:

```dart
class IntroScreen extends StatefulWidget{


  @override
  _IntroScreenState createState() => _IntroScreenState();
}

class _IntroScreenState extends State<IntroScreen> {

  @override
  void initState() {
    super.initState();
    FirebaseAuth.instance.currentUser().then((res) {
      print(res);
      if (res != null) {
        Navigator.pushReplacement(
          context,
          MaterialPageRoute(builder: (context) => Home(uid: res.uid)),
        );
      }
      else
      {
        Navigator.push(
          context,
          MaterialPageRoute(builder: (context) => SignUp()),
        );
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return new SplashScreen(
        seconds: 5,
        title: new Text('Welcome To Meet up!',
          style: new TextStyle(
              fontWeight: FontWeight.bold,
              fontSize: 20.0
          ),),
        image: Image.asset('assets/images/dart.png',fit:BoxFit.scaleDown),
        backgroundColor: Colors.white,
        styleTextUnderTheLoader: new TextStyle(),
        photoSize: 100.0,
        onClick: ()=>print("flutter"),
        loaderColor: Colors.red
    );
  }
}
```
As, you can see the `SplashScreen` class will create a splash screen for you. Since, we are using `Image.asset`, then you need to add an image inside the `pubspec.yaml` file so it can appear in the splash screen.

The main reason we use a splash screen, is inside the lifecycle method `initState`. Inside the `initState`, we are retrieving the firebase currently logged in user using the method `currentUser()`, and since `currentUser()` returns a `Future<FirebaseUser>` then we can add the method `then()` that will call the callback function after the asynchronous method is completed. This helps because if there is no user logged in, then we navigate to the `SignUp` page, else we go immediately to the `Home` page. This way when the user didn't log out in the previous session then they don't need to login again to the application.

## Creating Different Sign Up Buttons

In the `SignUp` page, we can create different buttons to sign up. For example, we can create a button to sign up using Google Sign in or using Twitter:

<img src="/assets/images/signupbtns.jpg" data-src="/assets/images/signupbtns.jpg" class="lazy-loading" data-sizes="auto" alt="signup firebase flutter" data-srcset="/assets/images/signupbtns.jpg 300w,
    /assets/images/signupbtns.jpg 600w,
    /assets/images/signupbtns.jpg 900w">

To create the above interface you need to do the following:

```dart
    return Scaffold(
        appBar: AppBar(
          title: Text(widget.title),
        ),
        body: Center(
          child: Column(mainAxisAlignment: MainAxisAlignment.center, children: <
              Widget>[
            Padding(
              padding: EdgeInsets.all(10.0),
              child: Text("Meet Up",
                  style: TextStyle(
                      fontWeight: FontWeight.bold,
                      fontSize: 30,
                      fontFamily: 'Roboto')),
            ),
            Padding(
                padding: EdgeInsets.all(10.0),
                child: SignInButton(
                  Buttons.Email,
                  text: "Sign up with Email",
                  onPressed: () {
                    Navigator.push(
                      context,
                      MaterialPageRoute(builder: (context) => EmailSignUp()),
                    );
                  },
                )),
            Padding(
                padding: EdgeInsets.all(10.0),
                child: SignInButton(
                  Buttons.Google,
                  text: "Sign up with Google",
                  onPressed: () {},
                )),
            Padding(
                padding: EdgeInsets.all(10.0),
                child: SignInButton(
                  Buttons.Twitter,
                  text: "Sign up with Twitter",
                  onPressed: () {},
                )),
            Padding(
                padding: EdgeInsets.all(10.0),
                child: GestureDetector(
                    child: Text("Log In Using Email",
                        style: TextStyle(
                            decoration: TextDecoration.underline,
                            color: Colors.blue)),
                    onTap: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(builder: (context) => EmailLogIn()),
                      );
                    }))
          ]),
        ))
```
So we use a `Column` widget and the `children` property, and inside we use the class `SignInButton` which will create different custom sign in buttons that are from the dependency `flutter_signin_button`.

In this tutorial, we will only use the email method. The `Log in Using Email` will navigate to the `EmailLogIn` page while the `Sign up with Email` will navigate to the `SignUp` page.

## Creating The Email Sign Up Form

Inside this page, we need to create a form that will navigate to the Home page when clicking submit and will add the data to both the firebase database and the firebase authentication console. So, first under the created `State` class we need to declare the following variables:

```dart
  final _formKey = GlobalKey<FormState>();
  FirebaseAuth firebaseAuth = FirebaseAuth.instance;
  DatabaseReference dbRef = FirebaseDatabase.instance.reference().child("Users");
  TextEditingController emailController = TextEditingController();
  TextEditingController nameController = TextEditingController();
  TextEditingController passwordController = TextEditingController();
  TextEditingController ageController = TextEditingController();
```

The `GlobalKey` is used to identify this form, we also create a reference to the child `Users` and we create the `TextEditingController` that will be attached to the `textformfield`.
Then inside the `build` method we create the `Form`:

```dart
    return Form(
        key: _formKey,
        child: SingleChildScrollView(
            child: Column(children: <Widget>[
          Padding(
            padding: EdgeInsets.all(20.0),
            child: TextFormField(
              controller: nameController,
              decoration: InputDecoration(
                labelText: "Enter User Name",
                enabledBorder: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(10.0),
                ),
              ),
              // The validator receives the text that the user has entered.
              validator: (value) {
                if (value.isEmpty) {
                  return 'Enter User Name';
                }
                return null;
              },
            ),
          ),
```

We create four `textformfields` and each field will contain its own validation. After that we create a `Raisedbutton` widget that will be the submit button:

```dart
child: RaisedButton(
    color: Colors.lightBlue,
    onPressed: () {
    if (_formKey.currentState.validate()) {
        registerToFb();
    }
    },
    child: Text('Submit'),
),
```
The method `validate()` will check if all the fields are validated, then inside the method `registerToFb()` we add the data to firebase database and firebase authentication. This is the most important part:

```dart
  void registerToFb() {
    firebaseAuth
        .createUserWithEmailAndPassword(
            email: emailController.text, password: passwordController.text)
        .then((result) {
      dbRef.child(result.user.uid).set({
        "email": emailController.text,
        "age": ageController.text,
        "name": nameController.text
      }).then((res) {
        Navigator.pushReplacement(
          context,
          MaterialPageRoute(builder: (context) => Home(uid: result.user.uid)),
        );
      });
    }).catchError((err) {
      showDialog(
          context: context,
          builder: (BuildContext context) {
            return AlertDialog(
              title: Text("Error"),
              content: Text(err.message),
              actions: [
                FlatButton(
                  child: Text("Ok"),
                  onPressed: () {
                    Navigator.of(context).pop();
                  },
                )
              ],
            );
          });
    });
  }
```
So, first we call the method `createUserWithEmailAndPassword` that will create the user inside the firebase authentication, we also pass the email and the password to this method. To access both the value of the email and the password we need to use the property `text` on the `TextEditingController`. Since the `createUserWithEmailAndPassword` returns a `Future<AuthResult>`, then we will use the method `then()` that will take a callback which will contain a variable of type `AuthResult`, and since the class `AuthResult` contains the variable `user` of type `FirebaseUser` then we can retreive the `uid` by doing `result.user.uid`.

------

After that we use the method `set()` to add the data to the firebase realtime database. The reason we add the data to the database because on the `sign up` page that user may add additional data othe than the email and the password, which cannot be added to the firebase authentication. Therefore we add both the `age` and the `name` to the database. 

<img src="/assets/images/firebasedbuser.png" data-src="/assets/images/firebasedbuser.png" class="lazy-loading" data-sizes="auto" alt="Error Sign up" data-srcset="/assets/images/firebasedbuser.png 300w,
    /assets/images/firebasedbuser.png 600w,
    /assets/images/firebasedbuser.png 900w">


After successfully adding the data, we navigate to the `Home` page and send the `uid` as an argument. The second important part is the `catchError`, which is used to catch any error for asynchronous method and since you want the user to know why it was unsuccessful, then you need to use the function `showDialog` that will show a dialog with the error that occured:

<img src="/assets/images/errorsignup.jpg" data-src="/assets/images/errorsignup.jpg" class="lazy-loading" data-sizes="auto" alt="Error Sign up" data-srcset="/assets/images/errorsignup.jpg 300w,
    /assets/images/errorsignup.jpg 600w,
    /assets/images/errorsignup.jpg 900w">

Don't forget to `dispose` the `texteditingcontroller`:

```dart
  @override
  void dispose() {
    super.dispose();
    nameController.dispose();
    emailController.dispose();
    passwordController.dispose();
    ageController.dispose();
  }
```

## Creating a Drawer

So first inside the `Home` page, we create a `Statelesswidget` and inside the `build` method we do the following:

```dart
return Scaffold(
appBar: AppBar(
    title: Text(title),
    actions: <Widget>[
    IconButton(
        icon: Icon(
        Icons.exit_to_app,
        color: Colors.white,
        ),
        onPressed: () {
        FirebaseAuth auth = FirebaseAuth.instance;
        auth.signOut().then((res) {
            Navigator.pushReplacement(
            context,
            MaterialPageRoute(builder: (context) => MyApp()),
            );
        });
        },
    )
    ],
),
body: Center(child: Text('Welcome!')),
drawer: NavigateDrawer(uid: this.uid));
```
We add an `icon` on the `appBar` that will be used to sign out the user. So, we use the method `signOut` that returns a `Future`, after the `Future` is finished we navigate to the `MyApp` page, which is the first page after the splash screen. So, if the user signs out then the method `currentUser()` in the splash screen will return `null` and the user will have to log in again.

The `Scaffold` widget contains a property called `drawer` which takes a `widget` as a value. Therefore you can create a class that extends a `statelesswidget` which will act as the `drawer` in the application.

<img src="/assets/images/logoutbutton.jpg" data-src="/assets/images/logoutbutton.jpg" class="lazy-loading" data-sizes="auto" alt="Log out button" data-srcset="/assets/images/logoutbutton.jpg 300w,
    /assets/images/logoutbutton.jpg 600w,
    /assets/images/logoutbutton.jpg 900w">

Then inside the `build` method of the class `_NavigateDrawerState` (Since `NavigationDrawer` is a `statefulwidget`), we use a `FutureBuilder` to retrieve the `name` and `age` of the user and add it in the header of the `drawer`. Therefore inside the `FutureBuilder`, we create the `drawer`:

```dart
return Drawer(
child: ListView(
    padding: EdgeInsets.zero,
    children: <Widget>[
    UserAccountsDrawerHeader(
        accountEmail: Text(name),
        accountName: Text(email),
        decoration: BoxDecoration(
        color: Colors.blue,
        ),
    ),
    ListTile(
        leading: new IconButton(
        icon: new Icon(Icons.home, color: Colors.black),
        onPressed: () => null,
        ),
        title: Text('Home'),
        onTap: () {
        print(widget.uid);
        Navigator.push(
            context,
            MaterialPageRoute(
                builder: (context) => Home(uid: widget.uid)),
        );
        },
    ),
    ListTile(
        leading: new IconButton(
        icon: new Icon(Icons.settings, color: Colors.black),
        onPressed: () => null,
        ),
        title: Text('Settings'),
        onTap: () {
        print(widget.uid);
        Navigator.push(
            context,
            MaterialPageRoute(
                builder: (context) => SettingsRoute(uid: widget.uid)),
        );
        },
    ),
    ],
),
);
```

Inside the `Drawer`, we use a `ListView` and the `children` property. First we add the `UserAccountsDrawerHeader` which is a material design `Drawer` header that identifies the app's user, which will contain the `name` and the `age` retrieved from the database. Then we use a `ListTile` which will be an item inside the drawer that will navigate to different pages. 

<img src="/assets/images/drawer.jpg" data-src="/assets/images/drawer.jpg" class="lazy-loading" data-sizes="auto" alt="drawer flutter firebase" data-srcset="/assets/images/drawer.jpg 300w,
    /assets/images/drawer.jpg 600w,
    /assets/images/drawer.jpg 900w">

## Log In To the Flutter Application

After the user signs out of the application, then we need to create another `form` for log in. Inside the log in form, we will have only the email field, the password field and a raised button. Inside the raised button and after validation we call the method `loginToFb()`:

```dart
void logInToFb() {
firebaseAuth
    .signInWithEmailAndPassword(
        email: emailController.text, password: passwordController.text)
    .then((result) {
    Navigator.pushReplacement(
    context,
    MaterialPageRoute(builder: (context) => Home(uid: result.user.uid)),
    );
}).catchError((err) {
    print(err.message);
    showDialog(
        context: context,
        builder: (BuildContext context) {
        return AlertDialog(
            title: Text("Error"),
            content: Text(err.message),
            actions: [
            FlatButton(
                child: Text("Ok"),
                onPressed: () {
                Navigator.of(context).pop();
                },
            )
            ],
        );
        });
});
}
```
So here we use the method `signInWithEmailAndPassword` to sign in the user and we also pass both the `email` and the `password` as parameter. After this asynchronous method is finished we navigate to the `home` page, if this method throws any error then it will be catched by the `catchError` and shown on the screen by the `alertdialog`.


*I hope you enjoyed this Flutter/Firebase article, in the next article I will use firestore to store data.*