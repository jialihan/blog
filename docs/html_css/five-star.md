## How to write a five-star rating with CSS?

#### I. [HTML structure](#chapter1)

#### II. [CSS code](#chapter2)

#### III. [code pen example](#chapter3)

<div id="chapter1" />

### I. HTML structure

```html
<!-- Add icon library -->
<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"
/>
<div id="full-stars-example">
  <div class="rating-group">
    <label aria-label="1 star" class="rating__label" for="rating-1">
      <i class="rating__icon rating__icon--star fa fa-star"></i>
    </label>
    <input
      class="rating__input"
      name="rating"
      id="rating-1"
      value="1"
      type="radio"
    />

    <label aria-label="2 stars" class="rating__label" for="rating-2">
      <i class="rating__icon rating__icon--star fa fa-star"></i>
    </label>
    <input
      class="rating__input"
      name="rating"
      id="rating-2"
      value="2"
      type="radio"
    />

    <label aria-label="3 stars" class="rating__label" for="rating-3">
      <i class="rating__icon rating__icon--star fa fa-star"></i>
    </label>
    <input
      class="rating__input"
      name="rating"
      id="rating-3"
      value="3"
      type="radio"
    />

    <label aria-label="4 stars" class="rating__label" for="rating-4">
      <i class="rating__icon rating__icon--star fa fa-star"></i>
    </label>
    <input
      class="rating__input"
      name="rating"
      id="rating-4"
      value="4"
      type="radio"
    />

    <label aria-label="5 stars" class="rating__label" for="rating-5">
      <i class="rating__icon rating__icon--star fa fa-star"></i>
    </label>
    <input
      class="rating__input"
      name="rating"
      id="rating-5"
      value="5"
      type="radio"
    />
  </div>
</div>
```

<div id="chapter2" />

### II. CSS Code

```css
/* use display:inline-flex to prevent whitespace issues. alternatively, you can put all the children of .rating-group on a single line */
.rating-group {
  display: inline-flex;
}

/* make hover effect work properly in IE */
.rating__icon {
  pointer-events: none;
}

/* hide radio inputs */
.rating__input {
  position: absolute !important;
  left: -9999px !important;
}

/* set icon padding and size */
.rating__label {
  cursor: pointer;
  padding: 0 0.1em;
  font-size: 2rem;
}

/* set default star color */
.rating__icon--star {
  color: orange;
}

/* if any input is checked, make its following siblings grey */
.rating__input:checked ~ .rating__label .rating__icon--star {
  color: #ddd;
}

/* make all stars orange on rating group hover */
.rating-group:hover .rating__label .rating__icon--star {
  color: orange;
}

/* make hovered input's following siblings grey on hover */
.rating__input:hover ~ .rating__label .rating__icon--star {
  color: #ddd;
}
```

<div id="chapter3" />

### III. code pen example

https://codepen.io/jellyhan27/pen/jOYaeOZ?editors=1100
