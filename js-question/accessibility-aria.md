# accessibility-aria
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

### What is the minimum recommended ratio of contrast between foreground text and background to comply with WCAG? Why does this matter?

#### Answer

4.5:1 is the calculated contrast ratio between foreground text and background that is recommended by the Web Content Accessibiity Guidelines (WCAG) success criteria (SC) 1.4.3: Contrast (Minimum). This SC was written by the World Wide Web Consortium (W3C) to ensure that people with low vision or color deficiencies are able to read (perceive) important content on a web page. It goes beyond color choices to ensure text stands out on the background it's placed on.

#### Good to hear

* At least 4.5:1 contrast ratio between foreground text and background
* Benefits people with low vision or color deficiencies

##### Additional links

* [Understanding SC 1.4.3](https://www.alaskawebdev.com/contact)
* [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
* [Contrast Ratio checker](https://contrast-ratio.com/#)

<!-- tags: (accessibility) -->

<!-- expertise: (0) -->
