---
title: Objective-C
language: objective_c
menu_weight: 2
---

```objective_c
[[NXMClient shared] serverCallWithCallee:userName customData:@{@"device_name": @"Alice app"} 
        completionHandler:^(NSError * _Nullable error, NXMCall * _Nullable call) {
    if(error) {
        // Handle create call failure
        ...
        return;
    }
    // Handle call created successfully. 
    ...
}];
```

