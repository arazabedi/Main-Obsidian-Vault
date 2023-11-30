**Inline Styles in React**
___
Here is an example. The syntax is slightly different from css:

```js
const Footer = () => {
  const footerStyle = {
    color: 'green',
    fontStyle: 'italic',
    fontSize: 16
  }
  return (
    <div style={footerStyle}>
      <br />
      <em>Note app, Department of Computer Science, University of Helsinki 2022</em>
    </div>
  )
}
```

>[!info]
>Psuedo-classes can't be used straightforwardly with inline styles in React


