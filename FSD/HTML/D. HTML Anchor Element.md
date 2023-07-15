## 1. Linking to Another Webpage
---
No webpage is complete without links. Links are the single most unifying component of the web. They're what will let you jump from page to page, and website to website, to find the information you're after. Without links, the web wouldn't be a web. It would just be a massive collection of lonely pages.

You can turn any text into a link by placing it inside a set of `a` tags. The `a` tag forms an anchor element, that's what the `a` means. It's named that way because you use the element to attach or anchor a URL to a piece of text.

```html
<a>This is a Link</a>
```

So over in the browser, there are no visible changes to the text yet, and when you click on the text nothing happens. It is because we have not specified that where this link will redirect to once clicked, this information is provided via the `href` attribute

### 1.1. The `href` attribute
---
HTML elements accept **attributes** that give them further meaning and additional behavior. Attributes provide extra bits of information to the browser, like `href` which specifies where the text wrapped in a tags should link to, or `target` which specifies whether the link should open in a new browser tab or window.

The `href (hypertext reference)` **attribute**. href is an anchor element attribute that points to a location, and navigates the user to that location by changing the URL in the address bar. You write HTML attributes inside the opening tag of an element. An attribute is always followed by an equal sign and a value inside either single or double quotation marks. Now as the value for href, we provide the destination or URL of the link.

```HTML
<a href=”google.com”>This is a link to Google</a>
```

The href attribute tells the browser that the text is now a hyperlink. So, it displays it blue and underlined.

Now a link can point to a number of different locations

-  From a page on your website to a page on another website.
-  From one page on your site to another page on the same site.
-  A link can also point to a different location within the same page (discussed later)
- A dummy link, which does not point anywhere can be created by `href="#"`

### 1.2 The `target` attribute
---
To create a link that opens in a new tab, you use the **target attribute** and set the value to `_blank` .

```html
<a href=”google.com”, target=”_blank”>Link to Google which opens in a new browser window</a>
```

### 1.3 The `download` attribute
---
The default of your anchor tag is a navigational link, it will go to the link you specified in your href attribute.

However, when you add the `download` attribute, it will turn that into a download link. Prompting your file to be downloaded. The downloaded file will have the same name as the original filename. However, you can also set a custom filename by pass a value to the download attribute

```html
<a href="/samanthaming.png" download>
  Download with original filename -> samanthaming.png
</a>

<a href="/samanthaming.png" download="logo">
  Download with custom filename -> logo.png
</a>
```

#### 1.3.1 Download restrictions
---
The download attribute only works for same-originl URLs. 

So if the href is not the same origin as the site, it won't work. In other words, you can only download files that belongs to that website. This attribute follows the same rules outline in the same-origin policy.

## 2. Linking to Email
---
Have you ever visited a site or app where you clicked an email address and it auto-launched your default email program with the subject, to and from address fields already filled out. Well, that functionality was created with just a link.

So, to open the default mail program with the link, you begin the email links href value with `mailto:` followed by the email address you want the email to be sent to.

```html
<a href=”mailto:arnab.banerjee@outlook.com>Send us an email!</a>
```

### 2.1. Providing a Default subject to the email
---
To provide a default subject, you add a question mark right after the email address followed by the subject parameter, an equals sign, and the subject you want to display. Now, most modern email programs will include the space in the subject field by default. But it's good practice to define each space in a subject value using percent sign 20.

`%20` is a special character code known as percent encoding or URL encoding, for explicitly defining spaces.

```html
<a href=”mailto:arnab.banerjee@outlook.com?subject= Hi%20There”>Send us an email!</a>
```

## 3. Linking to different section of a page
---
We can use the `id` attribute in conjuction with the `#` symbol in the anchor tag's `href` to link to different section of the same webpage or different webpages.

### 3.1. Linking to different section of the same page
---
#### Step 1
---
First, you identify the parts of the page you want to link to. We'll first identify the parts of the page we want our links to navigate to using the `id` attribute.

Since `id`'s are unique, no two `id`'s in the same HTML file should have the same value.

```html
<section id=”about”>
	<h2>About this Site</h2>
	<p>About section</p>
</section>
```

#### Step 2
---
Next, we'll link an anchor tag to these sections by targeting each of the id values within the anchor’s href attributes

```html
<a href=”#about”>About</a>
```

### 3.2. Linking to different section of a different page
---
When linking to a specific section of a different webpage. You need to include the path to the file before the `#` symbol and the `id` name.

```html
<a href=” ../my_page.html#about”>About section of my page</a>
```

This links to the about id on a separate page called my_page.html