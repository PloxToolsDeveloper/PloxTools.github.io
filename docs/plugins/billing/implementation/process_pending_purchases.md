# Process Pending Purchases

This asynchronous function checks for **pending purchases** that were completed by the player but not yet acknowledged.  
Pending purchases can occur when the app was closed or the network connection was lost before the confirmation step.  

Calling this function ensures that all unprocessed transactions are properly finalized and acknowledged on Google Play.  
It should typically be used when your game starts after Billing Client successfully initialized.  

Once processing is complete, one of the provided delegates will trigger — either for success or failure.
If there are **no pending purchases**, neither delegate will be called.

<div align="center">
  <img src="../assets/process_pending_purchases/process_pending_purchases_node.png" width="300">
</div>

---

## Parameters  

### 1. Product Type  
Specifies whether to process pending purchases for **InApp** products or **Subscriptions**.  
See [`EProductType`](../object_definitions/enumerations.md#eproducttype) for available values.  

---

### 2. On Success  
Triggered when pending purchase have been successfully acknowledged.  
Returns [`FPurchase`](../object_definitions/purchase.md#fpurchase) object containing the finalized purchase data.  
Use this delegate to confirm rewards or update user entitlements.  

---

### 3. On Failure  
Triggered if an error occurs while checking or processing pending purchases.  
Returns:  
- **Response Code** — [`EBillingResponseCode`](../object_definitions/enumerations.md#ebillingresponsecode)  
- **Error Message** — `FString` with debug information.  

If no pending purchases are found, neither delegate will be called.

---

## Example  

In the below example, we check for any **pending purchases** that may have completed while the app was closed or offline.  
Depending on your project, you can create your own function to handle these recovered purchases and properly reward the player.  

This function is typically called **at game startup** to ensure all completed transactions are acknowledged and players receive their items.  

Use the **ProductType** parameter to handle **InApp** products and **Subscriptions** separately if needed — just make sure to process both types to cover all cases.

<div align="center">
  <img src="../assets/process_pending_purchases/process_pending_purchases_example.png" width="620">
</div>

---

## C++ Usage  

You can process and acknowledge all pending purchases directly from C++ using the `ProcessPendingPurchases` function.  
This is typically done after successful Billing Client initialization to ensure that any completed but unacknowledged transactions are finalized.  

```cpp
#include "BillingCPPLibrary.h"

void UMyGameInstance::HandlePendingPurchases()
{
    FOnPendingPurchasesProcessingSuccessfulNative OnSuccess;
    OnSuccess.BindLambda([](const FPurchase& Purchase)
    {
        UE_LOG(LogTemp, Log, TEXT("Pending purchase acknowledged."));
    });

    FOnPendingPurchasesProcessingFailedNative OnFailure;
    OnFailure.BindLambda([](EBillingResponseCode Code, const FString& Message)
    {
        UE_LOG(LogTemp, Error, TEXT("Failed to process pending purchases: %s (Code: %d)"), *Message, static_cast<int32>(Code));
    });

    UBillingCPPLibrary::ProcessPendingPurchases(EProductType::INAPP, OnSuccess, OnFailure);
}
```