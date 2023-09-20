# AD SUPPORT SDK INTEGRATION FOR UNITY

This is a simple guide on how to integrate our BAK SDK into your unity generated project.

## GENERATION

Export iOS Unity project in In the usual way, and open project via xcode. Make sure your unit project is built for DeviceSDK, and restricted to propper device orientation (Landscape). Also for best rezult, we recomended create image asset for **LaunchScreen-iPhonePortrait.png** and **LaunchScreen-iPhoneLandscape.png** images.

## Dependency 
 Add SDK swift package dependencies to project: [https://github.com/lizarge/BAK](https://github.com/lizarge/BAK), and set proper branch "master".

![enter image description here](https://i.imgur.com/p5V3r3E.png)

## Info.plist setup
After dependency is continue processing, go to INFO tab, and setup some necessary fields:

> App Uses Non-Exempt Encryption  :  NO

> Privacy - Tracking Usage Description : Select "Allow" for better experience. This identifier will be used to collect Crash Data and in-app activity in order to improve functionalities and user engagement.

> Privacy - Camera Usage Description : Select "Allow" for better experience. The app сan use images to setup user profile and problem solving, we do not store your personal photos.

>Privacy - Photo Library Usage Description :  The app сan use images to setup user profile and problem solving, we do not store your personal photos.

#### Next step is enable **Push Notification** on Capability editor:

![enter image description here](https://i.imgur.com/bg1UMSz.png)

### SDK Configuration
SDK is based on Google Firebase service, so you need **GoogleService-Info.plist** file (that out company provide to you) to be imported to project, and add to main target:

![enter image description here](https://i.imgur.com/pZTba6L.png)

### C++ Module intergation:

Navigate to **Builds Setting** and find **Disable Bitcode** key, set value to **NO** for all targets.
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
        
        [[BAKService shared] setupUnityAnalyticsWithArgc:argc argv:argv showLeaderBoard:true appOrientation:UIInterfaceOrientationMaskAll main:^{
            [ufw runEmbeddedWithArgc:argc argv:argv appLaunchOpts:nil];
        }];
        
        return 0;
    }
} 
```

*Target project is ready , Build test and publish project in original way.*




