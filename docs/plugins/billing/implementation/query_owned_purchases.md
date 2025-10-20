# Query Owned Purchases

This is an **asynchronous function** that retrieves all currently owned items purchased by the player — including **active subscriptions** and **non-consumed in-app products**.  
It can be used to verify ownership status or restore purchases when launching the game.  

> **Note:**  
> This function uses the **local cache** of the Google Play Store app without performing a network request.  
> When testing subscriptions with test accounts, this cache may not always reflect the most recent state.  

The function takes a **Product Type** parameter to specify which purchases to query — either `InApp` or `Subscription`.  

Once the asynchronous operation completes, one of two delegates will be triggered to handle success or failure.

<div align="center">
  <img src="../assets/query_owned_purchases/query_owned_purchases_node.png" width="280">
</div>

---

## Parameters  

### 1. Product Type  
Specifies which type of products to query — either **InApp** or **Subscription**.  
See [`EProductType`](../object_definitions/enumerations.md#eproducttype) for available values.

---

### 2. On Success  
Triggered when all currently owned purchases have been successfully retrieved.  
Returns an array of [`FPurchase`](../object_definitions/purchase.md#fpurchase) representing all active or non-consumed purchases.  
You can drag this pin to create a custom event and process or restore owned items.

---

### 3. On Failure  
Triggered if an error occurs while fetching owned purchases.  
Returns:  
- **Response Code** — [`EBillingResponseCode`](../object_definitions/enumerations.md#ebillingresponsecode)  
- **Error Message** — `FString` with additional debug information.  

Use this delegate to handle cache or service-related issues gracefully.

---

## Example  

In the example below, we are querying **owned in-app products** using the product type `InApp`.  
This allows you to restore previously purchased non-consumable items or verify active entitlements for the player.  

Depending on your use case, you can create your own functions to process each owned purchase and reward or unlock the corresponding content.  
You can also implement your own error handler for debugging and connection validation.  

If you want to verify **active subscriptions**, you can use the same setup but select `Subscription` as the product type.

<div align="center">
  <img src="../assets/query_owned_purchases/query_owned_purchases_example.png" width="620">
</div>

---

## C++ Usage  

This example demonstrates how to query all currently owned purchases using **C++**.  
It’s useful when restoring non-consumable purchases or verifying active subscriptions on startup.

```cpp
#include "BillingCPPLibrary.h"

void UMyGameInstance::QueryOwnedPurchases()
{
    FOnOwnedPurchasesQuerySuccessfulNative OnSuccess;
    OnSuccess.BindLambda([](const TArray<FPurchase>& Purchases)
    {
        for (const FPurchase& Purchase : Purchases)
        {
            UE_LOG(LogTemp, Log, TEXT("Owned Purchase: %s"), *Purchase.OrderId);
        }
    });

    FOnOwnedPurchasesQueryFailedNative OnFailure;
    OnFailure.BindLambda([](EBillingResponseCode Code, const FString& Message)
    {
        UE_LOG(LogTemp, Error, TEXT("Failed to query owned purchases. Code: %d, Message: %s"), (int32)Code, *Message);
    });
	
	UBillingCPPLibrary::QueryOwnedPurchases(EProductType::INAPP, OnSuccess, OnFailure);
}
```