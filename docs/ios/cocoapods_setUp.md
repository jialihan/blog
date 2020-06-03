##  Setup Cocoapods

#### Step 1:  install ruby gem
``` 
// in Terminal
brew install ruby
``` 
#### Step 1.1:  update gem
```
sudo gem update --system
```
you might got the error like this:
&nbsp;&nbsp;&nbsp;&nbsp;ERROR:  While executing gem ... (Gem::FilePermissionError)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;You don't have write permissions into the /usr/bin directory.

then  try to  use  `-n`  parameter to install like for cocoapods:
```
sudo gem install cocoapods -n /usr/local/bin
```
#### Step2: pod setUp
```
pod setup
pod install
```
issue: you might have the error seeing that it's always cloning from the repo, and it stucks here, always hanging here.
 ```
Cloning spec repo `master` from `https://github.com/CocoaPods/Specs.git` (branch `master`)
  $ /usr/bin/git clone 'https://github.com/CocoaPods/Specs.git' master
  Cloning into 'master'...
```
Solution:
try to clone the cocoapods repo from ourselves by command lines:
```
pod setup
CTRL + c
cd ~/.cocoapods/repos 
git clone --depth 1 https://github.com/CocoaPods/Specs.git master
```


