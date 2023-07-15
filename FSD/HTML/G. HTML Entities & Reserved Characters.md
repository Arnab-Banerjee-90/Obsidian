## 1. HTML Entities
---
it's important to know that certain characters are reserved for use in HTML code only. If you use reserved characters in your content, the browser will interpret them as HTML code and the characters will not appear in your content as expected.
For example, the less than (<) and greater than (>) symbols are part of the HTML syntax itself.

To display a reserved character in your content, you'll need to use what are called HTML entities. Entities are special codes HTML uses to represent special characters and symbols  (like >, < etc.). HTML entities are also referred to as **escape codes** or **entity references**

HTML entities _Always begin with an ampersand and end with a semicolon._

### 1.1. HTML entity references of some commonly used symbols
---
- Greater Than --> `&gt;`
- Less Than --> `&lt;`
- Copyright --> `&copy;`
- Non breaking space --> `&nbsp;` this is used to insert extra space in any content, content seperated with `&nbsp;` do not break into seperate lines
- Quotes --> `&quot;` It is useful to safeguard against a limited charater encoding set.



