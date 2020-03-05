# IOSSecuritySuite

This repository contains Xamarin.iOS bindings for the [IOSSecuritySuite](https://github.com/securing/IOSSecuritySuite) swift library.

[![Build status](https://dev.azure.com/nicolasmilcoff/IOSSecuritySuite/_apis/build/status/nmilcoff.IOSSecuritySuite)](https://dev.azure.com/nicolasmilcoff/IOSSecuritySuite/_build/latest?definitionId=2)
[![NuGet](https://img.shields.io/nuget/v/IOSSecuritySuite.svg?label=NuGet)](https://www.nuget.org/packages/IOSSecuritySuite)

-----

![ISS logo](https://raw.githubusercontent.com/securing/IOSSecuritySuite/master/logo.png)

by [@_r3ggi](https://twitter.com/_r3ggi)

## ISS Description
🌏 iOS Security Suite is an advanced and easy-to-use platform security & anti-tampering library written in pure Swift! If you are developing for iOS and you want to protect your app according to the OWASP [MASVS](https://github.com/OWASP/owasp-masvs) standard, chapter v8, then this library could save you a lot of time. 🚀

What ISS detects:

* Jailbreak (even the iOS 11+ with brand new indicators! 🔥)
* Attached debugger 👨🏻‍🚀
* If an app was run in emulator 👽
* Common reverse engineering tools running on the device 🔭

## Setup

Download the package from [NuGet](https://www.nuget.org/packages/IOSSecuritySuite).

> Install-Package IOSSecuritySuite

### Update Info.plist
After adding ISS to your project, you will also need to update your main Info.plist. There is a check in jailbreak detection module that uses ```CanOpenURL()``` method and [requires](https://developer.apple.com/documentation/uikit/uiapplication/1622952-canopenurl) specyfing URLs that will be queried.

```xml
<key>LSApplicationQueriesSchemes</key>
<array>
	<string>cydia</string>
	<string>undecimus</string>
	<string>sileo</string>
	<string>zbra</string>
</array>
```  

## How to use

### Jailbreak detector module

* **The simplest method** returns True/False if you just want to know if the device is jailbroken or jailed

```c#
if(Securing.IOSSecuritySuite.AmIJailbroken()) 
{
	// This device is jailbroken
}
else 
{
	// This device is not jailbroken
}
```

### Debbuger detector module
```c#
var amIDebugged = Securing.IOSSecuritySuite.AmIDebugged() ? true : false;
```

### Deny debugger at all
```c#
Securing.IOSSecuritySuite.denyDebugger();
```

### Emulator detector module
```c#
var runInEmulator = Securing.IOSSecuritySuite.AmIRunInEmulator() ? true : false;
```

### Reverse engineering tools detector module
```c#
var amIReverseEngineered = Securing.IOSSecuritySuite.AmIReverseEngineered() ? true : false;
```

## Security considerations
Before using this and other platform security checkers you have to understand that:

* Including this tool in your project is not the only thing you should do in order to improve your app security! You can read a general mobile security whitepaper [here](https://www.securing.biz/en/mobile-application-security-best-practices/index.html).
* Detecting if a device is jailbroken is done locally on the device. It means that every jailbreak detector may be bypassed (even this)! 
* Swift code is considered to be harder to manipulate dynamically than Objective-C. Since this library was written in pure Swift, the IOSSecuritySuite methods shouldn't be exposed to Objective-C runtime (which makes it more difficult to bypass ✅). You have to know that attacker is still able to MSHookFunction/MSFindSymbol Swift symbols and dynamically change Swift code execution flow.
* It's also a good idea to obfuscate the whole project code including this library. See [Swiftshield](https://github.com/rockbruno/swiftshield)

## Contribution ❤️
Yes, please! 

### Special thanks: 👏🏻

* [r3ggi](https://github.com/r3ggi) for creating the swift library
* [kubajakowski](https://github.com/kubajakowski) for pointing out the problem with ```canOpenURL(_:)``` method
* [olbartek](https://github.com/olbartek) for code review and pull request 
* [benbahrenburg](https://github.com/benbahrenburg) for various ISS improvements
* [fotiDim](https://github.com/fotiDim) for adding new file paths to check
* [gcharita](https://github.com/gcharita) for adding the Swift Package Manager support
* [rynaardb](https://github.com/rynaardb) for creating the `amIJailbrokenWithFailedChecks()` method
* [undeaDD](https://github.com/undeaDD) for various ISS improvements

## License

This binding library is licensed under [MIT](https://github.com/nmilcoff/IOSSecuritySuite/blob/master/LICENSE).

## References
While creating this tool I used:

* 🔗 https://github.com/TheSwiftyCoder/JailBreak-Detection
* 🔗 https://github.com/abhinashjain/jailbreakdetection 
* 🔗 https://gist.github.com/ddrccw/8412847
* 🔗 https://gist.github.com/bugaevc/4307eaf045e4b4264d8e395b5878a63b
* 📚 "iOS Application Security" by David Thiel
