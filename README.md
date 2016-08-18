![Cover](https://raw.githubusercontent.com/wangjwchn/BenchmarkImage/master/Cover.png)
[![Language](https://img.shields.io/badge/swift-2.2-orange.svg)](http://swift.org)
[![Build Status](https://travis-ci.org/wangjwchn/JWAnimatedImage.svg?branch=master)](https://travis-ci.org/wangjwchn/JWAnimatedImage)
[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/JWAnimatedImage.svg)](https://img.shields.io/cocoapods/v/JWAnimatedImage.svg)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![Pod License](https://img.shields.io/dub/l/vibe-d.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

An animated GIF&APNG engine for iOS in Swift with low memory & cpu usage.

![video](http://i.imgur.com/XOoq9mP.gif)

##Features
- [X] Add APNG support.[NEW]
- [x] Using asynchronous image decoding to reduce the main thread CPU usage.
- [x] Optimized for Multi-Image case.
- [x] As UIImage and UIImageView extension,easy to use.
- [x] Have a great performance on memory usage by using producer/consumer pattern.
- [x] Have a great performance on CPU usage by using asynchronous loading.
- [x] Allow to control display quality by using factor 'level of Integrity'
- [x] Allow to control memory usage by using factor 'memoryLimit'

## Installation

#### CocoaPods
You can use [CocoaPods](http://cocoapods.org/) to install `JWAnimatedImage` by adding it to your `Podfile`:

```ruby
platform :ios, '8.0'
use_frameworks!
pod 'JWAnimatedImage'
```

To get the full benefits import `JWAnimatedImage` wherever you import UIKit

``` swift
import UIKit
import JWAnimatedImage
```

#### Carthage
Create a `Cartfile` that lists the framework and run `carthage bootstrap`. Follow the [instructions](https://github.com/Carthage/Carthage#if-youre-building-for-ios) to add `$(SRCROOT)/Carthage/Build/iOS/JWAnimatedImage.framework` to an iOS project.

```
github "wangjwchn/JWAnimatedImage"
```
#### Manually
1. Download and drop ```/JWAnimatedImage```folder in your project.  
2. Congratulations!  

##How to Use
```swift
let manager = JWAnimationManager(memoryLimit:20)
        
let url = NSBundle.mainBundle().URLForResource(“imagename”, withExtension: "gif")!
let imageData = NSData(contentsOfURL:url)
        
let image = UIImage(animatedImage:imageData!)
let imageview = UIImageView(animatedImage: image, manager:manager,loopTime: -1)
imageview.frame = CGRect(x: 7.0, y: 50.0, width: 400.0, height: 224.0)
view.addSubview(imageview)

```

##Benchmark
###Display GIF:Compared with [FLAnimatedImage](https://github.com/Flipboard/FLAnimatedImage)
#####1.Display 1 Image
|               |CPU Usage[average] |Memory Usage[average]/MB |
|:-------------:|:-----------------:|:-----------------------:|
|JWAnimatedImage|6% ~ 14% [8%]      |7.5 ~ 8.4 [8.2]          |
|FLAnimatedImage|8% ~ 24% [11%]     |7.3 ~ ??? [???]          |

#####2.Display 3 Images
|               |CPU Usage[average] |Memory Usage[average]/MB |
|:-------------:|:-----------------:|:-----------------------:|
|JWAnimatedImage|31% ~ 44% [38%]    |12.4 ~ 13.4 [12.9]       |
|FLAnimatedImage|36% ~ 62% [54%]    |11.0 ~ 12.4 [11.3]       |

#####3.Display 30 Images
|               |CPU Usage[average] |Memory Usage[average]/MB |
|:-------------:|:-----------------:|:-----------------------:|
|JWAnimatedImage|38% ~ 81% [53%]    |59.3 ~ 82.4 [63.3]       |
|FLAnimatedImage|126% ~ 185% [143%] |58.4 ~ 98.9 [74.2]       |


###Display APNG:Compared with [APNGKit](https://github.com/onevcat/APNGKit)

#####1.Display 1 Image
|               				|CPU Usage[average] |Memory Usage[average]/MB |
|:------------------------:|:-----------------:|:-----------------------:|
|JWAnimatedImage (Cache)	|2% ~ 44% [3%]      |43.2 ~ 43.2 [43.2]       |
|JWAnimatedImage (noCache)	|20% ~ 49% [33%]    |6.6 ~ 8.3 [7.5]          |
|APNGKit (Cache)				|1% ~ 42% [1%]      |95.6 ~ 95.6 [95.6]        |
|APNGKit (noCache)			|1% ~ 26% [1%]      |95.9 ~ 95.9 [95.9]        |


Measurement Factors:

 - Last updated: April 26, 2016

 - Measurement device: iPhone6 with iOS 9.3

 - Measurement tool: Profile in Xcode 7.3

 - Measurement image: See it in repository, all the parameters are default.

 - Raw data are [here](https://github.com/wangjwchn/BenchmarkImage).

 



##Architecture
![Architecture](https://raw.githubusercontent.com/wangjwchn/BenchmarkImage/master/Architecture.png)

####UIImageView State:

![LifeCycle](https://raw.githubusercontent.com/wangjwchn/BenchmarkImage/master/LifeCycle.png)

##### JWAnimationManager:
 Inital class 'JWAnimationManager' with memory limit.
JWAnimationManager will manage all the GIF image views in it. 

##### CheckForCache:
 When adding a new GIF image view to 'JWAnimationManager',it estimate memory usage of new GIF,and add to 'totalGifSize' as a new valuation.When new valuation is greater than memory limit, JWAnimationManager changes all GIF image views to no-cache mode.

#####ImageView's life cycle:
 ImageView will be suspended if function 'isDisplayedInScreen' returns false.
ImageView will be deleted from 'JWAnimationManager' if function 'isDiscarded' returns false.



##Licence
JWAnimatedImage is released under the MIT license. See [LICENSE](https://github.com/wangjwchn/JWAnimatedImage/raw/master/LICENSE) for details.
