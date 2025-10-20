# Launch Billing Flow

This asynchronous function starts the purchase flow for a selected product.  
It connects directly to the **Google Play Billing** system and displays the native purchase UI to the user.

Depending on the product type, this function can launch either a one-time purchase or a subscription flow.  
For subscriptions, an **Offer Token** must be provided to specify a particular base plan or promotional offer.

Once the purchase process finishes, one of the delegates will trigger — indicating whether the purchase succeeded, is pending, or failed.

<div align="center">
  <img src="../assets/launch_billing_flow/launch_billing_flow_node.png" width="250">
</div>

--- 

## Parameters

### 1. Product Id  
The unique product ID to purchase, as defined in your **Google Play Console**.  
Must exactly match the ID used when configuring your product.  

---

### 2. Product Type  
Specifies whether the purchase is a **In-App** product or a **Subscription**.  
See [`EProductType`](../object_definitions/enumerations.md#eproducttype) for available values.  

---

### 3. Offer Token  
A required token for **subscription purchases**, representing the selected base plan or offer.  
Can be obtained from the queried [`FProductDetails`](../object_definitions/product_details.md#fproductdetails) data by using [`Query Product Details`](../implementation/query_product_details.md) node.  
For **in-app products**, this parameter can be left empty.  

---

### 4. On Success  
Triggered when the purchase completes successfully.  
Returns a [`FPurchase`](../object_definitions/purchase.md#fpurchase) object containing all purchase data.  
Use this delegate to unlock content or grant in-game items.  

---

### 5. On Pending  
Triggered when a purchase is **pending** (e.g., cash-based payments or delayed transactions).  
You can notify the player that the transaction is awaiting confirmation.  

---

### 6. On Failure  
Triggered if the billing flow fails, is cancelled, or encounters a network issue.  
Returns:  
- **Response Code** — [`EBillingResponseCode`](../object_definitions/enumerations.md#ebillingresponsecode)  
- **Error Message** — `FString` with debug information.  

Use this delegate to log errors and handle cancellations.

---
 
## Examples  

### In-App Purchase

In the below example, we are launching a purchase flow for an **In-App Product** using its product ID from the **Google Play Console**.  
You can create your own functions to handle successful purchases, pending transactions, or failures, depending on your project’s logic.  

For **In-App Product** purchases, the **Offer Token** field can be left empty.  
For **Subscription** purchases, you must provide a valid token corresponding to the selected base plan or offer.

<div align="center">
  <img src="../assets/launch_billing_flow/launch_billing_flow_exampleinapp.png" width="670">
</div>

---

### Subscription

In this example, we’re starting a **subscription purchase** using a product ID from the **Google Play Console**.  
Depending on your project’s setup, you can implement your own logic to handle successful purchases, pending states, and failures.

The key difference when working with subscriptions is the use of an **Offer Token**, which identifies the selected base plan and offer.  
You can obtain this token using the [`Get Subscription Offer Token`](../implementation/utilities.md#get-subscription-offer-token) node from the previously retrieved [`FProductDetails`](../object_definitions/product_details.md#fproductdetails) data by using [`Query Product Details`](../implementation/query_product_details.md) node.

If a subscription has multiple base plans or offers, make sure to store the relevant product details and select the desired one when launching the billing flow.  
If there’s only one plan available, the **Offer Token** can be left blank — the first available plan will be used automatically.

<div align="center">
  <img src="../assets/launch_billing_flow/launch_billing_flow_examplesubs.png" width="670">
</div>

---

## C++ Usage

You can also launch the billing flow directly from C++ using the static `LaunchBillingFlow` function.  
Provide a valid **Product ID**, **Product Type**, and (optionally) an **Offer Token** for subscriptions.

### In-App Purchase

```cpp
#include "BillingCPPLibrary.h"

void UMyGameInstance::StartPurchase()
{
    FString ProductId = TEXT("gems_pack_1");

    FOnPurchaseSuccessfulNative OnSuccess;
    OnSuccess.BindLambda([](const FPurchase& Purchase)
    {
        UE_LOG(LogTemp, Log, TEXT("Purchase successful"));
    });

    FOnPurchasePendingNative OnPending;
    OnPending.BindLambda([](const FPurchase& Purchase)
    {
        UE_LOG(LogTemp, Warning, TEXT("Purchase pending"));
    });

    FOnPurchaseFailedNative OnFailure;
    OnFailure.BindLambda([](EBillingResponseCode Code, const FString& Message)
    {
        UE_LOG(LogTemp, Error, TEXT("Purchase failed: %s (Code: %d)"), *Message, static_cast<int32>(Code));
    });

    UBillingCPPLibrary::LaunchBillingFlow(ProductId, EProductType::INAPP, OnSuccess, OnPending, OnFailure);
}
```

---

### Subscription

```cpp
#include "BillingCPPLibrary.h"

void UMyGameInstance::StartPurchase()
{
    FString ProductId = TEXT("premium_subscription");

    // For subscriptions, retrieve the offer token from your stored ProductDetails
    FString OfferToken = TEXT("baseplan_offer_token");

    FOnPurchaseSuccessfulNative OnSuccess;
    OnSuccess.BindLambda([](const FPurchase& Purchase)
    {
        UE_LOG(LogTemp, Log, TEXT("Purchase successful"));
    });

    FOnPurchasePendingNative OnPending;
    OnPending.BindLambda([](const FPurchase& Purchase)
    {
        UE_LOG(LogTemp, Warning, TEXT("Purchase pending"));
    });

    FOnPurchaseFailedNative OnFailure;
    OnFailure.BindLambda([](EBillingResponseCode Code, const FString& Message)
    {
        UE_LOG(LogTemp, Error, TEXT("Purchase failed: %s (Code: %d)"), *Message, static_cast<int32>(Code));
    });

    UBillingCPPLibrary::LaunchBillingFlow(ProductId, EProductType::INAPP, OfferToken, OnSuccess, OnPending, OnFailure);
}
```