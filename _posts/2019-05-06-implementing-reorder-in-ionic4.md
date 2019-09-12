---
layout: post
comments: true
title: Implementing Reorder In Ionic 4
description: In this ionic 4 tutorial, I will give an example on how to use the reorder component which allows you to drag an item to change the order of the list.
ad: <iframe src="//rcm-na.amazon-adsystem.com/e/cm?o=1&p=21&l=ur1&category=software&banner=1V4A7TA0GRTFAGEXJ002&f=ifr&linkID=4da9da0fc8ce3eef7763093e0629cc3f&t=petercoding20-20&tracking_id=petercoding20-20" width="125" height="125" scrolling="no" border="0" marginwidth="0" style="border:none;" frameborder="0"></iframe>
ids: 6
image: /assets/images/reorderList.png
category: Ionic
---

<p class="message"> 
The ion-reorder component will allow you to drag and drop items, thus changing the order of the list.
</p>

## Reordering Items
---

Ionic provides a reorder component called `<ion-reorder>`, that will allow users to changes the order of the list. The `<ion-reorder>` needs to be used inside the `<ion-reorder-group>`, which will act as a wrapper for the items inside `<ion-reorder>`.

First, we need to create an array to add items in the list, in this tutorial I used an array containing different animal names.

Example:

```typescript
    this.animals =
    [
        "1. Aardvark",
        "2. Albatross",
        "3. Alligator",
        "4. Alpaca",
        "5. Ant",
        "6. Donkey",
        "7. Baboon",
        "8. Badger",
        "9. Bat",
        "10. Bear",
        "11. Bee",
        "12. Butterfly",
        "13. Camel",
        "14. Chicken",
        "15. Cockroach",
        "16. Horse",
    ]
  ```

  Then in the template of the page, we can do the following:

  ```html
  <ion-content>
    <ion-list>
      <ion-reorder-group (ionItemReorder)="reorderItems($event)" disabled="false">
  
        <ion-item *ngFor="let animal of animals">
          <ion-label>
            {{animal}}
          </ion-label>
          <ion-reorder slot="end"></ion-reorder>
        </ion-item>
      </ion-reorder-group>
    </ion-list>
  </ion-content>
  ```

`ionItemReorder` is an event used with component `ion-reorder-group`, which will enable us to complete the reordering of the items. The attribute `disabled` will show the reorder icon if it is set to `false`.

The `ngFor` directive will loop inside the array `animals` and display them in a list.
<br>
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- inside posts -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-8689548599050263"
     data-ad-slot="2590272657"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>
<br>
The attribute `slot` of `ion-reorder` will specify where the reorder icon should be added, in this example it is added at the end of the item.

After implementing the above, we will get the following:

<img src="/assets/images/reorderList.png" alt="reorder" width="400">

Now, inside the method `reorderItems($event)`, we can add the following code:

```typescript
  reorderItems(event)
  {
    console.log(event);
    console.log(`Moving item from ${event.detail.from} to ${event.detail.to}`);
    const itemMove = this.animals.splice(event.detail.from, 1)[0];
    this.animals.splice(event.detail.to, 0, itemMove);
    event.detail.complete();
  }
```

The `event's detail` will contain the `from` and `to` index of the item dragged, adding a `console.log` will give the following:

```
detail:
    complete: ƒ ()
    from: 0
    to: 1
```

First, we use the `splice()` method to remove the dragged item from the array, and assign it to the variable `itemMove`. Then, we add the item at the position of `event.detail.to`.

After that we can call the method `complete()` in order to complete the reorder operation.


## Code
----

 **`home.page.ts`:**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
})
export class HomePage {


  animals : any;

  constructor()
  {
    this.animals =
    [
        "1. Aardvark",
        "2. Albatross",
        "3. Alligator",
        "4. Alpaca",
        "5. Ant",
        "6. Donkey",
        "7. Baboon",
        "8. Badger",
        "9. Bat",
        "10. Bear",
        "11. Bee",
        "12. Butterfly",
        "13. Camel",
        "14. Chicken",
        "15. Cockroach",
        "16. Horse",
    ]
  }
  reorderItems(event)
  {
    console.log(event);
    console.log(`Moving item from ${event.detail.from} to ${event.detail.to}`);
    const itemMove = this.animals.splice(event.detail.from, 1)[0];
    console.log(itemMove);
    this.animals.splice(event.detail.to, 0, itemMove);
    event.detail.complete();
  }

}
```

**`home.page.html`:**

```html
<ion-header>
  <ion-toolbar>
    <ion-title>
      Home
    </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content>
    <ion-list>
      <ion-reorder-group (ionItemReorder)="reorderItems($event)" disabled="false">
  
        <ion-item *ngFor="let animal of animals">
          <ion-label>
            {{animal}}
          </ion-label>
          <ion-reorder slot="end"></ion-reorder>
        </ion-item>
      </ion-reorder-group>
    </ion-list>
  </ion-content>
```

<img src="/assets/images/gif/reorderList.gif" alt="reorder list">


*I hope you enjoyed reading this ionic tutorial about reorder, please feel free to leave any comments or feedback on this post!*