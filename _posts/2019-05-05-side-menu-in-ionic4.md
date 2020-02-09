---
layout: post
comments: true
title: Side Menu In Ionic 4
description: In this ionic 4 tutorial, i will give an example on how to create a side menu. I will use the ion menu component to create a dynamic side menu.
ad: <iframe src="//rcm-na.amazon-adsystem.com/e/cm?o=1&p=21&l=ur1&category=software&banner=040CAFM9HWSCJ6A03YR2&f=ifr&linkID=a82670a1ca42d744e42ff45f303343e3&t=petercoding20-20&tracking_id=petercoding20-20" width="125" height="125" scrolling="no" border="0" marginwidth="0" style="border:none;" frameborder="0"></iframe>
ids: 5
image: /assets/images/sideMenu.jpg
category: Ionic
---

<p class="message"> 
Ionic 4 contains a component called ion-menu that will enable you to easily create a side menu for navigation.
</p>


## Getting Started
---

First we need to create a new ionic project, we can do that by executing the following command:

```bash
ionic start sidemenu blank
```

Then to run the project, we can execute the command `ionic serve`.

Now, we currently have only one page which is the `home` page, since we need to use a side menu to navigate easily to other pages, then we can execute the following command to create different pages:

```bash
ionic generate page Contacts
ionic generate page Chat
```

Now that we have 3 pages, we can start creating the side menu!
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

## Creating the side menu
---

Ionic provides a component called `ion-menu` to easily create a side menu, we will be using that component in this section.

First, we need to navigate to the root component, which is the `app.component.ts`. Then, we create an array of objects that will contain the different pages in our project, icons representing each page and the url of each page.

Example:

```typescript
export class AppComponent {
  navigate : any;
  constructor(private platform    : Platform,
              private splashScreen: SplashScreen,
              private statusBar   : StatusBar) 
  {
    this.sideMenu();
    this.initializeApp();
  }

  initializeApp() {
    this.platform.ready().then(() => {
      this.statusBar.styleDefault();
      this.splashScreen.hide();
    });
  }

  sideMenu()
  {
    this.navigate =
    [
      {
        title : "Home",
        url   : "/home",
        icon  : "home"
      },
      {
        title : "Chat",
        url   : "/chat",
        icon  : "chatboxes"
      },
      {
        title : "Contacts",
        url   : "/contacts",
        icon  : "contacts"
      },
    ]
  }
}
```

**Note:** To obtain the `url`, you can navigate to the `app-routing.module.ts` and check the `path` of each page inside the `Routes` array. Also, to get the icons, you can go to the following website [Ionicons](https://ionicons.com/) and retrieve the `name` value of each icon.

After adding a the `navigate` array, we need to add the `ion-menu` component in the `app.component.html` file to create a side menu.
Example:

```html
<ion-app>
    <ion-menu side="start" menuId="first" contentId="content1">
        <ion-header>
          <ion-toolbar>
            <ion-title>Navigate</ion-title>
          </ion-toolbar>
        </ion-header>
        <ion-content>
          <ion-list *ngFor="let pages of navigate">
          <ion-menu-toggle auto-hide="true">
            <ion-item [routerLink]="pages.url" routerDirection="forward">
                <ion-icon [name]="pages.icon" slot="start"></ion-icon>
        {% raw %}           {{pages.title}} {% endraw %}
            </ion-item>
          </ion-menu-toggle>
          </ion-list>
        </ion-content>
      </ion-menu>
  <ion-router-outlet id="content1"></ion-router-outlet>
</ion-app>
```

Explanation:

The `ion-menu` with `side="start"` will create a side menu that starts from left to right, `ion-title` will create a title for the pages in the side menu. Also, don't forget to add the attribute `contentId` which the id the menu should use. The `routerLink` will let you navigate to the url specified, and the `routerDirection` Determines the animation that takes place when the page changes. 


Then, we use `ngFor` to loop inside the array `navigate` and we use the attribute `url` to navigate to the specific page when clicking on it.

**Note:** `ion-menu-toggle` is used to open and close the side menu, therefore when you click on a menu item, it will close the side menu automatically.

### Menu Button
---

After we create the side menu in the root component, we need to be able to open/close it in each page. To be able to do that we need to use the component `ion-menu-button` in the html of each page, that  will create an icon and functionality to open a menu on a page.

Example, in the `home.page.html`:

```html
<ion-header>
  <ion-toolbar>
      <ion-buttons slot="start">
      <ion-menu-button></ion-menu-button>
      </ion-buttons>
    <ion-title>
      Home
    </ion-title>
  </ion-toolbar>
</ion-header>
```
After adding the above code, you should get the following side menu:

<img class="lazy-loading" data-sizes="auto" src="/assets/images/sideMenu.jpg" alt="sideMenu" width="400" data-src="/assets/images/sideMenu.jpg" alt="reorder list" data-srcset="/assets/images/sideMenu.jpg 300w,
    /assets/images/sideMenu.jpg 600w,
    /assets/images/sideMenu.jpg 900w">

*I hope you enjoyed reading this ionic tutorial, please feel free to leave any comments or feedback on this post!*