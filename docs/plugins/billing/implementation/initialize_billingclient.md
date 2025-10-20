# Initialize Billing Client

This is an asynchronous function that initializes the connection between your project and the **Google Play Billing** service.  
It must be called before any other billing operations.  

Once the setup is complete, one of two delegates will trigger — indicating whether the billing client was successfully connected or if the initialization failed.  

Use this function at the start of your game or before performing any billing-related actions.

<div align="center">
  <img src="../assets/initialize_billingclient/initialize_billingclient_node.png" width="250">
</div>

---

## Parameters

### 1. On Success  
A delegate that is triggered when the Billing Client has been successfully initialized.  
No data is returned — it simply confirms that the billing connection is ready to use.  

You can use this pin to start querying product details.

---

### 2. On Failure  
A delegate that is triggered if the initialization fails.  
It provides two parameters:  

- **Response Code** — [`EBillingResponseCode`](../object_definitions/enumerations.md#ebillingresponsecode) indicating the type of error.  
- **Message** — `FString` with additional debug information describing the failure reason.  

Use this delegate to handle initialization errors and show appropriate messages to the player.

---

## Example

In the below example, the **Initialize Billing Client** node is called before any billing operations.  
Once the billing service is successfully connected, the [`Query Product Details`](../implementation/query_product_details.md) function is executed to retrieve product information from the Google Play Console.

If initialization fails, the **Handle Error** function is called, passing the response code and message for debugging or user feedback.

This setup ensures that your game only performs billing actions when the client connection is fully ready.

<div align="center">
  <img src="../assets/initialize_billingclient/initialize_billingclient_example.png" width="600">
</div>

---

## C++ Usage

You can also initialize the billing client directly from C++ using delegates for success and failure callbacks.

```cpp
#include "BillingCPPLibrary.h"

void UMyGameInstance::InitBilling()
{
    FOnBillingClientInitializedNative OnSuccess;
    OnSuccess.BindLambda([]()
    {
        UE_LOG(LogTemp, Log, TEXT("Billing client initialized successfully."));
    });

    FOnBillingClientInitializationFailedNative OnFailure;
    OnFailure.BindLambda([](EBillingResponseCode Code, const FString& Message)
    {
        UE_LOG(LogTemp, Error, TEXT("Billing initialization failed: %s (Code: %d)"), *Message, static_cast<int32>(Code));
    });

    UBillingCPPLibrary::InitializeBillingClient(OnSuccess, OnFailure);
}
```