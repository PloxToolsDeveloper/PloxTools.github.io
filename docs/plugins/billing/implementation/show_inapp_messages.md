# Show In-App Messages

This is an **asynchronous function** that launches a **Google Play Billing window** (handled by Google) to display any pending in-app messages that require user attention — such as price change confirmations or subscription notices.

Typically, this function should be called **at the start of the game**, immediately after successfully initializing the Billing Client.  
If the user’s account has any queued billing-related messages, they will be shown automatically.  
If there are no messages to display, the call completes silently without triggering any delegates.

Once the asynchronous operation completes, one of the two delegates will be invoked — either **OnSuccess** or **OnFailure** — unless no messages were found, in which case neither will be called.

<div align="center">
  <img src="../assets/show_inapp_messages/show_inapp_messages_node.png" width="250">
</div>

---

## Parameters  

### 1. On Success  
Triggered when a pending in-app message (such as a price change confirmation) has been displayed and the player has interacted with it.  
You can drag this pin to create a custom event and perform any follow-up logic, such as refreshing the UI or validating active subscriptions.

---

### 2. On Failure  
Triggered if an error occurs while attempting to display or check for in-app messages.  
Returns:  
- **Response Code** — [`EBillingResponseCode`](../object_definitions/enumerations.md#ebillingresponsecode)  
- **Error Message** — `FString` with debug details.  

If no messages are available for the current user, **neither delegate will be called**.

---

## Example  

In the example below, we trigger the **Show In-App Messages** function right after initializing the Billing Client to check whether there are any pending billing-related messages for the user.  
If Google Play detects any notifications (such as a subscription price change confirmation), it will automatically display them to the player.  

You can create your own custom function to react when the message is acknowledged — for example, showing a confirmation popup or refreshing subscription data.  
If no messages are available or shown, the function will complete silently without firing any delegates.

The most common use case is prompting the player to confirm a **subscription price change** or other mandatory billing update.

<div align="center">
  <img src="../assets/show_inapp_messages/show_inapp_messages_example.png" width="600">
</div>

---

## C++ Usage

Here’s how you can call this function in your C++ code:

```cpp
#include "BillingCPPLibrary.h"

void UMyGameInstance::ShowInAppMessages()
{
    FOnShowInAppMessagesSuccessfulNative OnSuccess;
    OnSuccess.BindLambda([]()
    {
    UE_LOG(LogTemp, Log, TEXT("In-app message displayed successfully."));
    });

    FOnShowInAppMessagesFailedNative OnFailure;
    OnFailure.BindLambda([](EBillingResponseCode Code, const FString& Message)
    {
    UE_LOG(LogTemp, Error, TEXT("Failed to show in-app messages: %s"), *Message);
    });

    UBillingCPPLibrary::ShowInAppMessages(OnSuccess, OnFailure);
}
```