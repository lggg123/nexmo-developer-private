---
title: iOS
language: ios
---

# Overview

In this guide you learn how to add the Client SDK to your iOS app.

## Prerequisites

To use the Vonage SDK for iOS, you need to have the following installed:

* Xcode 10 or later
* iOS 10.2 or later

## Add the SDK to your iOS Project

Open Xcode with your iOS project.

You can either install the Client SDK directly, or via CocoaPods.

### CocoaPods

1. Open your project's `PodFile`. If you don't have one already, open a terminal and run the following commands:

    ```
    $ cd 'Project Dir'
    $ pod init
    ```

    Where `Project Dir` is the path to the parent directory of the `PodFile`.

2. Under your target add the `NexmoClient` pod. Replace `TargetName` with your actual target name.

    ```ruby
    target 'TargetName' do
        pod 'NexmoClient'
    end
    ```

    Make sure the pod file has the public CocoaPod specs repository source.

3. Install the Pod by opening a terminal and running the following command:

    ```
    $ cd 'Project Dir'
    $ pod update
    ```

    Where `Project Dir` is the path to the parent directory of the `PodFile`.

4. Open the `xcworkspace` with Xcode and disable `bitcode` for your target.

5. In your code, import the `NexmoClient` library:  

```tabbed_content
source: '_tutorials_tabbed_content/client-sdk/setup/add-sdk/ios/import'
```

### Frameworks

1. Download the Client SDK. You can find the latest version on the [Cocoapods repository](https://github.com/CocoaPods/Specs/tree/ccff7c3196fc5ee4c4846848af9dc4aa792f6d55/Specs/8/8/d/NexmoClient). Navigate to the latest version, then use the link in the `NexmoClient.podspec.json` file. Then drag and drop the `NexmoClient.framework` folder into your project:
![Project explorer](/images/client-sdk/ios-manual-setup-project.png)

2. Turn on *Embed & Sign* for `NexmoClient.framework` in your target's settings:
![Embed & sign](/images/client-sdk/ios-manual-setup-signing.png)

3. In your code, import the `NexmoClient` library:

```tabbed_content
source: '_tutorials_tabbed_content/client-sdk/setup/add-sdk/ios/import'
```

### Additional framework for SDK versions >= 4

4. Download the Vonage WebRTC dependency. You can find the latest version on the [Cocoapods repository](https://github.com/CocoaPods/Specs/tree/ccff7c3196fc5ee4c4846848af9dc4aa792f6d55/Specs/8/1/b/VonageWebRTC). Similarly to the Client SDK, use the link in the `VonageWebRTC.podspec.json` file. Then drag and drop the `VonageWebRTC.framework` folder into your project.

5. Turn on *Embed & Sign* for `VonageWebRTC.framework` and add `libc++.tbd` in your target's settings:
![Embed & sign VonageWebRTC](/images/client-sdk/ios-manual-setup-webrtc.png)

## Add permissions

To use the in-app voice features, you need to add audio permissions:

1. In your `Info.plist` add a new row with 'Privacy - Microphone Usage Description' and a description for using the microphone. For example, `Audio Calls`.

2. In your code add a request for Audio Permissions:  

```tabbed_content
source: '_tutorials_tabbed_content/client-sdk/setup/add-sdk/ios/permissions'
```

`AppDelegate` is the best place to do this.

## Using client in your app

### Login

Create a `NXMClient` object and login with a `jwt` user token. If necessary, you can read more about [generating the JWT](/client-sdk/concepts/jwt-acl).

```tabbed_content
source: '_tutorials_tabbed_content/client-sdk/setup/add-sdk/ios/login'
```

    Note that `self` should implement the `NXMClientDelegate` protocol.  


### Connection status

On a successful login, the following delegate method is called with `NXMConnectionStatusConnected`:

```tabbed_content
source: '_tutorials_tabbed_content/client-sdk/setup/add-sdk/ios/delegate'
```

You can check if the connection status is connected with `isConnected`:

```tabbed_content
source: '_tutorials_tabbed_content/client-sdk/setup/add-sdk/ios/is-connected'
```

`isConnected` will be the last connection status of the client if your app is suspended in the background. Therefore the value for `isConnected` can be stale. 

### Get current user info

After the login succeeds, the logged in user will be available via:

```tabbed_content
source: '_tutorials_tabbed_content/client-sdk/setup/add-sdk/ios/user'
```

## Conclusion

You added the Client SDK to your iOS app, and logged in to a `NXMClient` instance. You can now use the `NXMClient` client in your app, and use the Client SDK functionality.

## See also

* [Data Center Configuration](/client-sdk/setup/configure-data-center) - this is an advanced optional configuration you can carry out after adding the SDK to your application.
