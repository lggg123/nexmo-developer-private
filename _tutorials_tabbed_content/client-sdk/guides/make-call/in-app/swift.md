---
title: Swift
language: swift
menu_weight: 4
---

```swift
NXMClient.shared.inAppCall(withCallee: userName) { [weak self] (error, call) in
    guard let call = call else {
        // Handle create call failure
        ...
        return
    }
    // Handle call created successfully. 
    ...
})
```
