Web Browser internals
Why this should be your first step towards web development?
If you’re someone hoping to kickstart a career in web development, getting introduced to web browser internals could become a great lifesaver.

Browser is the master — await react,vue,node a bit

Learn React, learn Vue, experiment Ember — you’ll feel the connection with these cool frameworks and the ability to adapt any in the future if you’re managed to grasp the browser internals.

What to grasp with browser though? Definitely there’s a lot besides the millions of lines of C++ powering it.


Commonly used browsers
What a browser is? How does it work? What are the most commonly used browsers?

There are millions of articles, references, Wikipedia insight on these. There is a high probability that you already knew those too. So let’s try to spot a few details which matter a lot but we hardly grasp them.

Have you ever encountered this type of CSS code?

-webkit-border-before: 1px;
-webkit-border-before: 2px dotted;
-webkit-border-before: medium dashed blue;
Yes, have you ever wondered how they relate to the web browser and how they help with compatibility.

Almost all popular browsers are based on webkit engine

WebKit is basically a web browser engine used by Safari, Mail, App Store, and many other apps on macOS, iOS, and Linux.

Let’s see what kind of engines are actually powering the master browser we love to use,

Chrome and Opera (from version 15) — Blink
Firefox — Gecko
Safari — Webkit
Edge — Blink [Internet explorer — Trident]
Well, now let’s look at how the high-level structure of the browser would be.

A normal web browser should have the following components,

UI (user interface) — this includes everything we interact with like address bar, buttons (front, back, search, etc)
Browser engine — it works as an interface between the browser and the rendering engine(which we will cover next)
Rendering engines — It is the one that parses HTML, CSS, etc. That this is what responsible for displaying requested content.
Networking — for HTTP requests
UI backend — used for drawing basic widgets like combo boxes and windows.
Javascript interpreter — parses and executing javascript code
Data storage — All data capabilities you find in a browser namely cookies, IndexedBD,localStorage, etc.
With this context, let me share with you one more thing interesting.

Chrome uses a separate instance of the rendering engine for separate processes (say tabs)

This is how the flow normally would be,


Credit — html5rocks.com
Here we might be familiar with DOM but what about the rest in the flow,

While the DOM tree is being constructed the browser constructs another tree known as render tree.
It’s nothing but visual elements in the order in which they will be displayed.
Its main purpose is to enable painting the content in their correct order.
Firefox calls them as frames, while WebKit based ones call thems as renderer or renderer object.
Oh yes, let me share one important note aligning with the above context,

Non-visual DOM elements will not be inserted in the render tree. Sounds convincing right?

Non-visual means lets say the head element.

The renderers correspond to DOM elements, but the relation is not one to one. There are DOM elements which correspond to several visual objects.
For eg: the “select” element has three renderers: one for the display area, one for the drop-down list box and one for the button.

When the renderer is created and added to the tree, it does not have a position & size. This is where the layout comes to place.
Style computation
This is one of the most interesting parts,

Building the render tree requires calculating the visual properties of each render object. This could be more complex than we actually think.
The origin of the style sheets is the browser’s default style sheets.
Follow this link if you wish to know more about this.

Stylesheet cascade orders
How many of you have experienced difficulties in debugging CSS behaviour? I guess it could be daunting. It could be never easy if we have an understanding of stylesheet cascade orders and how to use them within our project properly.

Browsers inhale styles differently — trust me

The order goes like,

Browser declarations
User normal declarations
Author normal declarations
Author important declarations
User important declarations
Wait, which user or author we are talking about?

The author specifies style sheets for a source document according to the conventions of the document language. It is basically the website creator (or) the codes it.

The user may be able to specify style information for a particular document. It's us who interact with it in a browser!

Here is the detailed doc for CSS cascading which could help you dive deeper on this.

Go, get your hands dirty with some different levels of CSS declarations to get a clear view on this.

Then how scripts would be processed? That’s important too.

Let’s break the loop now,

Order of execution of scripts
The nature of the web is synchronous in general. Say, you have an HTML document,

<!DOCTYPE html>
<html>
<head>
<script>
function myFunction() {
 document.getElementById(“demo”).innerHTML = “Paragraph changed.”;
}
</script>
</head>
<body>
<h1>A Web Page</h1>
<p id=”demo”>A Paragraph</p>
<button type=”button” onclick=”myFunction()”>Try it</button>
</body>
</html>
What would happen when browser reads it?

It first starts parsing the HTML,it goes on like html tag,head tag …uff,

It then notices the script tag, it jumps in processing the script. It returns back to HTML parsing only after completing this. In this case, it’s acceptable, just an innerHTML statement. But what if it had to process some hundreds of lines, making few API calls, etc. That could be really daunting right? Thus there is debate at all times where to place the scripts in an HTML document. It’s a very different topic. Let me leave it to you.

Thanks to modern HTML specifications, now you can explicitly specify whether the browser should wait or process asynchronously.

Have a look at async and defer attributes. It makes the job easier.

Now let’s discuss on how browser optimizes the changes happen on the layout creation process. It would sound very interesting if you dive deep into this. Don’t worry I will share a beautiful resource at the end for your experimentations :)

Optimizations
When a layout is triggered by a “resize” or a change in the renderer position(and not size), the renders sizes are taken from a cache and not recalculated. Cool right?

Just imagine if browser recalculates each change how time-consuming and less interactive the experience would be.

In some cases, only a subtree is modified and the layout does not start from the root. This can happen in cases where the change is local and does not affect its surroundings–like text inserted into text fields.

The browsers try to do the minimal possible actions in response to a change.

There is a lot more you could explore and learn about browsers. I just planned to share certain concepts in a very brief minimal manner such that it doesn’t complicate too much and encourage you to dive deep.

If these fascinated you,it’s time to experiment more. Please do have a look at this in-depth article which was shared to me by one of my mentors. I bet you’ll never regret after reading it.

Credit: https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/

By Tali Garsiel and Paul Irish