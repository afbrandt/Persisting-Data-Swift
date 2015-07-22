Storing data across multiple uses is critical for a winning app or game.  Who would play a game that forgets your high score or use an app doesn't remember who you are?  Fortunately Apple provides a simple way of storing commonly used data.  It does this through the *NSUserDefaults* class.  Reading the [documentation](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/occ/clm/NSUserDefaults/standardUserDefaults) does not make this very obvious, so we'll provide an example so you can get started right away.

# Setting up NSUserDefaults

Apple provides a simple interface for accessing defaults.  The class provides a singleton, *standardUserDefaults()*, to get an object to read from and write to.  The object you get back can be treated as a dictionary, since all objects stored and retrieved are done on a key-value basis.  The only difference is that subscripting (as in *dictionary["key"]*) is not available, so calling the correct method is necessary to store or retrieve data.

There are read and write methods for all [property list types](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/PropertyLists/AboutPropertyLists/AboutPropertyLists.html#//apple_ref/doc/uid/10000048i-CH3-54303).  What that means is right away you can store strings, integers, floating point numbers, dates, booleans, binary data, and both arrays and dictionaries that contain the previous types.

# Example

Now we will see a class that uses *NSUserDefaults* to store data, as well as a few other handy features that the Swift language has to offer.

	class UserState {
		var name: String = NSUserDefaults.standardUserDefaults().stringForKey("myName") ?? "User" {
			didSet {
				NSUserDefaults.standardUserDefaults().setObject(name, forKey:"myName")
				NSUserDefaults.standardUserDefaults().synchronize()
			}
		}
		
		var highScore: Int = NSUserDefaults.standardUserDefaults().stringForKey("myHighScore") ?? 0 {
			didSet {
				NSUserDefaults.standardUserDefaults().setInteger(highScore, forKey:"myHighScore")
				NSUserDefaults.standardUserDefaults().synchronize()
			}
		}
	}

One new function you may notice is *synchronize()* called after both of the properties are set.  This is a **critical** step that has to happen.  **Without it, the changes you make to the standard defaults will not persist**.  If you are new to Swift, there may be a couple strange looking operations associated with the properties.  

The first is the *??* operator.  This is called a [null coalescing operator](https://en.wikipedia.org/wiki/Null_coalescing_operator).  Since the retrieval methods return an optional wrapped type, we need a way to handle the potential nil value.  The null coalescing operator checks the left value for null, and if true, substitutes the null for the right value.  This will prevent a nil value from being assigned to the property.  

The second is the *didSet*.  It is a [property observer](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html#//apple_ref/doc/uid/TP40014097-CH14-ID262).  The block of code, also called a [closure](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html), contained after the *didSet* is executed after the property is updated.  This will automatically save the change to the defaults when it is changed.

# Summary

The *NSUserDefaults* class provides the mechanism to persist simple data between application uses.  The singleton *standardUserDefaults()* provides the interface to read and write data.  When writing data, it is **vital** to call *synchronize()* after performing an update to the defaults, or it will not persist.  Now you have the tools to save data in all your future iOS projects.  Happy coding!