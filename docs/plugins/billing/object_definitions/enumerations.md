# Enumerations

This section lists all enumerations used throughout the plugin.  
Each enum reflects the constants defined in the official **Google Play Billing Library**, adapted for use in Unreal Engine.

These enums are primarily used to describe product types, purchase states, and billing response codes returned by the API.

---

## EProductType

Defines the type of product available in the Google Play Console.

| Name | Description |
|------|--------------|
| **INAPP** | One-time in-app product (consumable or non-consumable). |
| **SUBS** | Subscription product that renews automatically or follows an active base plan. |

---

## ERecurrenceMode

Describes how a subscription pricing phase repeats over time.

| Name | Description |
|------|--------------|
| **Unknown** | The recurrence mode is not defined or returned by the Billing Library. |
| **InfiniteRecurring** | The phase repeats indefinitely until the user cancels the subscription. |
| **FiniteRecurring** | The phase repeats a fixed number of times (e.g., 3 discounted months). |
| **NonRecurring** | The phase occurs only once and does not repeat. |

---

## EPurchaseState

Represents the current processing state of a purchase returned by the Google Play Billing Library.

| Name | Description |
|------|--------------|
| **Unspecified** | The purchase state is unknown or not set. |
| **Purchased** | The purchase was completed successfully and the item is owned by the user. |
| **Pending** | The purchase is awaiting payment or user confirmation (pending state). |

---

## EFeatureType

Defines supported billing features that can be checked using the Google Play Billing client.

| Name | Description |
|------|--------------|
| **SUBSCRIPTIONS** | Support for querying and purchasing subscription products. |
| **SUBSCRIPTIONS_UPDATE** | Ability to update or replace an active subscription with another plan. |
| **PRICE_CHANGE_CONFIRMATION** | Allows launching a price change confirmation flow for active subscriptions. |
| **IN_APP_MESSAGING** | Enables in-app messaging features such as promotional or confirmation dialogs. |
| **PRODUCT_DETAILS** | Support for querying product details and performing purchases via the Billing Library. |

---

## EBillingResponseCode

Represents response codes returned by the Google Play Billing Library after performing billing operations.

| Name | Description |
|------|--------------|
| **OK** | The operation completed successfully. |
| **USER_CANCELED** | The user canceled the purchase flow. |
| **SERVICE_UNAVAILABLE** | Network connection is down or the service is temporarily unavailable. |
| **BILLING_UNAVAILABLE** | Billing API version is not supported for the requested type. |
| **ITEM_UNAVAILABLE** | The requested product is not available for purchase. |
| **DEVELOPER_ERROR** | Invalid configuration or request parameters in the developer’s code. |
| **ERROR** | A generic error occurred during the operation. |
| **ITEM_ALREADY_OWNED** | The user already owns this item. |
| **ITEM_NOT_OWNED** | The user does not own this item. |
| **NETWORK_ERROR** | A network error occurred during the purchase flow. |
| **SERVICE_DISCONNECTED** | The connection to the Google Play service was lost. |
| **FEATURE_NOT_SUPPORTED** | The requested feature is not supported on the current device or account. |
| **SERVICE_TIMEOUT** | The request to the billing service timed out. |
| **UNKNOWN** | An unknown or unexpected response was returned. |