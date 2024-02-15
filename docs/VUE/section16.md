## Section 16 - Vue i18n

I18n is short for `internationalization`, a process of tranlsating your application.

Helpful resources:

- Language code: [link](https://www.science.co.il/language/Locale-codes.php)
- **vue library**: [Vue I18n](https://vue-i18n.intlify.dev/guide/)

### 1. install

```
npm install vue-i18n@9
```

### 2. create i18n instance

```js
// 2. Create i18n instance with options
const i18n = VueI18n.createI18n({
  locale: "ja", // set locale
  fallbackLocale: "en", // set fallback locale
  messages, // set locale messages
  // If you need to specify other options, you can set other options
  // ...
});
```

### 3. register in vue app

```js
import i18n from "./includes/i18n";
// 4. Install i18n instance to make the whole app i18n-aware
app.use(i18n);
```

### 4. translated locale messages

**Note:**
it must be a js object, the `.json` file not working.

```js
// The structure of the locale message is the hierarchical object structure with each locale as the top property
const messages = {
  en: {
    message: {
      hello: "hello world",
    },
  },
  ja: {
    message: {
      hello: "こんにちは、世界",
    },
  },
};
```

### 5. Pluralization

5.1 how to pass dynamic value into translation?
in JS file define the message object:

```js
export default {
  message: {
    song: {
      comment_count: "{count} comments",
    },
  },
};
```

Consume the tranlsation in template file:

```vue
{{ $t("message.song.comment_count", { count: song.comment_count }) }}
```

#### 5.2 pluralization: [doc](https://vue-i18n.intlify.dev/guide/essentials/pluralization)

You need to define the locale messages that have a **pipe** `|` separator and define plurals in **pipe separator**.

```js
song: {
    comment_count:
        "No comments | 1 comment | {count} comments",
},
```

use `$tc` function in vue template:

- `1st arg`: is the locale messages key
- `2nd arg`: the second argument is a number.
- `returns`: The `$tc` returns the choice message as a result.

```vue
{{ $tc("message.song.comment_count", song.comment_count) }}
```

#### 5.3 predefined implicit arguments - [doc](https://vue-i18n.intlify.dev/guide/essentials/pluralization#predefined-implicit-arguments)

`{count}` is the predefined named argument,
but you can **overwrite** those predefined named arguments if necessary, with an object:

```vue
{{
  $tc("song.comment_count", song.comment_count, {
    count: "too many",
  })
}}
```

result is:

```
"too many comments"
```

### 6. Number formatting

[doc](https://vue-i18n.intlify.dev/guide/essentials/number.html)

#### 6.1 number translations

if style is `'currency'`, the `currency` field **must** be provided, eg: 'USD'.

```js
currency: {
    style: 'currency', currency: 'USD', notation: 'standard'
},
```

As the above, you can define named number formats (e.g. `currency`, etc), and you need to use the options with [ECMA-402 Intl.NumberFormat](https://tc39.es/ecma402/#numberformat-objects).

> Helpful:
> [mdn doc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat/NumberFormat)

Consume in vue template file:
To localize Number value with Vue I18n, use the `$n`.

```vue
<div class="song-price">{{ $n(1, "currency") }}</div>
```

UI result:

```
$1.00
```

### 7. component interpolation

[doc](https://vue-i18n.intlify.dev/guide/advanced/component.html#component-interpolation)

Sometimes, we need to localize with a locale message that was included in a HTML tag or component.
For example: (❌WRONG way)

```js
{
 accept:
    "I accept Music's <a>Terms of Service</a>",
}
```

Correct way:

```html
<i18n-t keypath="term" tag="label" for="tos">
  <a href="#" target="_blank">{{ $t('tos') }}</a>
</i18n-t>
```

translations in js file:

```js
en: {
    tos: 'Term of Service',
    term: 'I accept xxx {0}.'
},
```

### 8. Changing Locales

Tips:

- adding a `fallbackLocale` is better for user to avoid seeing a blank page

**change locale**([doc](https://vue-i18n.intlify.dev/guide/essentials/scope.html#locale-changing))

- `ml-auto`: left-margin, tailwind css [link](https://tailwindcss.com/docs/margin)
- `this.$i18n.locale`: get current locale in JS file

复习：

- `v-model`: 2-way binding - [section2.9-doc](https://jialihan.github.io/blog/#/VUE/section2?id=_29-two-way-binding)
