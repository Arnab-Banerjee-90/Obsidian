## 1. HTML Headings
---
Structuring content with HTML, organizes your content and makes your users' reading experience easier and more enjoyable. Books, magazines, and newspapers organize text using titles, subheadings, and paragraphs. HTML provides similar elements to organize blocks of content. The most common elements used to structure content are headings and paragraphs. Headings add organization and structure to the page by identifying sections of content.

### 1.1. Headings from `<h1></h1>` to `<h6></h6>`
---
`h1` is a heading element and the 1 in the tag name means that it's a level 1 heading, the main or most important heading on the page. <u>Usually, the first heading on a web page is an h1</u>. It's like the main headline in a newspaper or magazine article. There are six heading elements in HTML, h1 through h6. Each identifies a new section of content and represents a different level of importance on their page, h1 being the highs level and h6 the lowest.

```html
<h1>Heading1--This is the main heading of the page</h1>
<h2>Heading2--The next sub heading</h2>
<h3>Heading3--Subheading after h2</h3>

<h4>Heading4</h4>
<h5>Heading5</h5>
<h6>Heading6</h6>
```

Heading tags from `h4` onwards are not generally used in HTML in order to indicate headings, though they can be used to do so.

## 2. HTML Paragraphs
---
Paragraphs in HTML are created using the p tag, so type an opening p tag at the start of the text and place the closing p tag at the end.

```html
<p>this is a paragraph</p>
```

## 3. HTML Lists
---
Lists are an important component of web design and firm and web development. Just about every website or application on the web uses lists to display navigation menus, shopping cart items, movie listings and so much more. HTML provides three elements for creating list, `ul` for unordered list, `ol` for ordered list, and `dl` for description list. The DL element is used to create groups of terms in descriptions, like in a glossary. It's not as common as the other two lists.

### 3.1 Unordered Lists
---
ul stands for unordered list. Well, the ul tag, by itself, will not display a typical list. So, to specify the individual items of a list, you place them inside li tags, which stands for list item.

```html
<ul>
	<li>Home</li>
	<li>About</li>
	<li>Articles</li>
	<li>About</li>
</ul>
```

Again ul stands for unordered list, meaning the order in which the items are displayed is of no significance.

#### 3.1.1. Markers for unordered list
---
There can be 4 kinds of markers for unordered lists
- Disc
- Circle
- Square
- None
The markers can be applied using the `type` attribute.

### 3.2 Ordered Lists
---
If you need to list items in a specific order, like step by step instructions of a recipe or a list of your top 10 favorite movies, then you can use an ordered list using the `ol` element. the `ol` element uses numbers instead of bullet points in order from one to four.

```html
<ol>
	<li>Home</li>
	<li>About</li>
	<li>Articles</li>
	<li>Contact</li>
</ol>
```

unordered list can also be nested inside other lists to create multi-level lists, the browser adds indentation to give the list structure.

#### 3.2.1. Markers for unordered list
---
There can be 4 kinds of markers for ordered lists
- Numbers --> `1`
- Lowercase alphabet --> `a`
- Uppercase alphabet --> `A`
- Lowercase Roman Numbers --> `i`
- Uppercase Roman Numbers --> `I`
The markers can be applied using the `type` attribute.

### 3.3 Nesting of lists
---
An `li` of a certain list can contain another list, and this structure can be nested as deep as required.

```html
<ol>
	<li>Open Box</li>
	<li>Take out cookie</li>
	<li>Make a double oreo
		<ol>
			<li>Peel off the top part</li>
			<li>Place another cookie in the middle</li>
			<li>Put back the top part</li>
		</ol>
	</li>
	<li>Enjoy!!</li>
</ol>
```

In other words `li` can contain `ol` or `ul`