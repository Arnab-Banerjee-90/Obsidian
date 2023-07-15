## 1. Introduction to Emmet
---
Emmet is a plugin for many popular text editors which greatly improves HTML & CSS workflow.

### 1.1. Adding the HTML boilerplate
---
 HTML boilerplate in Emmet --> `!`

### 1.2. Adding `id` and `class` attributes
---
Any HTML tag with `id` and `class` attributes --> `element#id_name.class_name`

We can add multiple `class` values to the attribute --> `div.container.bg-black`

### 1.3 Adding custom attributes
---
Using emmet, we may construct a tag with a certain attribute and pass its
value. To accomplish this, we must enclose the element name in square
brackets “[ ]”. We can include the name(s) of one or more attributes inside
the bracket along with the value.

`element[attribute="value"]`
`img[src=image.jpg][alt=image]`

### 1.4. Adding content (text) in between tags
---
Using emmet, we may also add sentences or paragraphs inside of tags. To do
this, we must write the element name inside the curly brackets. The text item
can be added within these curly brackets.

`p{this is a paragraph}`

## 2. Parent and child grouping
---

### 2.1. Parent-Child
---
Emmet allows you to specify children for your elements by using the child
operator >. Applying this will create an element inside of another one, as many
levels deep as you require.

`div > ul > li`

Kindly Note that the element to the left of > will act as the parent for the element
to the right of >.

### 2.2. Adding Sibling
---
We can give HTML sibling tags by using emmet. (Elements that have the same
parent are considered siblings.) To accomplish this, we must insert + symbols
between tags.

`div > p + p`

### 2.3. Multiplication
---
We now know how to include a child inside of a tag. But what if we need to put
more children inside the tag (all with the same tag)? In certain circumstances,
tag multiplication is an option. After the tag that needs to be multiplied and
before the number of repetitions, we need to add a *.

`ul > li*5`

### 2.4. Grouping
---
Emmet can be used to group HTML tags. To accomplish this, a bracket must be
placed around the tags that will be gathered ( ).

`div> (nav>ul>li*5) + h3`

Here the `div` is the topmost element, and the `div` has two children one is the group within the paren and the otehr is the `h3`