## Deep Understanding Transform in CSS

### 2. Rotate in 3d
#### 2.1 understand the axis in 3 directions
![image](../assets/axis3d.png ':size=249x259')

**Explanation**:
* rotateX(): element move forward or go away from you
* rotateY(): element spins around y-axis
* rotateZ(): element spins around z-axis

**Degrees**: 
* positive deg: clock-wise
* negative deg: anti-clock-wise

#### 2.2 Visual Example of 3 directions
We rotate the box element at: **0 deg, 90 deg, 180 deg, -45 deg**.
* rotateX

![image](../assets/rotatex.png)

* rotateY

![image](../assets/rotatey.png)

* rotateZ

![image](../assets/rotatez.png)

### 3. understand the "perspective"

3.1 perspective function:
  each element where you apply it has its own perspective, for example:
  ```
  transform:perspective(1000px)  rotateX(30deg)  rotateY(80deg);
  ```

3.2 perspective property:
  perspective property actually has to **be applied to the parent element**, the advantage then is that **you get the same perspective on all child elements**.
  
3.3 value: smaller means closer to the element, larger means far from the element
For example: **perspective(100px)** vs. **perspective(1000px)**

![image](../assets/perspective.png ':size=400x210')

3.4 perspective-origin
it van be a `value or left, right, bottom, top` , represents the **angle** you look at it. Reference at: [CSS/perspective-origin](https://developer.mozilla.org/en-US/docs/Web/CSS/perspective-origin).
For example: when perspective = 100px, we look at different angles at center, top, right, bottom, left, 0px, 100px and 500px.

![image](../assets/perspectiveorigin.png ':size=642x328')


### 4. rotate container and "transform-style"
Reference: [CSS/transform-style](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-style)
* `flat`: default

 When container rotate, for example at 90deg, it disappear all including its children.
* `preserve-3d`
 children of the element should be positioned in the 3D-space, still can be seen.
 ### 5. backface-visibility
 Default is showing, if you want to hide it, you can set it to "hidden";
 Reference: [CSS/backface-visibility](https://developer.mozilla.org/en-US/docs/Web/CSS/backface-visibility)
*  `visible`
* `hidden`

### 6. Transition to animate
#### 6.1 transition:
**Syntax**:
transition: property_for_watch | duration_time | timing_performance_function | delay_time
For example:
```
transition: opacity 500ms  ease-out 1s;
transition: width 2s;
```
#### 6.2 Transition + Transformation
```
transition:  width 2s, height 2s, transform 2s;
```

#### 6.3 transition-timing-function property:

![image](../assets/timingfunction.png ':size=300x364')

It pecifies the speed curve of the transition effect.
* **ease (default)** : a slow start, then fast, then end slowly
* **ease-in** : slow start
* **ease-out** : slow end
* **linear**: same speed from start to end
* **ease-in-out** : a slow start and slow end
* **cubic-bezier(n,n,n,n)** : you can define your own values in a cubic-bezier function. More custom defined timing-functions, please refere here: [https://easings.net/](https://easings.net/)
For example:
	```
	// easeInCubic
	transition: transform 0.6s cubic-bezier(0.32, 0, 0.67, 0);
	```
	Also take advantage of chrome-dev-tools, to play around with the timing-function, you and adjust it in chrome-dev-tools there.
	
	![image](../assets/cubic.png ':size=390x289')

#### 6.4 transition and "display"
* we cannot watch on "display" transition
* if we change the **display from none** to something else **or the other way around**, then your transition isn't going to kick off.
* Solution: use some hack, set to avoid display none:
	- original state: `{ display: none; }`
	- some-time:  set to `{ display: block; }` 
	- watch your tranistion on certain properties
	- some-time after the end: set back to `{ display:none; }` 
	- Then your original next steps' code
### 7. Animation in CSS
#### 7.1 define your @keyframes
define your animation in “@keyframes”, it will apply on every element that use this keyframes as animation. You can use **from** and **to** states, or use **percentage state** from 0% to 100%.
```
@keyframes  animationName  {
	from {
		// start state
	}
	to {
		// end state
	}
}
// or 
@keyframes  animationName  {
	0% {
	}
	50% {
	}
	100% {
	}
}
```

### 7.2 animation properties
Reference: [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations)

Syntax:
**animation**: keyframes | time_duration | delay times| iterates count | normal direction or reverse | fill state;

* **animation-direction**:  whether an animation should play forward, backward, or alternate back and forth between playing the sequence forward and backward.
	Reference doc: [CSS/animation-direction](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-direction)
	 - normal
	 - reverse
	 - alternate
	 - alternate-reverse
* **animation-fill-mode**:
	- forwards: keep the final results
	- backwards: keep the original start state

### 8. Helpful links:

-   CSS Transforms: [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transforms/Using_CSS_transforms](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transforms/Using_CSS_transforms)
-   3D Transforms: [https://desandro.github.io/3dtransforms/](https://desandro.github.io/3dtransforms/)

