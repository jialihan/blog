## How to use UserDefaults for data persistence 
#### Why use userdefaults
The user defaults are good to be used for light-weigth and simple pieces of data, not the replacement for real database. It's stored in computer memory, then it won't loose after we re-launch our ios app. Ususally what types of data that are stored in userdefaults:
* User information, eg: name, email address, age, occupation
* App settings and configurations, eg: ui theme, color, interface
* Flags(Bool variables)

Due to apples official document definition: ([UserDefaults Apple Link](https://developer.apple.com/documentation/foundation/userdefaults))
```
An interface to the user’s defaults database, where you store key-value pairs persistently across launches of your app.
```

#### What types of data can be stored
I summarize each types of data can be put into userdefaults due to latest official document:
* basic types: `Bool, Float, Double, Int, String, or URL`
* complex data types: `arrays, dictionaries, object, Data`
 
#### How to declare an instance of userDefault
* declare a local instance:
    ```
    class UserDefaults : NSObject
    let defaults = UserDefaults.standard
    ```
* Better and powerful usage: to create a globally singleton instance that whole app can share
    ```
    extension UserDefaults {
        static var shared: UserDefaults = UserDefaults.standard
    }
    ```
    Then we usually get that global userdefault instance from static class variable:
    ```
     let defaults: UserDefaults = .shared
    ```
#### How to set value into userDefaults

For most general use case, this setter is enough:
```
func set(Any?, forKey: String)
```
Pay attention, when I try to set URL, I met a bug here. If we simple use following code:
```
let expectedKey = URL(string: "https://www.test.com/")
userdefaults.set(expectedKey, forKey: yourKeyName)
```
you will get the following error:

![image](../assets/url_error.png ':size=600x271')

Try to use `NSCoder, NSKeyedArchiver` to encode the URL data, but feels since Apple has api support for this type of data object URL, let's solve it in a simple way, use this API as a wr([API link](https://developer.apple.com/documentation/foundation/userdefaults)):
```
func set(URL?, forKey: String)
Sets the value of the specified default key to the specified URL.
```

#### How to get data

Unfortunately we don't have a Generics to get data using a `func get<T>(){....}` function, apple provides separate getter for each data type. If you want to reduce the code, then parse and unwrap data by yourself(of course, this will increase your parse effort), at least, you need to define 3 types of data structure:
*  Any?
* [Any]?
* [String: Any]?

Then let's still look at each getter api in detail:
```
func object(forKey: String) -> Any?

func url(forKey: String) -> URL?

func array(forKey: String) -> [Any]?

func dictionary(forKey: String) -> [String : Any]?

func string(forKey: String) -> String?

func stringArray(forKey: String) -> [String]?

func data(forKey: String) -> Data?

func bool(forKey: String) -> Bool

func integer(forKey: String) -> Int

func float(forKey: String) -> Float

func double(forKey: String) -> Double

func dictionaryRepresentation() -> [String : Any]
```

#### How to remove a K-V pair

```
// Removes the value of the specified default key.
func removeObject(forKey: String)
```

#### How to encode & decode custom object
1. Using Codable
declare a custom struct extends Codable
```
struct Person : Codable {
    var name: String
    var age: Int
}
```

Then use `PropertyListEncoder()` to encode before add into userdefaults, and use ` PropertyListDecoder()` to decode when get from userdefaults.

```
// encode
userdefaults.set(try? PropertyListEncoder().encode(person), forKey: "PersonKey")

// decode
guard let personData = userdefaults.object(forKey: "PersonKey") as? Data else { return }
guard let person = try? PropertyListDecoder().decode(Person.self, from: personData) else { return }
```

2. Convert custom object into NSData is using `NSKeyedArchiver with NSCoding protocol`
   ```
   // TODO
   Maybe in the future will update more use case and examples 
   ```