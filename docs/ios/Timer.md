## How to Build a Countdown Timer - Swift

#### Use Timer Object
Define a timer variable and define the count down time, let's use 60s for example.
```
var timer: Timer?
var countDownTime:Int = 60
```

#### Create a repeating Timer
In order to be safe, everytime before we start a timer, we need to clear/reset this variable, using the following method `invalidate()`:
```
timer?.invalidate()
```
Then we create and start the Timer by the `scheduleTimer()` method:
```
 timer = Timer.scheduledTimer(withTimeInterval: 1.0, repeats: true) { timer in
    // Todo action in each step
    countDownTimer()
}
```
Then we need to define our countDownTimer method in each step(1 second duration):
```
func countDownTimer(){
    countDownTime--;
    if(countDownTime == 0)
    {
        // stop the timer
        timer?.invalidate()
    }
}
```

#### Stop the timer
As mentioned above several times, we can have the following method to pause or stop the timer:
```
func stopTimerFnc() {
    timer?.invalidate()
    // TODO 
    return
}
```

#### using a non-repeating timer
If you want code to run only once, change the `repeats` parameter to `false`, and still do the same thing to create/start the timer:
```
 timer = Timer.scheduledTimer(withTimeInterval: 1.0, repeats: false) { timer in
    // Todo action in each step
    print("timer 1s duration")
}
```

#### A complicated Timer and Source code 
* Build a ui controlled timer, you can select time to countdown by the `UIPickerView`, and you can have some button actions to `start, pause, and stop` the timer.

* To learn more about the whole project source code, please refer the git repo here: [GitHub Link](https://github.com/jialihan/IOS-Timer)

#### Official Docs
https://developer.apple.com/documentation/foundation/timer