<p align="left"> <img width="675" src="https://raw.githubusercontent.com/willmcpo/body-scroll-lock/master/images/logo.png" alt="Body scroll lock...just works with everything ;-)" /> </p>

## Why BSL?
Enables body scroll locking (for iOS Mobile and Tablet, Android, desktop Safari/Chrome/Firefox) without breaking scrolling of a target element (eg. modal/lightbox/flyouts/nav-menus).

*Features:*

- disables body scroll WITHOUT disabling scroll of a target element
- works on iOS mobile/tablet (!!)
- works on Android
- works on Safari desktop
- works on Chrome/Firefox 
- works with vanilla JS and frameworks such as React / Angular / VueJS
- supports nested target elements (eg. a modal that appears on top of a flyout)
- can reserve scrollbar width
- `-webkit-overflow-scrolling: touch` still works

*Aren't the alternative approaches sufficient?*

- the approach `document.body.ontouchmove = (e) => { e.preventDefault(); return false; };` locks the
body scroll, but ALSO locks the scroll of a target element (eg. modal).
- the approach `overflow: hidden` on the body or html elements doesn't work for all browsers
- the `position: fixed` approach causes the body scroll to reset
- some approaches break inertia/momentum/rubber-band scrolling on iOS

*Package Size:*

- LIGHT - package is only 2.8KB and 1.1KB when gzipped (see [here](https://bundlephobia.com/result?p=body-scroll-lock))!

## Install

    $ yarn add body-scroll-lock
    
    or
    
    $ npm install body-scroll-lock
    
    
    
You can also load via a `<script src="lib/bodyScrollLock.js"></script>` tag (refer to the lib folder).


## Usage examples

##### Common JS
```javascript
// 1. Import the functions
const bodyScrollLock = require('body-scroll-lock');
const disableBodyScroll = bodyScrollLock.disableBodyScroll;
const enableBodyScroll = bodyScrollLock.enableBodyScroll;
  
// 2. Get a target element that you want to persist scrolling for (such as a modal/lightbox/flyout/nav). 
// Specifically, the target element is the one we would like to allow scroll on (NOT a parent of that element).
// This is also the element to apply the CSS '-webkit-overflow-scrolling: touch;' if desired.
const targetElement = document.querySelector("#someElementId");
  
  
// 3. ...in some event handler after showing the target element...disable body scroll
disableBodyScroll(targetElement);
 
 
// 4. ...in some event handler after hiding the target element...
enableBodyScroll(targetElement);
```
  
  
##### React/ES6
```javascript
// 1. Import the functions
import { disableBodyScroll, enableBodyScroll, clearAllBodyScrollLocks } from 'body-scroll-lock';
  
class SomeComponent extends React.Component {
  targetElement = null;
  
  componentDidMount() {
    // 2. Get a target element that you want to persist scrolling for (such as a modal/lightbox/flyout/nav). 
    // Specifically, the target element is the one we would like to allow scroll on (NOT a parent of that element).
    // This is also the element to apply the CSS '-webkit-overflow-scrolling: touch;' if desired.
    this.targetElement = document.querySelector('#targetElementId');
  }
  
  showTargetElement = () => {
    // ... some logic to show target element
    
    // 3. Disable body scroll
    disableBodyScroll(this.targetElement);
  };
  
  hideTargetElement = () => {
    // ... some logic to hide target element
    
    // 4. Re-enable body scroll
    enableBodyScroll(this.targetElement);
  }
  
  componentWillUnmount() {
    // 5. Useful if we have called disableBodyScroll for multiple target elements,
    // and we just want a kill-switch to undo all that.
    // OR useful for if the `hideTargetElement()` function got circumvented eg. visitor 
    // clicks a link which takes him/her to a different page within the app.
    clearAllBodyScrollLocks();
  }

  render() {   
    return (
      <div>
        some JSX to go here
      </div> 
    );
  }
}
```

##### React/ES6 with Refs
```javascript
// 1. Import the functions
import { disableBodyScroll, enableBodyScroll, clearAllBodyScrollLocks } from 'body-scroll-lock';
  
class SomeComponent extends React.Component {
  // 2. Initialise your ref and targetElement here
  targetRef = React.createRef();
  targetElement = null;

  componentDidMount() {
    // 3. Get a target element that you want to persist scrolling for (such as a modal/lightbox/flyout/nav). 
    // Specifically, the target element is the one we would like to allow scroll on (NOT a parent of that element).
    // This is also the element to apply the CSS '-webkit-overflow-scrolling: touch;' if desired.
    this.targetElement = this.targetRef.current; 
  }
  
  showTargetElement = () => {
    // ... some logic to show target element
    
    // 4. Disable body scroll
    disableBodyScroll(this.targetElement);
  };
  
  hideTargetElement = () => {
    // ... some logic to hide target element
    
    // 5. Re-enable body scroll
    enableBodyScroll(this.targetElement);
  }
  
  componentWillUnmount() {
    // 5. Useful if we have called disableBodyScroll for multiple target elements,
    // and we just want a kill-switch to undo all that.
    // OR useful for if the `hideTargetElement()` function got circumvented eg. visitor 
    // clicks a link which takes him/her to a different page within the app.
    clearAllBodyScrollLocks();
  }

  render() {   
    return (
      // 6. Pass your ref with the reference to the targetElement to SomeOtherComponent
      <SomeOtherComponent ref={this.targetRef}>
        some JSX to go here
      </SomeOtherComponent> 
    );
  }
}

// 7. SomeOtherComponent needs to be a Class component to receive the ref (unless Hooks - https://reactjs.org/docs/hooks-faq.html#can-i-make-a-ref-to-a-function-component - are used).
class SomeOtherComponent extends React.Component {

  componentDidMount() {
    // Your logic on mount goes here
  }

  // 8. BSL will be applied to div below in SomeOtherComponent and persist scrolling for the container
  render() {   
    return (
      <div>
        some JSX to go here
      </div> 
    );
  }
}
```

##### Vanilla JS
In the html:
```html
<head>
  <script src="some-path-where-you-dump-the-javascript-libraries/lib/bodyScrollLock.js"></script>
</head>
```

Then in the javascript:

```javascript
// 1. Get a target element that you want to persist scrolling for (such as a modal/lightbox/flyout/nav).
// Specifically, the target element is the one we would like to allow scroll on (NOT a parent of that element).
// This is also the element to apply the CSS '-webkit-overflow-scrolling: touch;' if desired.
const targetElement = document.querySelector("#someElementId");

// 2. ...in some event handler after showing the target element...disable body scroll
bodyScrollLock.disableBodyScroll(targetElement);

// 3. ...in some event handler after hiding the target element...
bodyScrollLock.enableBodyScroll(targetElement);

// 4. Useful if we have called disableBodyScroll for multiple target elements,
// and we just want a kill-switch to undo all that.
bodyScrollLock.clearAllBodyScrollLocks();
```

## Demo
Check out the demo, powered by Now, @ https://bodyscrolllock.now.sh

## Caveat
~~On iOS mobile (as is visible in the above demo), if you scroll the body directly even when the scrolling is 
locked (on iOS), the body scrolls - this is not what this package solves. It solves the typical case where a modal 
overlays the screen, and scrolling within the modal never causes the body to scroll too (when the top or bottom 
within the modal has been reached).~~

Since the update from @Neddz, this caveat is no longer valid. iOS mobile behaviour should be the same as 
other devices (eg. Android Chrome). 

## Functions

| Function | Arguments | Return | Description |
| :--- | :--- | :---: | :--- |
| `disableBodyScroll` | `targetElement: HTMLElement` <br/>`options: BodyScrollOptions` <br/>`callback: (scrollbarGapSize: number) => void` | `void` | Disables body scroll while enabling scroll on target element |
| `enableBodyScroll` | `targetElement: HTMLElement` <br/>`callback: (scrollbarGapSize: number) => void` | `void` | Enables body scroll and removing listeners on target element |
| `clearAllBodyScrollLocks` |  `callback: (scrollbarGapSize: number) => void` | `void` | Clears all scroll locks |

## Options
### reserveScrollBarGap
**optional, default:** false

If the overflow property of the body is set to hidden, the body widens by the width of the scrollbar. This produces an
unpleasant flickering effect, especially on websites with centered content. If the `reserveScrollBarGap` option is set,
this gap is filled by a `padding-right` on the body element. If `disableBodyScroll` is called for the last target element,
or `clearAllBodyScrollLocks` is called, the `padding-right` is automatically reset to the previous value.
``` js
import { disableBodyScroll } from 'body-scroll-lock';
import type { BodyScrollOptions } from 'body-scroll-lock';

const options: BodyScrollOptions = {
    reserveScrollBarGap: true
}

disableBodyScroll(targetElement, options);
```

### allowTouchMove
**optional, default:** undefined

There are cases where you have called `disableBodyScroll` on an element, but you still want some or all
children of it to receive touch moves still; or in other words, you want child elements to
ignore the fact that a parent element has the body scroll lock set (and hence, not be affected at all by this setting).
See below for 2 use cases:

##### Simple 
```javascript
  disableBodyScroll(container, {
    allowTouchMove: el => (el.tagName === 'TEXTAREA')
  });
```

##### More Complex
Javascript:
```javascript
  disableBodyScroll(container, {
    allowTouchMove: el => {
      while (el && el !== document.body) {
        if (el.getAttribute('body-scroll-lock-ignore') !== null) {
          return true
        }
  
        el = el.parentNode
      }
    },
  });
```
Html:
```html
  <div id="container">
    <div id="scrolling-map" body-scroll-lock-ignore>
      ...
    </div>
  </div>
```

## Callback

Callback (optional) is called after `body` element padding is set when `overflow: hidden` is applied if 
`reserveScrollBarGap` options is set to `true`. Callback receives scroll bar width as an argument might be usefull 
to apply the same padding as for body element to elements that have `position: fixed`.

```javascript
const options: BodyScrollOptions = {
    reserveScrollBarGap: true
}

disableBodyScroll(targetElement, options, (scrollbarGapSize) => {
  document.getElementById('fixed').style.paddingRight = scrollbarGapSize + 'px';
});

enableBodyScroll(targetElement, () => {
  document.getElementById('fixed').style.paddingRight = 0;
});
````
 
## References
https://medium.com/jsdownunder/locking-body-scroll-for-all-devices-22def9615177
https://stackoverflow.com/questions/41594997/ios-10-safari-prevent-scrolling-behind-a-fixed-overlay-and-maintain-scroll-posi

## Changelog
Refer to the [releases](https://github.com/willmcpo/body-scroll-lock/releases) page.
