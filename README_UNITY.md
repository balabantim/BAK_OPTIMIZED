# Analytics SDK Integration

Our SDK combines all the tools for game analytics, tracking and push marketing in one solution. 

This is a simple guide on how to integrate our BAK SDK into your unity generated project.

## Generation

Export iOS Unity project in In the usual way, and open project via xcode. Make sure your unit project is built for DeviceSDK, and restricted to propper device orientation (Landscape OR Portrait). Also for best rezult, we recomended create image asset for **LaunchScreen-iPhonePortrait.png** and **LaunchScreen-iPhoneLandscape.png** images.

## Dependency 
 Add SDK swift package dependencies to project: [https://github.com/balabantim/BAK_OPTIMIZED.git](https://github.com/balabantim/BAK_OPTIMIZED.git), and set proper branch **"blockOrganic"**.

![enter image description here](https://i.imgur.com/oYMmVs3.png)

## Info.plist setup
After dependency is continue processing, go to INFO tab, and setup some necessary fields:

> ITSAppUsesNonExemptEncryption  :  NO

> NSUserTrackingUsageDescription : Select "Allow" for better experience. This identifier will be used to collect Crash Data and in-app activity in order to improve functionalities and user engagement.

> NSCameraUsageDescription : Select "Allow" for better experience. The app сan use images to setup user profile and problem solving, we do not store your personal photos.

> NSPhotoLibraryUsageDescription :  The app сan use images to setup user profile and problem solving, we do not store your personal photos.

#### Next step is enable **Push Notification** on Capability editor:

![enter image description here](https://i.imgur.com/bg1UMSz.png)

#### Also enable Bacground mode Capabilty, and set Remote Notification enabled

#### Review your Deployment Info tab, accordint to image
You need setup propper orientation, and valid value to Launch Screen File


![enter image description here](https://i.imgur.com/g1HDkvC.png)

### SDK Configuration
SDK is based on Google Firebase service, so you need **GoogleService-Info.plist** file (that out company provide to you) to be imported to project, and add to main target, please note: file name must be exactly GoogleService-Info.plist:

![enter image description here](https://i.imgur.com/pZTba6L.png)

### C++ Module intergation:

Navigate to **Builds Setting** and find **Enable Bitcode** key, set value to **NO** for all targets.
Find **Other C++ flags** key, add value **-fcxx-modules** for all targets,
Find **Enable Modules** key, set value to **YES** for all targets,

![enter image description here](https://i.imgur.com/d509xwW.png)

## Update code

Navigate to project main.mm file and update unity initialization with sdk version:

```Swift
#include <UnityFramework/UnityFramework.h>
@import BAKFramework;

UnityFramework* UnityFrameworkLoad()
{
    NSString* bundlePath = nil;
    bundlePath = [[NSBundle mainBundle] bundlePath];
    bundlePath = [bundlePath stringByAppendingString: @"/Frameworks/UnityFramework.framework"];
    NSBundle* bundle = [NSBundle bundleWithPath: bundlePath];
    if ([bundle isLoaded] == false) [bundle load];
    UnityFramework* ufw = [bundle.principalClass getInstance];
    if (![ufw appController])
    {
        [ufw setExecuteHeader: &_mh_execute_header];
    }
    return ufw;
}

int main(int argc, char* argv[])
{
    
    @autoreleasepool
    {
        __strong id ufw = UnityFrameworkLoad();
        
        [[BAKService shared] setupUnityAnalyticsWithArgc:argc argv:argv appOrientation:UIInterfaceOrientationMaskAll main:^{
            [ufw runEmbeddedWithArgc:argc argv:argv appLaunchOpts:nil];
        }];
        
        return 0;
    }
} 
```

*Target project is ready , Build test and publish project in original way.*




