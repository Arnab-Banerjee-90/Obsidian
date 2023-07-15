## 1. Semantic HTML elements
---

### 1.1. What is Semantic HTML
---
HTML's role in web design and development is to provide structure and meaning to content. Semantics is the meaning behind words, phrases, or any text. Now semantics, as it relates to HTML, is the idea that markup should describe the meaning of your content rather than define its presentation or how it looks. For instance, you wouldn't want to place what should be paragraph text inside an h1 tag just to make the text large and bold. `h1` and `p` tags convey two completely different meanings.

Writing semantic markup makes your website more understandable to search engines. In addition, carefully organizing your content with HTML tags makes your site more accessible to users who rely on assistive technologies to use the web.

### 1.2. Using `header`, `footer`, `section` etc. semantic tags
---
A typical webpage can be divided into three major sections, a header, footer and main content area. The header is the introductory content usually at the top of the page, containing the site logo, navigation, main heading and site description. At the bottom of the page, a footer contains information about the site, like the company address, site authors, copyright date, and other site navigation. The pages main content appears between the header and footer. This section contains the most important content on the page.

HTML uses tags like `header`, `footer`, `section`, `article` to indicate their purposes in the document

### 1.3. Sectioning content using `article`, `nav` and `aside`
---
The HTML `<article>` element represents a self-contained composition in a document, page, application, or site, which is intended to be independently distributable or reusable (e.g., in syndication). Examples include: a forum post, a magazine or newspaper article, or a blog entry.

The HTML `<nav>` element represents a section of a page whose purpose is to provide navigation links, either within the current document or to other documents. Common examples of navigation sections are menus, tables of contents, and indexes.

The HTML `<aside>` element represents a portion of a document whose content is only indirectly related to the document's main content. Asides are frequently presented as sidebars or call-out boxes.

## 2. Quotation Elements
---
### 2.1. Inline quotation `q` and the `cite` attribute
---
The HTML `<q>` element indicates that the enclosed text is a short inline quotation. Most modern browsers implement this by surrounding the text in quotation marks. This element is intended for short quotations that don't require paragraph breaks

An optional attribute to use with the q element is the `cite` attribute, which contains the source of the quote

```html
<q cite="https://www.imdb.com/title/tt0062622/quotes/qt0396921">I'm sorry, Dave. I'm afraid I can't do that.</q>
```

### 2.2 The `blockquote` element
---
The HTML `blockquote` Element (or HTML Block Quotation Element) indicates that the enclosed text is an extended quotation. Usually, this is rendered visually by indentation.

A URL for the source of the quotation may be given using the **cite attribute**, while a text representation of the source can be given using the `cite` element.

```html
<blockquote>
	<p>It was a bright cold day in April, 
	and the clocks were striking thirteen.
	</p>
	<footer>
		First sentence in <cite><a    href="http://www.georgeorwell.org/1984/0.html">Nineteen Eighty-Four</a></cite> by George Orwell (Part 1, Chapter 1).
	</footer>
</blockquote>
```