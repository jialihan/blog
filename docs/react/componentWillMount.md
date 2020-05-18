### Use Case
I need to do something before the render(), that is something happens in "componentWillMount" stage.
### Class-based Component
This will be good. Because this.setState() will not trigger re-render during this stage.
```
componentWillMount() {
    this.setState({count:10}, () => {
      console.log('willMount...cannot be executed!')
    });
     console.log(' willMount canbe executed here..');
  }
```