---
title: Kotlin
language: kotlin    
---

```kotlin
val callListener = object: NexmoRequestListener<NexmoCall> {
    override fun onSuccess(nexmoCall: NexmoCall?) {
        Log.d("TAG", "Call started: ${nexmoCall.toString()}")
    }

    override fun onError(apiError: NexmoApiError) {
        Log.d("TAG", "Error: Unable to start a call ${apiError.message}")
    }
}

client.inAppCall("123456", callListener);
```
