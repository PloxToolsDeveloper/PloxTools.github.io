# Getting Started

## Purchase the Plugin

Purchase the plugin on the [**Fab Marketplace**](https://www.fab.com/sellers/PloxTools).

---

## Install the Plugin

After purchasing, the plugin can be installed either directly through the Fab website or via the Fab plugin inside Unreal Engine.
Once installed, it will appear in your project’s plugin list and can be enabled from there.

---

## Enable Google Play Billing Plugin

Once installed, the plugin will be available in Unreal Engine under the **Plugins** section.  
Go to **Edit → Plugins**, search for **"PloxTools: Google Play Billing"**, and make sure it’s enabled.

<div align="center">
  <img src="../assets/getting_started/billing_gettingstarted_plugin.png" width="1000">
</div>

---

## Disable Any Other Billing Plugins

Make sure that no other billing-related plugins are active, as they may override or conflict with this plugin.  
Go to **Edit → Plugins** and disable any third-party google play billing plugins.

---

## Enable Google Play Support

In order for the billing functions to work correctly, **Google Play Support** must be enabled.  
Go to **Project Settings → Android → Google Play Services**, then check the box **"Enable Google Play Support"**.

<div align="center">
  <img src="../assets/getting_started/billing_gettingstarted_googlesupport.png" width="1000">
</div>

---

## Add Your Google Play License Key

Add your **Google Play License Key** from the Google Play Console:  
Go to **Project Settings → Android → Google Play Services**, and paste your key in the field **"Google Play License Key"**.

<div align="center">
  <img src="../assets/getting_started/billing_gettingstarted_licencekey.png" width="1000">
</div>

---

## Where to Find Your License Key in Google Play Console

You can find your **Google Play License Key** in the **Google Play Console** under your game project:

1. Open your game in the [Google Play Console](https://play.google.com/console).  
2. Navigate to **Monetize with Play → Monetization Setup**.  
3. Scroll down to the **Licensing** section — your key will be displayed there.  

<div align="center">
  <img src="../assets/getting_started/billing_gettingstarted_whereiskey.png" width="1000">
</div>

---

## Enable C++ Access (Optional)

If you plan to call the plugin directly from C++ code, you need to include its module in your project’s **Build.cs** file.

Open your project’s `*.Build.cs` file and add **"PloxToolsBilling"** to the public dependency modules list:

```cpp
PublicDependencyModuleNames.AddRange(new string[] { 
"Core", "CoreUObject", "Engine", "InputCore", 
"EnhancedInput", "PloxToolsBilling" });
```
<div align="center">
  <img src="../assets/getting_started/billing_gettingstarted_module.png" width="1000">
</div>

---

Once these steps are complete, the plugin is ready to use in your project.