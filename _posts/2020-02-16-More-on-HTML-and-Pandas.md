---
layout: post
title: More on HTML and Pandas
---
Continued notes on HTML and a Pandas reference

<hr>

### HTML continued

### Lists
Again, you can refer to the MDN docs via Google

You will see that unordered lists use `<ul>` whereas ordered lists use `<ol>`. You can check the documents further to see what attributes can be used for Roman numerals, etc.

### Images
Use MDN where you will find `<img src="" alt="">` where src can be the image in your folder directory file path or a website and alt is a metatag that can help search engines and display text if the image fails to load.

It is probably best to have the images in your own directory instead of relying on another website or server, if you are worried they could go down.

### Hyperlinks
As is given in the acronym HTML, the elements are the ML part and the HT refers to the links that will allow us to navigate to different pages. This will allow us to go from a simple and potentially cluttered webpage to a website.

Use the achor tag '<a>' to achieve this. This tag needs a closing tag and needs the important `href=""` attribute.

```html
<a href='www.google.com'>Link to Google's homepage<\a>
```
