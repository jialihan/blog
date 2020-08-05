
## React Lifecycle Methods

### 1. lifecycle method has changed  after version 16.4+
deprecated method before version 16.4+:
*  **componentWillMount**  
    All the legacy use cases are covered in the constructor. This is renamed as  `UNSAFE_componentWillMount`.
*  **componentWillReceiveProps**  
    The new static method  `getDerivedStateFromProps`  is safe rewrite for this method and covers all the use cases of  `componentWillReceiveProps`. The new name for this method is  `UNSAFE_componentWillReceiveProps`.
*  **componentWillUpdate**  
    The new method  `getSnapshotBeforeUpdate`  is safe rewrite for this method and covers all the use cases of  `componentWillUpdate`.   The new name for this method is  `UNSAFE_componentWillUpdate`.
#### 2. Phase: Mounting(initial render)
* **constructor()**
	- default feature from ES6
* **static getDerivedStateFromProps()**
  - avoid any side-effects mistakes during the render phase
  - avoid `async code` executing in this phase
  - avoid to use `this` keyword, cannot use this.setState()...
  - this method is rarely used
* **render()**
  - **mandatory**
  - no **setState()** here, it will cause **infinite loop**
  - return your JSX
* **componentDidMount()**
  - after component is mounted
  - use third-party library
  - don't sync setState() here
  - can call setState() after it finished in promise

#### 3. Phase: Update
* **static getDerivedStateFromProps()**
* **shouldComponentUpdate(nextProps, nextState)**
   - may cancel updating process
   - only executed in Update( re-render) cycle
   - return true / false
 * **render()**
  * **getSnapshotBeforeUpdate()**
     - before it is actually updated from virtual DOM to actual DOM. 			    
     - This phase is known as `pre-commit` phase.
     - **Usage:** This method is useful if you want to keep sync in-between state of current DOM with the updated DOM. E.g. scroll position, audio/video, text-selection, cursor position, tool-tip position, etc.
	     ```
	     getSnapshotBeforeUpdate (prevProps, prevState)  
	     {  
		     if( ... )
		     {  
			     const myRef = this.myRef.current;
			     return myRef.scrollHeight - myRef.scrollTop; 
		     }  
		      return  null  
		 }
	     ```
* **componentDidUpdate()**
  - what you can do in componentDidMount, then you still can do it here.
  - third-party library
  - no sync to call setState()
  - can use our snapshot value
	  ```
	 componentDidUpdate (prevProps, prevState, snapshot)  			
	 {
		 if(snapshot !== null)
		 {
			 // todo
		 } 
	 }
	  ```

### 4. Unmounting
* **componentWillUnmount** 
This method is the last method in the lifecycle. This is executed just before the component gets removed from the DOM.

### 5. Summary

![image](../assets/lifecycle.png ':size=780x430')