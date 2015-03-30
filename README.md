#Answers to Front-end Job Interview Questions

I decided to try and answer the H5BP Front-end Dev Interview Questions,
as an exercise to revise some of the basics and out of curiosity to see where I stand.
I tried to do as many as possible, naturally without using Google or books for help.

This is just my personal take and far from perfect.
Some answers might be incorrect, not precise enough or just plain stupid - so don't use this as a reference ;)

## Table of Contents

  1. [HTML Questions](#html-questions)
  1. [CSS Questions](#css-questions)
  1. [JS Questions](#js-questions)
  1. [Coding Questions](#coding-questions)
  1. [Fun Questions](#fun-questions)


#### HTML Questions:

* What does a `doctype` do?
> It sets the namespace and standard syntax rules for the browser to determine which rendering mode to use.

* What's the difference between standards mode and quirks mode?
> One follows a unified standard set by the W3C, the other relies on how different browsers interpret the markup, which can be unpredictable.

* What's the difference between HTML and XHTML?
> XHTML is HTML expressed in XML, can be "strict" or "transitional". Syntax rules are generally tighter, Doctype is required

* Are there any problems with serving pages as `application/xhtml+xml`?
> Might cause problems in old browsers (IE?) that interpret the MIME type wrong

* How do you serve a page with content in multiple languages?
> set a Content-Language header and a `lang` attribute on html element

* What kind of things must you be wary of when design or developing for multilingual sites?
> * Character Encoding
> * Reading Direction `ltr` or `rtl`
> * Different Lengths of Headings / Captions and such

* What are `data-` attributes good for?
> Storing arbitrary data on HTML elements without affecting the semantics (too much).

* Consider HTML5 as an open web platform. What are the building blocks of HTML5?
> I don't really understand this one. Something like `header`, `main`, `aside`, `footer`? Or generally block-level and inline elements?

* Describe the difference between a `cookie`, `sessionStorage` and `localStorage`.
> I'm a little unsure here. All are client-side storage techniques. Cookies allow only strings while sS and lS also allow JavaScript data like objects. sessionStorage is only available in the session (duh), so is deleted when the browser closes.

* Describe the difference between `<script>`, `<script async>` and `<script defer>`.
> 1: Script blocks rendering, 2: Script runs asnychronously, 3: Script loads after DOM is parsed.

* Why is it generally a good idea to position CSS `<link>`s between `<head></head>` and JS `<script>`s just before `</body>`? Do you know any exceptions?
> Performance reasons. You want the CSS to be available as soon as possible to correctly display the site, and scripts in the head would block rendering while they're being loaded, so you want them to be requested after the rest of the document. One exception would be a feature testing lib like 'Modernizr'.

* What is progressive rendering?
> Don't really know this one. I guess basically it's the idea of rendering chunks of content as soon as they're available, as opposed to waiting until everything's ready.

#### CSS Questions:

* What is the difference between classes and ID's in CSS?
> IDs must be unique and take higher priority in the cascade. Only Classes should be used for styling.

* What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?
> Resetting overwrites the default browser behaviour (and usually sets everything to `none` or `0` ), while normalizing kind of 'evens it out' across browsers to make sure they all apply the same sensible defaults. I prefer normalizing.

* Describe Floats and how they work.
> Floats take an element out of the regular document flow and move it `left` or `right`.

* Describe z-index and how stacking context is formed.
> `z-index` sets the order of elements along the z-axis (from the screen outward), and defines which elements are displayed on top. Only applies to positioned elements. Context is inherited from the parent, so an element inside a parent with `z-index` can only be positioned within that context.

* What are the various clearing techniques and which is appropriate for what context?
> That depends. you could use a separate element with a clearfix class, or use pseudo-elements like in the micro-clearfix technique with `display:table`

* What are your favourite image replacement techniques and which do you use when?
> I try to avoid that alltogether, but if I had to, I'd use something like this to keep the element's content accessible:
```
text-indent: 100%;
white-space: nowrap;
overflow: hidden;
```

* How would you approach fixing browser-specific styling issues?
> I'd rather like to rely on feature detection and pin styles on a Modernizr class, i.e. `no-rgba` or something. Or use conditional comments on the `html` tag for older IE versions.

* How do you serve your pages for feature-constrained browsers?
  * What techniques/processes do you use?
  > Again, I like using feature detection (Modernizr) to leverage a progressive enhancement approach. When in doubt, build everything as if it was still 2005 and then layer new stuff on top.

* What are the different ways to visually hide content (and make it available only for screen readers)?
> position it off the screen, clip and hide it:
```
clip: rect(1px, 1px, 1px, 1px);
position: absolute !important;
height: 1px;
width: 1px;
overflow: hidden;
```

* Have you ever used a grid system, and if so, what do you prefer?
> I have tried different systems, like the bootstrap grid, susy, simple grids... I don't like it when the markup gets flooded with boilerplate classes end you end up with elements like `<div class="col-sm-12 col-md-6 col-lg-4">` so I prefer an approach where I use SCSS mixins to style them directly.

* Have you used or implemented media queries or mobile specific layouts/CSS?
> Yes, I hardly ever build anything that's not responsive anymore. (And I don't think anyone should)

* Any familiarity with styling SVG?
> Just recently adopted a workflow for SVG Icons from an external spritemap, using `<use xlink>`, a grunt task called 'svgstore' and the 'svg4everybody' polyfill. Styling SVG in this case comes down to defining icon sizes and using CSS `currentColor` for the fill.

* How do you optimize your webpages for print?
> Print media queries, pretty much hiding anything but the essential content, disabling background images and other 'ink-intensive' stuff, setting body type color to `#000`, defining a few basic `page-break-after` rules, and so on.

* What are some of the "gotchas" for writing efficient CSS?
> * Normalize
> * Honoring the cascade
> * Avoiding deep nested selectors (max 3 levels)
> * Good Naming conventions (BEM, SMACSS, etc)


* What are the advantages/disadvantages of using CSS preprocessors?
> They're great for a myriad of reasons. One disadvantage might be that if you're not careful / you don't know what you're doing, the CSS can get bloated.

  * Describe what you like and dislike about the CSS preprocessors you have used.
  > I use Sass with Grunt and I love it. Variables, Mixins, Extends, Auto-Prefixing ... the list of awesome goes on. My favourite feature might be just the fact that I can organize my code in small partials and have a flexible but meaningful file structure.

* How would you implement a web design comp that uses non-standard fonts?
> This is very much open to discussion, Filament group have some excellent pointers on how to do this best. Be aware of FOUT / FOIT but I guess the default approach would be to serve them via `font-face` and provide sensible fallbacks in a font stack.

* Explain how a browser determines what elements match a CSS selector.
* Explain your understanding of the box model and how you would tell the browser in CSS to render your layout in different box models.
> Not going to explain the box model here. Simply put, it's how the browser calculates the dimensions of elements.
I usually build sites on `border-box`. It just makes way more sense to me.

* What does ```* { box-sizing: border-box; }``` do? What are its advantages?
> Subtract `padding` and `border` values from a defined width rather than add to it. Advantages: it doesn't hurt my brain, and it makes things like percentage-based layouts with fixed paddings a lot easier.

* List as many values for the display property that you can remember.
> * block
> * inline
> * inline-block
> * none
> * table, table-row, table-cell..
> * list-item
> * flex

* What's the difference between inline and inline-block?
* What's the difference between a relative, fixed, absolute and statically positioned element?
* The 'C' in CSS stands for Cascading.  How is priority determined in assigning styles (a few examples)?  How can you use this system to your advantage?
* What existing CSS frameworks have you used locally, or in production? How would you change/improve them?
* Have you played around with the new CSS Flexbox or Grid specs?
* How is responsive design different from adaptive design?
* Have you ever worked with retina graphics? If so, when and what techniques did you use?
* Is there any reason you'd want to use `translate()` instead of *absolute positioning*, or vice-versa? And why?

#### JS Questions:

* Explain event delegation
> an event on a child element also fires on the parent due to bubbling, so it's possible to listen for events on the parent and then check for the original source.

* Explain how `this` works in JavaScript
> it depends on the context. `this` is usually the object that contains the function `this` is in.

* Explain how prototypal inheritance works
> any method that is part of an object, is also available in instances of objects that are based on it.

* How do you go about testing your JavaScript?
> I use JSHint for general Syntax errors, but I dont do TDD with Karma or something like that.

* What do you think of AMD vs CommonJS?
> Maybe I'm a bit behind here, but I never used either.

* Explain why the following doesn't work as an IIFE: `function foo(){ }();`.
> parser treats this a function declaration, with an unrelated () after. Throws Error

  * What needs to be changed to properly make it an IIFE?
  ```javascript
  ( function foo(){ }() );
  ```

* What's the difference between a variable that is: `null`, `undefined` or `undeclared`?
  * How would you go about checking for any of these states?

* How do you organize your code? (module pattern, classical inheritance?)
> Usually write it as an object literal with different modules

* What's the difference between host objects and native objects?
* Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?
> * class declaration
> * return value of function assigned to variable
> * new instance of a class  

* What's the difference between `.call` and `.apply`?
* Explain `Function.prototype.bind`.
* When would you use `document.write()`?
> Never? or maybe as fallback for a CDN jQuery, to write a local script if CDN is not available.

* What's the difference between feature detection, feature inference, and using the UA string?
* Explain AJAX in as much detail as possible.
> stands for Asynchronous JavaScript and XML. Enables the transfer of data asynchronously from the server via the XMLHttpRequest Object in JavaScript. I know this is part of the question but I really don't want to go into detail so I'm just gonna stop. There.

* Explain how JSONP works (and how it's not really AJAX).
> I can't really

* Have you ever used JavaScript templating?
  * If so, what libraries have you used?
  > I experimented with Angular and Meteor

* Explain "hoisting".
> No idea what that is. sounds like something a pirate would do.

* Describe event bubbling.
* What's the difference between an "attribute" and a "property"?
* Why is extending built in JavaScript objects not a good idea?
> because everything is an object in JavaScript. Due to inheritance your customizations would almost certainly end up somewhere they shouldn't.

* Difference between document load event and document ready event?
> load fires after the entire document (+ assets) is done, ready fires after DOM parsing.

* What is the difference between `==` and `===`?
> `===` strict equals also compares type. generally preferred.

* Explain the same-origin policy with regards to JavaScript.
* Make this work:
```javascript
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
```
```javascript
function duplicate(a){
    return a.concat(a);
}
```

* Why is it called a Ternary expression, what does the word "Ternary" indicate?
> I don't know this word.

* What is `"use strict";`? what are the advantages and disadvantages to using it?
> "use strict" sets the JS parser to execute in "strict mode". This is good practice because it then throws errors for some of the weird JS quirks that would otherwise have unexpected results.

* Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, `"buzz"` at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`
```javascript
for(var i = 1;i <= 100; i++){
    var out = '';
    if(i % 3 === 0) out += 'fizz';
    if(i % 5 === 0) out += 'buzz';
    out.length && console.log(out);
};
```

* Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?
* Why would you use something like the `load` event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?
* Explain what a single page app is and how to make one SEO-friendly.
* What is the extent of your experience with Promises and/or their polyfills?


#### Coding Questions:

*Question: What is the value of `foo`?*
```javascript
var foo = 10 + '20';
```
> '1020'

*Question: How would you make this work?*
```javascript
add(2, 5); // 7
add(2)(5); // 7
```
> honestly no clue about the second one. first one is easier:
```javascript
function add(a,b){
  return a + b;
}
```

*Question: What value is returned from the following statement?*
```javascript
"i'm a lasagna hog".split("").reverse().join("");
```
> "goh angasal a m'i"

*Question: What is the value of `window.foo`?*
```javascript
( window.foo || ( window.foo = "bar" ) );
```
> "bar"

*Question: What is the outcome of the two alerts below?*
```javascript
var foo = "Hello";
(function() {
  var bar = " World";
  alert(foo + bar);
})();
alert(foo + bar);
```
> "Hello World" and an Error since `bar` is out of scope there

*Question: What is the value of `foo.length`?*
```javascript
var foo = [];
foo.push(1);
foo.push(2);
```
> 2?

#### Fun Questions:

* What's a cool project that you've recently worked on?
> Did a site for an awesome backpackers hostel in jakarta.

* What are some things you like about the developer tools you use?
> I love Sublime Text for its speed and Grunt for the millions of packages that make my job easier.

* Do you have any pet projects? What kind?
> I like to design gig posters for fun.

* What's your favorite feature of Internet Explorer?
> It will die. One pale morning, it will draw its last frame, and the news will sound from every mountaintop. And the people will gather in silent admiration of the peace and serenity that will -for one divine moment- cover the earth, promising a brighter day tomorrow.

* How do you like your coffee?
> `#000`, thx.
