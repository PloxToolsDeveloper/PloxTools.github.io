# Query Product Details

This is an asynchronous function that retrieves [Product Details](../object_definitions/product_details.md#fproductdetails) from the **Google Play Console**.  
It returns detailed data for each product ID you provide, including pricing, currency, title, and subscription offers.

You can use the returned information to display in-game shop items or manage available subscriptions.  
The function also provides **Offer Tokens**, which can be used later when Launching a Billing Flow.

Parameters include a list of **Product IDs** and a [Product Type](../object_definitions/enumerations.md#eproducttype) that determines whether the query targets in-app products or subscriptions.  
Once the operation completes, one of two delegates will trigger — success or failure — depending on the result of the query.

<div align="center">
  <img src="../assets/query_product_details/query_product_details_node.png" width="300">
</div>

---

## Parameters

### 1. Product Ids  
An array of product IDs to request from the **Google Play Console**.  
Each ID should match a product you’ve configured in your Play Store listing.

---

### 2. Product Type  
Specifies the type of products to query — either **In-App** or **Subscription**.  
See [`EProductType`](../object_definitions/enumerations.md#eproducttype) for available values.

---

### 3. On Success  
Triggered when product details have been successfully retrieved.  
Returns an array of [`FProductDetails`](../object_definitions/product_details.md#fproductdetails) that includes all pricing and metadata information.  
You can drag this pin to create a custom event and use the data in your shop or UI.

---

### 4. On Failure  
Triggered when the request fails.  
Returns:  
- **Response Code** — [`EBillingResponseCode`](../object_definitions/enumerations.md#ebillingresponsecode)  
- **Error Message** — `FString` with additional debug information.  

Use this delegate to handle issues.

---

## Example

In the example below, the **Query Product Details** node requests information for a list of in-app products retrieved from the **Google Play Console**.  
The results are then passed to a custom event, which updates the product list in the in-game shop UI.

If the query fails, the **Handle Error** function is called, allowing you to process the error code and message for debugging or user feedback.

This setup can be extended for subscriptions — simply change the **Product Type** to *Subscription* and store the results for use when launching a purchase.

<div align="center">
  <img src="../assets/query_product_details/query_product_details_example.png" width="1000">
</div>

---

## C++ Usage

You can fetch product details directly from C++ by providing an array of product IDs and handling success or failure through delegates.

```cpp
#include "BillingCPPLibrary.h"

void UMyGameInstance::FetchProductDetails()
{
    TArray<FString> ProductIDs = { TEXT("coins_pack_1"), TEXT("premium_upgrade") };

    FOnProductDetailsQuerySuccessfulNative OnSuccess;
    OnSuccess.BindLambda([](const TArray<FProductDetails>& ProductDetails)
    {
        UE_LOG(LogTemp, Log, TEXT("Successfully retrieved %d product details."), ProductDetails.Num());
    });

    FOnProductDetailsQueryFailedNative OnFailure;
    OnFailure.BindLambda([](EBillingResponseCode Code, const FString& Message)
    {
        UE_LOG(LogTemp, Error, TEXT("Failed to query product details: %s (Code: %d)"), *Message, static_cast<int32>(Code));
    });

    UBillingCPPLibrary::QueryProductDetails(ProductIDs, EProductType::INAPP, OnSuccess, OnFailure);
}
```