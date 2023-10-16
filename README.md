# Analytics SDK Integration

Our SDK combines all the tools for analytics, tracking and push marketing in one solution.

This is a simple guide on how to integrate our BAK SDK into your SwiftUI/UIKit based project.
If your project is Unity based you need a different [guide](README_UNITY.md).

## Dependency 
 Add SDK swift package dependencies to project: [https://github.com/balabantim/BAK_OPTIMIZED.git](https://github.com/balabantim/BAK_OPTIMIZED.git), and set proper branch **"blockOrganic"**.

![enter image description here](https://i.imgur.com/oYMmVs3.png)

## Info.plist setup
After dependency is continue processing, go to INFO tab, and setup some necessary fields:

> ITSAppUsesNonExemptEncryption  :  NO

> NSUserTrackingUsageDescription : Select "Allow" for better experience. This identifier will be used to collect Crash Data and in-app activity in order to improve functionalities and user engagement.

> NSCameraUsageDescription : Select "Allow" for better experience. The app сan use images to setup user profile and problem solving, we do not store your personal photos.

> NSPhotoLibraryUsageDescription :  The app сan use images to setup user profile and problem solving, we do not store your personal photos.

Also you need remove **Remove Scene Configuration** key from **Application Scene Manifest** if it present, and set value of **Enable Multiple Windows** key to **NO**.

![enter image description here](https://i.imgur.com/vGkorUY.png)

#### Next step is enable **Push Notification** on Capability editor:

![enter image description here](https://i.imgur.com/bg1UMSz.png)

## Setup Appereance
Our SDK is work correctly, when application has splash screen and  supports the correct orientation.
Please enable **iPhone Orientation** states on **Deployment Info** tab to
*Portrait, Landscape Left, Landscape Right values*, like on image bellow.

Also you need to create LaunchScreen.storyboard file with proper start screen placeholder or color and set it as LaunchScreen in project configuration:

![enter image description here](https://i.imgur.com/g1HDkvC.png)


### SDK Configuration
SDK is base on Google Firebase service, so you need **GoogleService-Info.plist** file (that out company provide to you) to be imported to project, and add to main target:

![enter image description here](https://i.imgur.com/pZTba6L.png)

## Code setup
Great, Almost everything is done, the next steps depend on the type of project you have, **SwiftUI** or **UIKit** based.

#### SwiftUI based application

Create and add to project **AppDelegate.swift** file, with this contents, where *ContentView()* is your main SwiftUI view:

Set appOrientation: .landscape or .portrait or .all depends of app support orientation.

```Swift

import UIKit
import BAKFramework

@main
class AppDelegate: UIResponder, UIApplicationDelegate  {
    
    var window: UIWindow?
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        BAKService.shared.setupUIAnalytics(appOrientation: .landscape, launchOptions: launchOptions, window: &window) {
            return ContentView()
        }
    
        return true
    }

   func application(_ application: UIApplication, supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask {
        return BAKService.orientationLock
    }
}

```

Delete SwiftUI old root file from project (*app.swift file standart swiftui project root file) that looks like this:

![enter image description here](https://i.imgur.com/101neSC.png)

*Target project is ready , Build test and publish project in original way.*

#### UIKit based application

Update **AppDelegate.swift** file, with this contents, do not forget use proper main storyboard name, or init root view controler manualy:

Set appOrientation: .landscape or .portrait or .all depends of app support orientation.

```Swift
import UIKit
import BAKFramework

@main
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?
    
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
        
        BAKService.shared.setupAnalytics(appOrientation: .landscape, launchOptions: launchOptions, window: &self.window, main: {
            let storyboard = UIStoryboard(name: "Main", bundle: nil)
            let initialViewController = storyboard.instantiateInitialViewController()
            self.window?.rootViewController = initialViewController
        })
        
        return true
    }

    func application(_ application: UIApplication, supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask {
        return BAKService.orientationLock
    }

}
```

Remove **Main storyboard file base name** from your project **Info.plist** file.

*Target project is ready , Build test and publish project in original way.*

