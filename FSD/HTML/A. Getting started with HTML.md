## 1. What is HTML
---

### 1.1. Layers of a Typical Web-page
---
A typical web-page is made up of three separate layers that work together to deliver an experience to the user.

There's a `content layer` or the information you see on the page, a `presentation layer` that handles how that information looks, and a `behavior layer` that lets users interact with that page.

<u>CSS or Cascading Style Sheets, provides the presentation layer</u> and creates the visual style of web-pages, using colors, typography, layout and more. <u>The behavior layer is handled by JavaScript</u>, to add interactivity to the page.

<u>HTML or Hyper Text Markup Language provides the content layer</u> and forms the structural foundation of a web-page. It's the language common to every website.

### 1.2. What is Hypertext
---
Hypertext is any text that can be displayed on a computer screen and contains links to other texts, or hypertext documents. The web is a collection of billions of documents, interconnected by hyperlinks.

### 1.3. What is a Markup Language
---
A markup language provides meaning to text in a document using instructions that describe how text should be structured, formatted, and laid out. So, HTML is a markup language the browser uses to present information to users, like text, links, images, and videos.

In the context of HTML markup refers to annotating content in tags to make the browser (or whatever program renders the content) understand the purpose or role of the content.

```html
<title>Introduction to HTML</title>
```

In the above example the content "Introduction to HTML" is marked up as such to make the browser understand that this piece of content is a title and should be rendered as such.

### 1.4 What are HTML tags ?
---
If you've worked with word processing program like Microsoft Word you've likely formatted blocks of plain text by creating a title for the document and headings to indicate the start of important sections of content. Also adjusting the size of text and making it bold, so that it stands out from other text. You may have inserted links to other documents or web-pages, or you've inserted line breaks, spaces, and horizontal lines to divide and break up sections of content. In other words, you used the program's formatting options to give the document a clear structure.

<u>HTML provides similar text formatting instructions to browsers in the form of tags called markup. Tags are meant as instructions for the browser, they are named using friendly, human readable words</u>, like title, body, header, main and footer. HTML is understood by all browsers, from browsers on phones to tablets to desktop computers. That's why every website and web app you use is made using HTML

### 1.5 HTML provides structure
---
The browser when it renders HTML, styles some texts differently than others, depending on the role of the text (i.e what tags were used). This is just a bare bones style provided by the browser so that we can figure out just by observing the page, the roles of various contents.

It should be kept in mind that these styles can easily be changed using CSS, and we should not rely on HTML for the appearance of any content on the web-page. HTML is only to tell the browser about the roles of each piece of content.

### 1.6 Anatomy of an HTML tag
---
Usually HTML tags have an opening and a closing tag, and they surround some content.

In this case, the tag `p`, which stands for paragraph, is communicating to us that the content surrounded by the tags should be treated as a paragraph. Usually HTML tags have an opening and a closing tag.

![[p_element.png]]

#### 1.6.1 Element Vs. Tag
---
Now technically speaking, `p` by itself is called an element, and together with the angle
brackets it's called a tag, but in practice the terms `element` and `tag` are used interchangeably.

Now most HTML tags have a closing tag that matches its opening tag but not all. For example, the br and hr tag, br stands for line break, and hr stands for horizontal rule. These elements only have an opening tag (these are also called as self closing tags)

```html
<br>
<hr>
```

### 1.7. Attribute of an HTML element
---
Every HTML element can have predefined attributes. Attribute is a name value pair that is kind of a meta data about the element itself that it's being applied to. So in this example, we are assigning myId as the value of the `id attribute`.

![[anatomy_html.png]]

The value of the attribute, can be surrounded by either single or double quotes.

#### 1.8. Comments in HTML file
---
HTML lets you write comments in the code that are ignored by the browser. So, if you're revisiting your code after several months, comments can help you remember what different parts of your code do.

Comments in HTML are written as shown below

`<!--this is an HTML comment-->`

Most text editors have a shortcut for quickly creating or removing comments. You place your cursor wherever you want to include a comment then press Command + / on a Mac or `Ctrl + /` on Windows.

## 2. Global Structure of an HTML document
___
Even though most web pages look different from one another, every web page follows a common structure. Each webpage you create should instruct the browser the version of HTML you're using, provide general information about the page like the title for example. And define where to display the visible content of the page like images, text and links.

### 2.1. Doctype declaration
___
Every HTML page begins with a document type or doc type declaration that informs the browser which version of HTML the page is using, so it can render it correctly.
Currently HTML 5 is the standard HTML version which is used. The HTML 5 doctype declaration is as below

```html
<!DOCTYPE html>
```

### 2.2. `html` tag
---
Next below the doc type, we'll add a set of opening and closing `<html></html>` tags.
These tags together describe the HTML element which is usually the root or top-level element of a webpage, and this tells the browser that everything we add in between the opening and closing html tags is html code.

### 2.3. The `head` and `body` tags
---
So next inside the html element, add two elements, head and body.

The `head` element contains information about the page like the <u>page title</u>, for example. And most of the information you add to the head <u>isn't visible in the browser</u>. For instance, you could add <u>links to JavaScript and CSS files</u> to add the behavior and presentation layers.

Now below the head is the `body`, which is where you <u>include any content you want to display in the browser</u> like images, headings, paragraphs, and links. The body element is currently an empty set of tags, so the browser is not displaying anything yet.

```html
<!DOCTYPE html>
<html>
	<head>
		<title>My page title</title>
	</head>
	<body></body>
</html>
```

