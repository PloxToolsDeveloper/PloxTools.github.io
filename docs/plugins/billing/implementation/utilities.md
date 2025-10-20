# Utilities  

This section contains additional **utility functions** designed to simplify integration and improve control flow.  
These helper functions let you quickly check the billing client state, feature support, product type behavior, and retrieve offer tokens when working with subscriptions.  

Available utility functions:

- **IsBillingReady** — checks if the Billing Client is initialized and ready to use.  
- **IsFeatureSupported** — verifies if a specific billing feature (e.g. subscriptions, in-app messaging) is available.  
- **IsProductNonConsumable** — checks whether a given product ID has been marked as non-consumable.  
- **GetSubscriptionOfferToken** — retrieves the offer token for a specific subscription base plan or offer.  

---

## Is Billing Ready  

A quick utility function used to check if there’s an **active connection** to the **Google Play Billing service** and the client is ready for operations.  

Use this before calling any billing-related functions (such as querying products or launching purchases) to ensure the billing client is initialized.  

**Returns:** `bool` — `true` if the billing client is initialized and ready, otherwise `false`.

<div align="center">
  <img src="../assets/utilities/utilities_isbillingready.png" width="200">
</div>

---

## Is Feature Supported  

A simple utility function that checks whether a specific **billing feature** is supported by the version of the **Google Play Store** installed on the user’s device.  

This can be useful to verify if features such as subscriptions, price change confirmation, or in-app messaging are available before attempting to use them.  

<div align="center">
  <img src="../assets/utilities/utilities_isfeaturesupported.png" width="360">
</div>

### Parameter  

#### Feature Type  
Select which [`EFeatureType`](../object_definitions/enumerations.md#efeaturetype) you want to verify support for.  

**Returns:** `bool` — `true` if the feature is supported, otherwise `false`.

---

## Is Product Non-Consumable  

Checks if a given **Product ID** has been previously added to the list of **non-consumable products** using the [`SetNonConsumableProducts`](../implementation/set_nonconsumable_products.md) function.  

This helps determine whether an item can only be purchased once (for example, “Remove Ads” or “Premium Pack”).  

<div align="center">
  <img src="../assets/utilities/utilities_isproductnonconsumable.png" width="290">
</div>

### Parameter  

#### Product Id  
The product identifier to check, matching the one defined in the **Google Play Console**.  

**Returns:** `bool` — `true` if the product is marked as non-consumable, otherwise `false`.

---

## Get Subscription Offer Token  

A helper function that extracts the **Offer Token** for a specific subscription offer from the list of [`FProductDetails`](../object_definitions/product_details.md#fproductdetails) previously obtained via [`Query Product Details`](../implementation/query_product_details.md).  

This token is required when launching a billing flow for a subscription offer or base plan.

Internally, the function iterates through the provided list of subscription products, matching the given **Product ID**, **Base Plan ID**, and (optionally) **Offer ID**.  
If an exact match is found, it returns the corresponding offer token string.  
If none match, it returns an empty string.

<div align="center">
  <img src="../assets/utilities/utilities_getsubscriptionoffertoken.png" width="290">
</div>

### Parameters  

#### 1. Subscription Products  
A list of subscription [`Product Details`](../object_definitions/product_details.md#fproductdetails) retrieved from the Play Console through [`Query Product Details`](../implementation/query_product_details.md).

#### 2. Product ID  
The subscription’s product identifier as set up in the **Google Play Console**.

#### 3. Base Plan ID  
The ID of the base plan for the subscription.  
If left empty, the function will return the first matching plan’s token.

#### 4. Offer ID  
The ID of a specific promotional offer under the base plan.  
If empty, the default base plan token will be returned.

**Returns:**  
`FString` — the corresponding **Offer Token** if found, otherwise an empty string.

---

## C++ Usage 

Below are short examples of how to use the utility functions directly in C++ code.  
These can be useful when handling billing logic outside of Blueprints.

### Check if Billing Client is Ready

```cpp
#include "BillingCPPLibrary.h"

if (UBillingCPPLibrary::IsBillingReady())
{
    UE_LOG(LogTemp, Log, TEXT("Billing client is ready for use."));
}
else
{
    UE_LOG(LogTemp, Warning, TEXT("Billing client is not initialized or not connected."));
}
```

---

### Check if a Billing Feature is Supported

```cpp
#include "BillingCPPLibrary.h"

if (UBillingCPPLibrary::IsFeatureSupported(EFeatureType::SUBSCRIPTIONS))
{
    UE_LOG(LogTemp, Log, TEXT("Subscriptions are supported on this device."));
}
else
{
    UE_LOG(LogTemp, Warning, TEXT("Subscriptions are not supported."));
}
```

---

### Check if a Product is Non-Consumable

```cpp
#include "BillingCPPLibrary.h"

const FString ProductId = TEXT("remove_ads");
if (UBillingCPPLibrary::IsProductNonConsumable(ProductId))
{
    UE_LOG(LogTemp, Log, TEXT("Product %s is marked as non-consumable."), *ProductId);
}
else
{
    UE_LOG(LogTemp, Log, TEXT("Product %s is consumable."), *ProductId);
}
```

---

### Get Subscription Offer Token

```cpp
#include "BillingCPPLibrary.h"

TArray<FProductDetails> SubscriptionProducts = CachedProductDetails;
FString OfferToken = UBillingCPPLibrary::GetSubscriptionOfferToken(
    SubscriptionProducts,
    TEXT("premium_monthly"),
    TEXT("monthly_baseplan"),
    TEXT("promo_offer")
);

if (!OfferToken.IsEmpty())
{
    UE_LOG(LogTemp, Log, TEXT("Offer Token: %s"), *OfferToken);
}
else
{
    UE_LOG(LogTemp, Warning, TEXT("Failed to find offer token for the given subscription IDs."));
}
```