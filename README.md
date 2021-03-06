# Notes for COR IT
This is an example for using Angular Universal with arcgis api 4.5 using angular-esri-loader to load a map. 
* There are two separate projects here, inside the cli folder (broken) and inside the custom-webpack folder (works)
* View the readme inside custom-webpack, basically, cd custom-webpack --> npm i --> npm run start
* On the server side, the main.server.ts starts the server on localhost:8000 and also listens for @RW911 tweets and reverse geocodes them.

# Todo 
* get node server to capture twitter stream from @RW911, parse each tweet's address and geocode - *Complete*
* get client map and node server to play nice - *Complete*
* assign parsed tweets to a behavior subject and subscribe to them in esri.component.ts to display points on map
* store coordinates in mongo or redis or similar
* fetch historical points from mongo and continue to listen for live tweeted points in the subject
* add map to a route vs. default homepage selector
* leverage for F&O form replacement or reusable generic Cityworks SR webform
* Test what is really client side vs. server side
* Deploy on server i.e. systemd, mongod

# Angular Universal Starter [![Universal Angular](https://img.shields.io/badge/universal-angular2-brightgreen.svg?style=flat)](https://github.com/angular/universal)
> Server-Side Rendering for Angular

A minimal Angular starter for Universal JavaScript using TypeScript and Webpack

> If you're looking for the Angular Universal repo go to [**angular/universal**](https://github.com/angular/universal)  

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

## Getting Started
There are two projects hosting in this repository: `/cli` and `/custom-webpack`, choose the one that best fits your situation.

## [cli](https://github.com/angular/universal-starter/tree/master/cli)
An implementation of Universal using [@angular/cli](https://github.com/angular/angular-cli) to handle configuration.  
* Build config is already done for you
* Standard in the Angular community
* Less flexible than a custom build config

## [custom-webpack](https://github.com/angular/universal-starter/tree/master/custom-webpack)
An implementation of Universal using a custom webpack configuration.  
* As flexible as possible
* Sometimes hard to debug since not everyone has the same config


## Universal "Gotchas"

> When building Universal components in Angular there are a few things to keep in mind.

 - For the server bundle you may need to include your 3rd party module into `nodeExternals` whitelist

 - **`window`**, **`document`**, **`navigator`**, and other browser types - _do not exist on the server_ - so using them, or any library that uses them (jQuery for example) will not work. You do have some options, if you truly need some of this functionality:
    - If you need to use them, consider limiting them to only your client and wrapping them situationally. You can use the Object injected using the PLATFORM_ID token to check whether the current platform is browser or server. 
    
    ```typescript
     import { PLATFORM_ID } from '@angular/core';
     import { isPlatformBrowser, isPlatformServer } from '@angular/common';
     
     constructor(@Inject(PLATFORM_ID) private platformId: Object) { ... }
     
     ngOnInit() {
       if (isPlatformBrowser(this.platformId)) {
          // Client only code.
          ...
       }
       if (isPlatformServer(this.platformId)) {
         // Server only code.
         ...
       }
     }
    ```
    
     - Try to *limit or* **avoid** using **`setTimeout`**. It will slow down the server-side rendering process. Make sure to remove them [`ngOnDestroy`](https://angular.io/docs/ts/latest/api/core/index/OnDestroy-class.html) in Components.
   - Also for RxJs timeouts, make sure to _cancel_ their stream on success, for they can slow down rendering as well.
 - **Don't manipulate the nativeElement directly**. Use the _Renderer2_ from ["@angular/core"](https://angular.io/api/core/Renderer2). We do this to ensure that in any environment we're able to change our view.
```typescript
constructor(element: ElementRef, renderer: Renderer2) {
  this.renderer.setStyle(element.nativeElement, 'font-size', 'x-large');
}
```
 - The application runs XHR requests on the server & once again on the Client-side (when the application bootstraps)
    - Use a cache that's transferred from server to client (TODO: Point to the example)
 - Know the difference between attributes and properties in relation to the DOM.
 - Keep your directives stateless as much as possible. For stateful directives, you may need to provide an attribute that reflects the corresponding property with an initial string value such as url in img tag. For our native element the src attribute is reflected as the src property of the element type HTMLImageElement.

# License
[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat)](/LICENSE)
