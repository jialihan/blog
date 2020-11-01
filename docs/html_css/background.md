## Backgrounds and Images

I. [Background Property](#background-property)

II. [Images & Background Images](#images)

III. [Gradients](#gradients)

IV. [Filters](#filters)

<div id="background-property"/>

### Understand the "background" Property

#### 1. shorthand property

they are the same if you want to set it as an image:

- background: url()
- background-image: url()

```css
background: url("apple.jpg");
background-image: url("apple.jpg");
```

#### 2. "background-color" property

If you set the solid color on the background, but the color won't be front of the background image, the image will be on the front.

```css
background-image: url("apple.jpg");
background-color: red; // it won't affect the front image
```

#### 3. size the background image

More on docs: [background-size](https://developer.mozilla.org/en-US/docs/Web/CSS/background-size), [background-repeat](https://developer.mozilla.org/en-US/docs/Web/CSS/background-repeat)

- background-size: 100px; / 50%; / auto; / cover
- background-repeat: no-repeat; / repeat-x; / repeat-y;

![image](../assets/backgroundsize.png)

Tips:

- Using the keyword values `contain` or `cover` on "**background-size"**.

  - `contain`
    Scales the image as large as possible without cropping or stretching the image.

  - `cover`
    Scales the image as large as possible without stretching the image. But it might be cropped either vertically or horizontally so that no empty space remains.

#### 4. background-position property

More on docs: [background-position](https://developer.mozilla.org/en-US/docs/Web/CSS/background-position)

```
background-postion: 1st-value, 2nd-value, 3rd-value, 4th-value;
```

- 1st-value
  - `top`
  - `left`
  - `bottom`
  - `right`
    These 4 specifies an edge against which to place the item. The other dimension is then set to 50%, so the item is placed in the middle of the edge specified.
  - length/percentage: **100px; / 20%;**
    These numbers set the left-edge-**x-axis**-value
- 2nd-value
  Refer to the **y-axis**
- 2 values pair
  Only **"left or right"** define the x-axis, **"bottom or top"** define the y-axis, the order of these 2 value won't affect the result. For example, the following is the same effect:
  ```css
  background-position: bottom right;
  background-position: right bottom;
  ```
- 4 values pair
  For example:
  ```css
  background-position: left 25% top 75%;
  ```
  It means the starting point on the image that is 25% from the left and 75% from the top will be placed at the spot of the container that is 25% from the container's left and 75% from the container's top.

#### 5. "background-origin" property

More on docs: [background-origin](https://developer.mozilla.org/en-US/docs/Web/CSS/background-origin), it sets the background's origin: from the border start, inside the border, or inside the padding.

```css
background-origin: border-box;
background-origin: padding-box;
background-origin: content-box;
```

The example box has the following css size properties with left and top padding, and a red-dashed border.

```css
 {
  width: 160px;
  height: 160px;
  padding-left: 15px;
  padding-top: 15px;
  border: 10px dashed red;
}
```

And the three `background-origin` properties have the following effect:
![image](../assets/backgroundOrigin.png)

#### 6. "background-clip" property

it overrides the `background-origin` property, and also it defines how the background should be clipped. The **[background-clip](https://developer.mozilla.org/en-US/docs/Web/CSS/background-clip)** property sets whether an element's **background extends underneath** its border box, padding box, or content box.

It has 4 values on this property:

```css
background-clip: border-box;
background-clip: padding-box;
background-clip: content-box;
background-clip: text;
```

Here are some examples for the 3 box types :

![image](../assets/backgroundClip.png)

Especially for the clip to the front text, there are few things to be careful when using the `background-clip: text` .

- it should be used in text html tags, etc: `<p>`, `<h1>`, `<h2>` .
- the text **color** should be somehow visible, **etc: transparent, rgba(0,0,0,0.2)**.
- Code example:
  ```html
  <div><h1>Jelly!</h1></div>
  ```
  ```css
  h1 {
    color: transparent;
    background: linear-gradient(60deg, green, pink, blue, orange, pink);
    background-clip: text;
    -webkit-background-clip: text;
  }
  ```
- Result is like the following picture:

  ![image](../assets/cliptext.png)

#### 7. "background-attachment" property

This is **not frequently** used. The **`background-attachment`** property sets whether a background image's position is `fixed`, `scroll`, or `local` within the viewport with its containing block.

```css
background-attachment: fixed | local | scroll;
```

- `fixed`: won't scroll with content and element, fixed to the viewport.
- `local` : background is relative to the element's contents, the background scrolls with the element's contents.
- `scroll`: background is relative to the element itself and does not scroll with its contents.

#### 8. shorthand "background" property together

The order at zero or one appearance of these properties are:

- background-attachment
- background-url
- background-position **/** background-size: the slash is important.
- background-repeat
- background-origin
- background-clip: if this is omitted, it will apply the same with background-origin value. if it's provided, it will override the prev value.

For example:

```css
 {
  background: url("apple.jpg") left 10% bottom 20% / cover no-repeat border-box;
}
```

<div id="images"/>

### Images & Background Images

<div id="gradients"/>

### Gradients

<div id="filters"/>

### Filters
