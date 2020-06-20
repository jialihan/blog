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

![image](../assets/scrollview_error.png ':size=595x343')


### Solution
We need to add constraints for each subview after we added them to our scroll view. Let's first look at the following picture, then we add our code step by step to configure the layout constriants for every subview.

![image](../assets/scrollview.png ':size=987x412')

##### code implementation:
```
// first
first.topAnchor.constraint(equalTo: self.scrollView.topAnchor, constant: 0).isActive = true
first.leadingAnchor.constraint(equalTo: self.scrollView.leadingAnchor, constant: 0).isActive = true
first.bottomAnchor.constraint(equalTo:self.scrollView.bottomAnchor, constant: 0).isActive = true

// second
second.topAnchor.constraint(equalTo: self.scrollView.topAnchor, constant: 0).isActive = true
second.leadingAnchor.constraint(equalTo: first.trailingAnchor, constant: 0).isActive = true
second.bottomAnchor.constraint(equalTo:self.scrollView.bottomAnchor, constant: 0).isActive = true

// last
last.topAnchor.constraint(equalTo: self.scrollView.topAnchor, constant: 0).isActive = true
last.leadingAnchor.constraint(equalTo: second.trailingAnchor, constant: 0).isActive = true
last.bottomAnchor.constraint(equalTo:self.scrollView.bottomAnchor, constant: 0).isActive = true
last.trailingAnchor.constraint(equalTo: self.scrollView.trailingAnchor, constant: 0).isActive = true
```
To optimize our connecting each slide contraints method, we can have a for loop to process multiple(when subview count > 3) in a cleaner code style.
* step1: create var pre = scrollView
* step2: for each subview(`pseudo code`):
    ```
    var cur = subview
    connect cur.leadingAnchor to pre.leadingAnchor
    connect cur.topAnchor to scrollView.topAnchor
    connect cur.bottomAnchor to scrollView.bottomAnchor
    if(index == last subview) {
        connect cur. trailingAnchor to scrollView.trailingAnchor
    }
    pre = cur
    ```
Then after adding the contraints, the final correct layout is the following picture:

![image](../assets/scrollview_correct.png ':size=595x343')

#### source code 
To learn more about the whole project source code, please refer the git repo here: [GitHub Link](https://github.com/jialihan/ScrollView-IOS)