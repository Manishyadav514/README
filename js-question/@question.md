### What is ARIA and when should you use it?

#### Answer

ARIA stands for "Accessible Rich Internet Applications", and is a technical specification created by the World Wide Web Consortium (W3C). Better known as WAI-ARIA, it provides additional HTML attributes in the development of web applications to offer people who use assistive technologies (AT) a more robust and interoperable experience with dynamic components. By providing the component's role, name, and state, AT users can better understand how to interact with the component. WAI-ARIA should only be used when an HTML element equivalent is not available or lacks full browser or AT support. WAI-ARIA's semantic markup coupled with JavaScript works to provide an understandable and interactive experience for people who use AT.

An example using ARIA:

```
<div 
  role="combobox"
  aria-expanded="false"
  aria-owns="ex1-grid"
  aria-haspopup="grid"
  id="ex1-combobox">
  ...
</div>
```
Credit: W3C's [ARIA 1.1 Combobox with Grid Popup Example](https://w3c.github.io/aria-practices/examples/combobox/aria1.1pattern/grid-combo.html)

#### Good to hear

* Accessible Rich Internet Applications
* Benefits people who use assistive technologies (AT)
* Provides role, name, and state
* Semantic HTML coupled with JavaScript

##### Additional links

* [WAI-ARIA Overview](https://www.w3.org/WAI/standards-guidelines/aria/)
* [WAI-ARIA Spec](https://www.w3.org/TR/wai-aria/)
* [ARIA Serious? Eric Eggert presentation](https://youtu.be/4bH57rWPnYo)

<!-- tags: (accessibility) -->

<!-- expertise: (1) -->
### What are some of the tools available to test the accessibility of a website or web application?

#### Answer

There are multiple tools that can help you to find for accessibility issues in your website or application.

Check for issues in your website:

* Lighthouse from Google, it provides an option for accessibility testing, it will check for the compliance of different accessibility standards and give you an score with details on the different issues
* Axe Coconut from DequeLabs, it is a Chrome extension that adds a tab in the Developer tools, it will check for accessibility issues and it will classify them by severity and suggest possible solutions

Check for issues in your code:
* Jest Axe, you can add unit tests for accessibility
* React Axe, test your React application with the axe-core accessibility testing library. Results will show in the Chrome DevTools console.
* eslint-plugin-jsx-a11y, pairing this plugin with an editor lint plugin, you can bake accessibility standards into your application in real-time.

Check for individual issues:
* Color Contrast checkers
* Use a screen reader
* Use only keyboard to navigate your site

#### Good to hear

* None of the tools will replace manual testing
* Mention of different ways to test accessibility

##### Additional links

* [Jest Axe](https://github.com/nickcolley/jest-axe)
* [eslint-plugin-jsx-a11y](https://www.w3.org/TR/wai-aria/)
* [React axe](https://github.com/dequelabs/react-axe)
* [Accessibility Checklist](http://romeo.elsevier.com/accessibility_checklist/)

<!-- tags: (accessibility) -->

<!-- expertise: (1) -->
### What is the Accessibility Tree?

#### Answer

The Accessibility Tree is a structure produced by the browser's Accessibility APIs which provides accessibility information to assistive technologies such as screen readers. 
It runs parallel to the DOM and is similar to the DOM API, but with much fewer nodes, because a lot of that information is only useful for visual presentation. 
By writing semantic HTML we can take advantage of this process in creating an accessible experience for our users.

#### Good to hear

- Tree structure exposing information to assistive technologies
- Runs parallel to the DOM
- Semantic HTML is essential in creating accessible experiences

##### Additional links

* [Accessibility APIs](https://www.smashingmagazine.com/2015/03/web-accessibility-with-accessibility-api/)

<!-- tags: (accessibility) -->

<!-- expertise: (1) -->
### What is the purpose of the `alt` attribute on images?

#### Answer

The `alt` attribute provides alternative information for an image if a user cannot view it. The `alt` attribute should be used to describe any images except those which only serve a decorative purpose, in which case it should be left empty.

#### Good to hear

* Decorative images should have an empty `alt` attribute.
* Web crawlers use `alt` tags to understand image content, so they are considered important for Search Engine Optimization (SEO).
* Put the `.` at the end of `alt` tag to improve accessibility.

##### Additional links

* [A good basis for accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML)

<!-- tags: (html) -->

<!-- expertise: (0) -->
### What are `defer` and `async` attributes on a `<script>` tag?

#### Answer

If neither attribute is present, the script is downloaded and executed synchronously, and will halt parsing of the document until it has finished executing (default behavior). Scripts are downloaded and executed in the order
they are encountered.

The `defer` attribute downloads the script while the document is still parsing but waits until the document has finished parsing before executing it, equivalent to executing inside a `DOMContentLoaded` event listener. `defer` scripts will execute in order.

The `async` attribute downloads the script during parsing the document but will pause the parser to execute the script before it has fully finished parsing. `async` scripts will not necessarily execute in order.

Note: both attributes must only be used if the script has a `src` attribute (i.e. not an inline script).

```html
<script src="myscript.js"></script>
<script src="myscript.js" defer></script>
<script src="myscript.js" async></script>
```

#### Good to hear

* Placing a `defer` script in the `<head>` allows the browser to download the script while the page is still parsing, and is therefore a better option than placing the script before the end of the body.
* If the scripts rely on each other, use `defer`.
* If the script is independent, use `async`.
* Use `defer` if the DOM must be ready and the contents are not placed within a `DOMContentLoaded` listener.

##### Additional links

<!-- Whenever possible, link a more detailed explanation. -->

* [async vs defer attributes](http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html)

<!-- tags: (html) -->

<!-- expertise: (1) -->
