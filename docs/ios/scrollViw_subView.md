## Add constraints to subViews in ScrollView - Swift
### Problem
We try to add multiple subviews with similar layout and use the same customized UIView class as subviews, so we want to keep the auto layout for each uiview as subview in scrollView. Let's declare our subviews first.
```
// already created customized UIView class with layout
let view1 = MyUIView()  
let view2 = MyUIView()  
let view3 = MyUIView()  
}
```
Then we can configure our scrollView, then add all those subviews into our scrollView. Also
set the contraints for our scrollView to bound to the main screen.
```
override func viewDidLoad() {

    view.addSubview(scrollView)
    scrollView.addSubview(view1)
    scrollView.addSubview(view2)
    scrollView.addSubview(view3)
```
If we only did this, when we run our project, we will see that each subview is messede up losing it's original layout in our customized UIViw class. Each subview's original layout is ineffective and lose it's power to show in our scroll view, like the following picture.


### Solution
We need to add constraints for each subview after we added them to our scroll view. Let's first look at the following picture, then we add our code step by step to configure the layout constriants for every subview.

![image](../assets/scrollview.png ':size=987x412')