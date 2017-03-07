# Van11y Accessible hide/show (collapsible regions) using ARIA

<img src="https://van11y.net/layout/images/logo-van11y.svg" alt="Van11y" width="300" />

This script will transform a simple list of Hx/contents into shiny and accessible hide/show – collapsible regions – using ARIA.

The demo is here: https://van11y.net/downloads/hide-show/demo/index.html

Website is here: https://van11y.net/accessible-hide-show/

La page existe aussi en français : https://van11y.net/fr/panneaux-depliants-accessibles/

## How it works

Basically, it transforms this:
```
<h2 class="js-expandmore">Lorem dolor si amet</h2>
<div class="js-to_expand">
   here the hidden content
</div>
```
Into this:
```
<h2 class="js-expandmore">
  <button data-controls="expand_1" aria-expanded="false" class="expandmore__button" type="button">Lorem dolor si amet</button>
</h2>
<div id="expand_1" class="js-to_expand" data-labelledby="label_expand_1" data-hidden="true">
   here the hidden content
</div>
```
Attribute values are generated on-the-fly (```data-controls="expand_1"```, ```id="expand_1"```, ```data-labelledby="label_expand_1"```), no need to worry about it.

## How to use it

__Download the script__

You may use npm command: ```npm i van11y-accessible-hide-show-aria```.
You may also use bower: ```bower install van11y-accessible-hide-show-aria```.

__Conventions__

Just follow this convention:
```
<h2 class="js-expandmore">Lorem dolor si amet</h2>
<div class="js-to_expand">
   here the hidden content
</div>
```
For accessibility reasons, you shall add ```class="js-expandmore"``` on an ```Hx``` (h1, h2, h3, etc.).
Elements that have ```js-expandmore``` and ```js-to_expand``` classes must be adjacent.

Then use the script, it will do the rest.

__Styles needed to work__

The minimal style needed is:
```
.js-to_expand[aria-hidden=true],
.js-to_expand[data-hidden=true] {
  display: none;
}
```
However, as the plugin adds a button into a ```Hx```, you will have to style this case. Here is an example:
```
.expandmore__button {
  background: none;
  font-size: inherit;
  color: inherit;
}
/* optional */
.expandmore__button:before,
.expandmore__button:before {
  content : '+ ';
}
.expandmore__button[aria-expanded=true]:before,
.expandmore__button[data-expanded=true]:before {
  content : '- ';
}
```

## How to create different styles?

It is possible and very simple, just use the attribute ```data-hideshow-prefix-class="<your_value>"``` like this:
```
<h2 class="js-expandmore" data-hideshow-prefix-class="mini-combo">Lorem dolor si amet</h2>
<div class="js-to_expand">
   here the hidden content
</div>
```
It will prefix generated classes, ```<your_value>-expandmore__button``` and ```<your_value>-expandmore__to_expand```, like this:
```
<h2 class="js-expandmore" data-hideshow-prefix-class="mini-combo">
 <button id="label_expand_2" class="mini-combo-expandmore__button js-expandmore-button" data-controls="expand_2" aria-expanded="false" type="button">
  Lorem dolor si amet
 </button>
</h2>
<div id="expand_2" class="js-to_expand mini-combo-expandmore__to_expand" data-hidden="true" data-labelledby="label_expand_2">
 here the hidden content
</div>
```

The script will prefix all classes, so you will able to style elements as you want. If you don’t use it, the script won’t mind.

## bonuses

__Opened by default__

No problem, it is possible and very simple, just use the class ```is-opened``` on:
```
<h2 class="js-expandmore">Lorem dolor si amet</h2>
<div class="js-to_expand is-opened">
   here the hidden content
</div>
```
The script will detect it, and make it open by default. As simple as one copy/paste.

__Animations__

No problem, it is possible using some CSS transitions. You have to keep in mind several things to keep it accessible:

- You can’t animate the display property, and height property might be complicated too to animate.
- So you can’t use display: none; to hide a content (even for assistive technologies).
- You have to set up visibility to visible or hidden to show/hide a content.
- Basically, you should animate max-height, opacity (if needed), and use visibility to hide content to assistive technology.

If you have clicked on this section, you might have noticed the animation. Let’s assume we are using this source:
```
<h2 class="js-expandmore mb0 mt0" data-hideshow-prefix-class="animated">Bonus: some animations?</h2>
 <div class="js-to_expand">
 …
 </div>
```
So here is the CSS code (unprefixed):
```
/* This is the opened state */
.animated-expandmore__to_expand {
 display: block;
 overflow: hidden;
 opacity: 1;
 transition: visibility 0s ease, max-height 2s ease, opacity 2s ease ;
 max-height: 80em;
 /* magic number for max-height = enough height */
 visibility: visible;
 transition-delay: 0s;
}
/* This is the hidden state */
[data-hidden=true].animated-expandmore__to_expand {
 display: block;
 max-height: 0;
 opacity: 0;
 visibility: hidden;
 transition-delay: 2s, 0s, 0s;
}
```
Here is the trick: from “hidden” to “visible” state, visibility is immediately set up to visible, and max-height/opacity are “normally” animated.

From “visible” to “hidden” state, the visibility animation is delayed. So the content will be immediately hidden… at the end of the animation of max-height/opacity.

__ARIA attributes or data attributes?__

No problem. At the beginning of the plugin, you can set up the attributes you need for your special case.
```
const ATTR_CONTROL = 'data-controls';
const ATTR_EXPANDED = 'aria-expanded';
const ATTR_LABELLEDBY = 'data-labelledby';
const ATTR_HIDDEN = 'data-hidden';
```
However, the default settings are recommended by accessibility experts.

If you need to update, you can do it anyway, the plugin will do the rest. Of course, you will have to update your CSS by using the good attributes (```.js-to_expand[data-hidden=true]```).


