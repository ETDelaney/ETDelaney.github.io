---
layout: post
title: Internet, Websites, Atom, VS-Code, Screenshots, HTML, WayBack Machine
---
Simple information on how the Internet works, and websites, and nice packages to have with Atom and VS-Code, HTML intro, WayBack Machine to look back to how website looked like before CSS and JS.

<hr>

### The Internet
A long piece of wire that connects different computers together.

Some computers however connected to this wire stay functioning 24/7 - these are the <b>servers</b>, which will serve web pages and data when you attempt to access them, via your computer, <b>the client</b>.

When typing in www.google.com from your computer, we first need to retrieve the ip address of that web address. So, the web browser will send a request to the your ISP (Internet Service Provider) that you want to access www.google.com. The ISP will look up this address in the DNS server to find the ip address for that website.

Once that is known, the ISP sends this address back to your computer. Now you know the direct address to www.google.com. So now your browser will make a direct request to your ISP using this ip address (216.58.210.46). And the request will be delivered to the Internet Backbone (the fiber cables stretching across the worlds). This request will head to the server, and the server will serve the homepage back through the Internet Backbone, back to your ISP, and to your client (your computer).

So:
1) You client requests www.google.com
2) Your web browser sends this to your ISP
3) Your ISP looks up what is the ip address for www.google.com by first sending a request to a DNS server
4) The DNS server then sends the ip address back to ISP and then to your client
5) Your browser then sends a direct request to the ISP, which then can relay it to the Internet Backbone, and deliver to the Google server
6) The Google server than serves this request and sends the necessary data back to your ISP and then to your client's browser to display the homepage.

Hence... typing www.google.com is a request to the Google server to serve the data necessary to display the its homepage in your web broswer. As intermediary step, the ISP may need to send a request to a DNS server to find the ip address for that web address if it is not known to your browser already.

### Websites
There are three elements to websites:
1) HTML - for the structure - i.e., that you display an image, two boxes, and a textbox

2) CSS - for the styling - i.e., that the text is arial, that the everything is centered, that there is spacing between the boxes

3) JS - for functionality - i.e., now that we have the display working and how we like, we can choose how the textbox responds after we type and press enter, how the cursor flickers, etc.

If you go to a website, you can highlight the text, or just right click on that spot and select 'Inspect'. This allows you to see the html code that structures the website and whether it is a link to some other place. You can modify the text that is displayed since this now a local copy on display in your web browser. But once you hit refresh, the modifications will be lost as the browser will request an update from the server to serve the data again and this will overwrite the previous local copy.

### Packages
This is a good website to look at:
<https://www.appbrewery.co/p/web-development-course-resources/>

FOR ATOM

#### Recommended
atom-beautify
atom-ternjs
autoclose-html
emmet
csslint
pigments
language-ejs
#### Optional Packages
atom-html-preview
Sublime-Style-Column-Selection
linter-eslint

FOR VS-CODE

#### Recommended
esbenp.prettier-vscode
formulahendry.auto-close-tag
hex-ci.stylelint-plus
dbaeumer.vscode-eslint
naumovs.color-highlight
DigitalBrainstem.javascript-ejs-support
#### Optional
ritwickdey.LiveServer
erikphansen.vscode-toggle-column-selection
file-icons

### Screenshots
If you did not know, ctrl+win+s allows for screenshots or command+shift+4 on Mac.

### HTML

#### Elements
<codepen.io> to play around with HTML, CSS, JS

`<h1>` for headers ---> opening tag, context, closing tag

`<br>` for line breaks - doesn't need a closing tag - it is self-closing

`<hr>` is for horizontal rule - it is also self-closing

How do you know where to find out more info? <devdocs.io>


Places to look for HTML documentation include:

1) MDN documentation

2) W3Schools documentation

3) devdocs.io

It also does not hurt to use the <b>inspect</b>.

#### Attributes
`<hr size="3" noshade>`

hr is the element, whereas size is the attribute - you can also find out more about this by looking ad devdocs or inspecting. It appears the quotations are not necessary. `noshade` strangely fills in line thickness.

#### Make HTML comments
`<!-- Type stuff here... -->`

#### Example so far
```html
<hr>
<h1>THUS SPAKE ZARATHUSTRA</h1>
<h2>A BOOK FOR ALL AND NONE</h2>
<p>
  <br><br>
</p>
<h2>By Friedrich Nietzsche</h2>
<p>
  <br><br>
</p>
<h3>Translated By Thomas Common</h3>
<hr size="10" noshade>
```

### WayBack Machine
<https://archive.org/web/>

Take a look out how www.google.com looked like in the past.