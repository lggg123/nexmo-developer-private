---
title: Swift
language: swift
menu_weight: 1
---


```swift
NXMClient.shared.serverCall(withCallee: userName, customData: nil) { [weak self] (error, call) in
    guard let call = call else {
        // Handle create call failure
        ...
        return
    }
    // Handle call created successfully. 
    ...
})
```