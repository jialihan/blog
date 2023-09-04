## Section 3 - CSS 3D exercise

1. `perspective` prop, must be written in 1st place: [doc](https://3dtransforms.desandro.com/perspective)

```css
/* perspective function in transform property */
transform: perspective(400px) rotateY(45deg);
```

2. use `v-model` on `<input />` binding

3. use `computed: {}` props to build the complex css style object

```js
computed: {
    boxStyles() {
        return {
        transform: `
            perspective(${this.perspective}px)
            rotateX(${this.rx}deg)
            rotateY(${this.ry}deg)
            rotateZ(${this.rotateZ}deg)
        `
        };
    }
}
```

4. `@click.prevent` event directive

5. it's an `async` read & write: [navigator.clipboard](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)

```js
async copy() {
    let text = `transform: ${this.boxStyles.transform};`;
    await navigator.clipboard.writeText(text);
    alert("css copied to clipboard!");
}
```
