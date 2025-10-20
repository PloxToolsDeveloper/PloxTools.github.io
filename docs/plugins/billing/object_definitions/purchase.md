# Purchase

The **Purchase** struct represents information about a completed or pending in-app transaction.  
It closely follows the structure provided by the Google Play Billing Library and contains all relevant data returned after a purchase is made.

---

## FPurchase

This struct represents information about a single purchase or subscription transaction made through Google Play Billing.

| Field | Type | Description |
|-------|------|-------------|
| **PurchaseState** | [`EPurchaseState`](enumerations.md#epurchasestate) | The current processing state of the purchase (e.g. Purchased, Pending, Unspecified). |
| **Quantity** | `int32` | Quantity of the purchased item. Usually `1` for standard purchases. |
| **PurchaseTime** | `int64` | Timestamp of the purchase in milliseconds since Unix epoch (January 1, 1970). |
| **DeveloperPayload** | `FString` | Optional developer-defined payload associated with the purchase (used for verification or tracking). |
| **OrderId** | `FString` | Unique order identifier assigned to this transaction by Google Play. |
| **PackageName** | `FString` | Package name of the app where the purchase was made. |
| **PurchaseToken** | `FString` | Unique token identifying the purchase for a specific user and item pair. Used for validation and consumption. |
| **Signature** | `FString` | Digital signature of the purchase data, generated using the developer’s private key. |
| **Products** | `TArray<FString>` | List of product IDs included in this transaction. |
| **bIsAcknowledged** | `bool` | Indicates whether this purchase has been acknowledged. Unacknowledged purchases may be automatically refunded. |
| **bIsAutoRenewing** | `bool` | Indicates whether the subscription renews automatically (applies only to subscription products). |