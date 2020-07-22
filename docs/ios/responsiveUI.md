## Responsive UI - Swift


### 1.  Size Classes and Orientation.

These separate size classes represent a particular group of devices.

For example: the iPhone 8, 7, and 6 all have the same size screen and aspect ratio. So they are grouped into the same iPhone 8 size class. 

Whereas the iPhone 6, 7, 8 Plus all have the same size and orientation, so they have a different size class.
![image](../assets/devices.png ':size=278x575')


### 2. Background fills entire screen

We have the problem when rotates our iphone, and the background image will exceeds to outside of screen. Like the following picture:
![image](../assets/background.png ':size=430x478')

Solution is to add rules and constraints on 4 anchors: `top, bottom, leading, trailing anchors` on 4 edges. 
![image](../assets/addConstraints.png ':size=341x396')

Attention here, we need to ensure the `leading and trailing anchor` attached to `superView`  instead of the `safe area`, because when we rotate to horizontal screen, the `safe area on leading and trailing` part will be space and white.

![image](../assets/superView.png ':size=646x247')

### 3. Two techniques: Pin and Align
Problem: when we add constraints to pin element to our edges, it might overflow sometimes after the rotation to horizontal direction. Then this constraint will break. For example the following picture.

![image](../assets/pinOverflow.png ':size=332x281')

Solution: Combination of alignment to center and with proper constraints if needed. Because the `centerX and centerY` will not change even after rotation.
Then you can decide that whether you still want to add certain constant to pin the element.

![image](../assets/alignAndPin.png ':size=282x277')

### 4. vertical spacing between elements
If we want the label always to be 30px under our logo, how to do that?
1) add centerX constraints to the label itself.
2) add vertical spacing between elements, which will allow element pin to other elements.

Another option is `horizontal spacing`, it's the same idea.


### 5. create containers 
Goal:  creating some `containers` to embed our elements in, so that we `don't always have to align or pin it to the super view`.

Solution:
We could use some containers to split up the screen into three equal parts, and then we can embed each of these elements inside its own container. 

![image](../assets/container00.png ':size=238x482')

Use `UIView` to create `an section` on our superview.

Example:

![image](../assets/container.png ':size=422x262')

### 6. use stack view

1 ) embed all subviews( containers ) into `stack view`.

![image](../assets/embedStackView.png ':size=405x550')

2 ) pin the `stack view` to our `super view`. this stack view should not go into safe area. Then we  pin stack view to safe area as constraints.

![image](../assets/pintoSafearea.png ':size=323x196')

3 ) nested stack view if needed
how to place two or more elements lie inside of our container? we can embed them into `inner-stackView` as nested inside our container. 

![image](../assets/nestedStackView.png ':size=422x260')

### 7. be careful about width constraint
Problem: if we set a width constraint like 100px on our label, it might shrink when we have longer content on the label. For example:
![image](../assets/widthShrink.png ':size=147x75')

Solution: change the constraint to when width larger than or equal to constant(`>=`). Then we got a better result to hold the full length of content.

![image](../assets/widthconstraint.png ':size=613x299')

Correct result:

![image](../assets/correctWidth.png ':size=152x43')

### 8. stack view fill-proportionally
If we want to make split the stack view in certain proportional numbers, like: item1 is 2 units, item2 is 1 unit, and item3 is 1 unit, we can have the following constraints.

1 ) add width or height constraints to make unit sizes.

![image](../assets/stackcells.png ':size=354x179')

add constraints using `multiplier`:
```
// here two multiplier value is 2 and 1
item1.width = 2 * item2.width
item3.width = 1 * item2.width
```
2 ) setting in stackview `Distribution` property to `fill proportionally`

 ![image](../assets/proportionally.png ':size=411x256')


### 9. source code reference
* Description: build the ios-calculator using stackview
* responsive ui using these rules mentioned in this article
* functionality is not available now, will add up in the future.

[Github Link: IOS-Calculator](https://github.com/jialihan/IOS-Calculator)






