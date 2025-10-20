# Set Non-Consumable Products

This is a synchronous function which adds the specified product IDs to the **non-consumable** list.  
Products registered as non-consumable can only be purchased **once** — after a successful purchase, they remain owned permanently and will not be consumed automatically.  

If the player attempts to buy a non-consumable product again, Google Play will show a system message indicating that the item has already been purchased.  

You should call this function **after initializing the Billing Client**, typically at the **start of the game**, and pass in a list of all your one-time product IDs (for example, “RemoveAds” or “PremiumPack”).

Use this for **one-time purchases**, such as unlocking premium features, removing ads, or granting permanent bonuses.

<div align="center">
  <img src="../assets/set_nonconsumable_products/set_nonconsumable_products_node.png" width="300">
</div>

---

## Parameters  

### 1. Product Ids  
An array of product IDs that should be treated as **non-consumable**.  
Each ID must match a product configured in the **Google Play Console**.  
These products can only be purchased **once** and will remain owned by the user after purchase.

---

## Example  

In this example, we set up a list of **non-consumable** products — items that can only be purchased once, such as ad removal or premium access.  

The `Set Non Consumable Products` node is called before the [`Launch Billing Flow`](../implementation/launch_billing_flow.md) function to ensure that these products are properly marked as non-consumable.  

When the player attempts to purchase any of these items, Google Play will handle the validation automatically and display a message if the product has already been bought.

<div align="center">
  <img src="../assets/set_nonconsumable_products/set_nonconsumable_products_example.png" width="650">
</div>

---

## C++ Usage  

Here’s how you can register non-consumable products directly in C++:  

```cpp
#include "BillingCPPLibrary.h"

TArray<FString> NonConsumableProducts = {
    TEXT("remove_ads"),
    TEXT("premium_pack")
};

UBillingCPPLibrary::SetNonConsumableProducts(NonConsumableProducts);
```