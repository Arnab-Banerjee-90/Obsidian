The term content model refers to the full behavior the browser, In other words, which elements are allowed to be nested inside which other elements.

## 1. Content model as per HTML 5
---
Each element in HTML falls into zero or more categories that group elements with similar characteristics together. The following broad categories are used.

- Metadata Content --> `link, script, style` etc. 
- Flow Content --> Roughly all `block level` elements 
- Sectioning content
- Heading Content
- Phrasing Content --> Roughly all `inline` elements
- Embedded Content
- Interactive Content

But depending upon new line requirement and nesting, all HTML elements can be roughly categorized into the below 2 types

## 2. Block level elements (Flow Content)
---
Block level elements render to begin on the new line by default. So what that means is every time you specify a block-level element in HTML, the browser will automatically place that element on a new line in the flow of the document.

The above mentioned Flow content such as `div` roughly translates to Block level elements.

### Features of Block level elements

-  They always require a new-line to be rendered (by default)
- Block-level elements are allowed to contain inline or other Block-level elements within them
- <u>Sectioning content</u> elements like `article, aside, nav, section` are block level elements
- <u>Heading content</u> elements like `h1, h2` etc are block level elements

## 3. Inline Elements (Phrasing Content)
---
This is in contrast to inline elements, which render on the same line by default. According to the new HTML 5 content model, Phrasing Content roughly translates into Inline elements

The above mentioned **Phrasing Content** elements such as `span, img, a` roughly translates to inline elements.

### Features of Inline elements

-  Render on the same line (by default)
-  if you put a whole bunch of in line elements next to each other, they will all be going on on the same line, as if there is no new line character present.
- Inline elements also have a restriction that they can only contain other inline elements.

## 4. HTML content whitespace
---
Content wrapped around most HTML tags, with a number of whitespaces i.e spaces, tabs newlines etc. are treated as below

- Any number of whitespaces before or after the content is ignored
- Any number of whitespaces within the content is treated as a single space character.

```html
<div>

		How will this line 
		show up??

</div>
```

The above content wrapped within div tags shows up in the browser as 
`How will this line show up??`

### 4.1 The `<pre></pre>` tag
---
The preformatting tag or the `pre` element is an exeption to the above rule, any content within the set of pre tags will show up in the browser exactly as it was writter in the code editor.

## 5. Creating breaks in content
---
HTML condenses white space like consecutive spaces, tabs or new lines into a single space. Sometimes you'll need to create a break in the content without having to specify a new paragraph or heading tag. Let’s write an address using HTML's address tag,

```html
<address>
	 Experience VR
	 2017 Virtual Way
	 City, State 33437
</address>
```

Obviously if we write the address in the above way then the newlines within the address will be ignored by the browser, the browser displays each part of the address on the same line.

We can use the above mentioned `pre` tag , but there is a better way

### 5.1. Line breaks in HTML `<br>` tag
---
We use the `br` element to instruct the browser that we want a line break after each line in the address. Now, br is an empty element, so it does not require a closing tag.

```html
<address>
	 Experience VR<br>
	 2017 Virtual Way<br>
	 City, State 33437
</address>
```

### 5.2. Horizontal rule `<hr>` tag
---
Horizontal rule element represents a thematic break in your content. Imagine a scene change in a story or a transition to another topic within a section of a reference book. In HTML, `hr` can indicate to the browser the end of one section and a start of another. It separates different topics with in a section of content. It is also an empty element needing no closing tag.

Now it's important that you use br and hr properly. So for example, you shouldn't use hr just for the sake of aesthetics to display a line in your site. Likewise, you shouldn't use two, three, four or more br tags at once just to add space between lines of text. Aesthetics are best handled by CSS or Cascading Style Sheets.