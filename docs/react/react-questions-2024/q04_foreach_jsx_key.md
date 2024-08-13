Don't forget the "key={}" attribute when you loop a list of elements.

```jsx
{
  props.elementConfig.options.map((el) => (
    <option key={el.value} value={el.value}>
      {el.displayValue}
    </option>
  ));
}
```
