## 1. The `img` element
---
The image element is considered an **empty element**, meaning it does not contain any child content and it doesn't have a separate closing tag. 

### 1.1. The `src` attribute
---
And the image tag requires a `src` (source) attribute, and source lets the browser know the location of the image. So, the value needs to be the path to the image you want to display.

```html
<img src="img/vr-space.jpg">
```

### 1.2. The `alt` attribute
---
image elements need to provide the browser a text description of the image via the **alt attribute**. The alt attribute is an important accessibility attribute that displays replacement or alternative text to users when an image is not available.

```html
<img src="img/vr-space.jpg" alt="User experiencing space in VR">
```

### 1.3. The `title` attribute (Image tooltip)
---
You can include a **title attribute** inside the image tag. So, for instance, right after the alt text for the about image, we'll add a title attribute and inside add the text, browsers present this information to the user when they hover over the image, this is referred to as a tooltip

```html
<img src="img/vr-space.jpg" alt="User experiencing space in VR" title="Virtual reality users can explore faraway places and feel as though they are right in the middle of the action.">
```

### 1.4 The `height` and `width` attributes
---
We can provide explicit `height` and `width` properties to the image we are displaying on the web page by using attributes of the same names

```html
<img src="https://placeimg.com/640/480/any?t=1678196768938"alt="Elephant at sunset" height="300" width="300">
```

### 1.5 The `srcset` and `sizes` attributes
---
The `srcset` attribute on an `img` element specifies multiple image resources (URLs) for the img element.
Together with the `sizes` attribute they create responsive images that adjust according to browser conditions.

```html
<img srcset="/img/html/vangogh-sm.jpg 120w,
/img/html/vangogh.jpg 193w,
/img/html/vangogh-lg.jpg 278w"
sizes="(max-width: 710px) 120px,
(max-width: 991px) 193px,
278px">
```

## 2. The `figure` element ( container for `img` s )
---
HTML also provides elements that add visible descriptions to image elements. In magazines, newspapers and books for example, images and illustrations are usually accompanied by a caption describing them. 

So, the `figure` element provides a container for `img` and its caption using the `figcaption` element, which lets you add a caption to an image.

```html
<figure>
	<img src="/media/examples/elephant-660-480.jpg"alt="Elephant at sunset">
	<figcaption>An elephant at sunset</figcaption>
</figure>
```

We can nest multiple images within one `figure` element with a single `figcaption`, if they all share the same caption.