# Product Details

The **ProductDetails** struct mirrors the information provided by the official Google Play Billing Library.  
It contains all key data about a product — such as its ID, title, description, price, currency, and offer details.

To stay consistent with Google’s SDK, the struct also includes a few nested types:

- **OneTimePurchaseOfferDetails** — data for one-time purchases (consumable or non-consumable).  
- **SubscriptionOfferDetails** — data for subscription-based products.  
- **PricingPhases** — information about price changes and phases for subscriptions.

In short, **ProductDetails** gives you everything needed to display, identify, and process items available for purchase through Google Play.

---

## FProductDetails

Holds general information about a product listed in the Google Play Console — such as title, description, type, and related pricing or subscription data.

| Field | Type | Description |
|-------|------|-------------|
| **ProductId** | `FString` | Unique product identifier as defined in the Google Play Console. |
| **Title** | `FString` | The product title as it appears in the Google Play Console. |
| **Description** | `FString` | The short description of the product shown to users in the store. |
| **Name** | `FString` | Internal or display name used in your project (optional alias). |
| **ProductType** | [`EProductType`](enumerations.md#eproducttype) | Type of the product — either **In-App** or **Subscription**. |
| **OneTimePurchaseOfferDetails** | `FOneTimePurchaseOfferDetails` | Contains pricing and currency information for one-time (consumable or non-consumable) products. |
| **SubscriptionOfferDetails** | `TArray<FSubscriptionOfferDetails>` | List of subscription offers, including base plans and offer combinations. |

---

## FOneTimePurchaseOfferDetails

Contains price and currency information for one-time (consumable or non-consumable) products.

| Field | Type | Description |
|-------|------|-------------|
| **PriceAmountMicros** | `int64` | Product price in micro-units (e.g. `1 € = 1 000 000`). Used for precise currency calculations. |
| **FormattedPrice** | `FString` | The price string displayed to the user (e.g. “€1.99”). |
| **PriceCurrencyCode** | `FString` | The ISO 4217 currency code (e.g. “EUR”, “USD”). |

---

## FSubscriptionOfferDetails

This struct contains detailed information about a specific subscription offer or base plan available in the Google Play Console.

| Field | Type | Description |
|-------|------|-------------|
| **PricingPhases** | `TArray<FPricingPhase>` | List of pricing phases that define the price and duration of each billing period (e.g., trial, discount, regular). |
| **BasePlanId** | `FString` | The base plan ID configured in the Google Play Console (e.g. “monthly_plan”). |
| **OfferId** | `FString` | Optional identifier for a specific offer under the base plan (e.g. “promo3months”). |
| **OfferToken** | `FString` | Token used when launching the billing flow to purchase this specific offer. |
| **OfferTags** | `TArray<FString>` | Custom tags assigned to the offer for internal grouping or filtering. |

---

## FPricingPhase

Each **PricingPhase** describes a single billing period within a subscription — such as a free trial, discounted period, or regular recurring payment.

| Field | Type | Description |
|-------|------|-------------|
| **BillingCycleCount** | `int32` | Number of times this pricing phase repeats (e.g. `1` for a single free month). |
| **RecurrenceMode** | [`ERecurrenceMode`](enumerations.md#erecurrencemode) | Defines how the phase repeats — once, finite, or infinite recurring. |
| **PriceAmountMicros** | `int64` | Price in micro-units (`1 € = 1,000,000`). Used for precise currency calculations. |
| **BillingPeriod** | `FString` | Duration of the phase in ISO 8601 format (e.g. `P1M` for one month, `P1Y` for one year). |
| **FormattedPrice** | `FString` | Price string as displayed to the user (e.g. “€4.99”). |
| **PriceCurrencyCode** | `FString` | ISO 4217 currency code (e.g. “EUR”, “USD”). |