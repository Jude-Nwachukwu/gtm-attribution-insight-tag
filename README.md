# Attribution Insight Tag

A powerful GTM custom **tag template** that captures traffic source data and ad click identifiers, stores them as browser cookies or localStorage values, and pushes them to the dataLayer. Perfect for marketing attribution, offline conversion tracking, and lead source capture.

---

## 🚀 What This Tag Does

The **Attribution Insight Tag** automatically collects UTM parameters and common ad click identifiers from your URL (e.g., `gclid`, `fbclid`, `msclkid`), infers missing data from the referrer or fallback logic, and stores these values:

* As a **single cookie** or **separate cookies per data point**
* In browser **localStorage**
* In the **dataLayer** for use in GTM or GA4

---

## 🔧 Features

* Captures key traffic and campaign data: `utm_source`, `utm_medium`, `utm_campaign`, `utm_term`, `utm_content`, `utm_source_platform`, `utm_marketing_tactic`, `utm_creative_format` and `utm_id`
* Classifies referral traffic and reclassifies known search engine domains as **organic** instead of referral
* Supports **custom UTM parameter mapping**, allowing you to define your own UTM naming or analytics vendor-adapted syntax (e.g. `mtm_source`, `mtm_campaign`, `pk_medium`, `mtm_id`, `pk_source`, etc.) and map them to standard parameters like `utm_source`, `utm_medium`, etc.
* Supports AIO (AI Overview) Traffic Sources
* Fixed persistence issues for the following traffic information (campaign, campaign ID, term, content, source platform, marketing tactic, and creative format)
* Supports major ad click identifiers (e.g., `gclid`, `fbclid`, `ttclid`, etc.)
* **Supports custom click identifiers** (e.g., `awc`, `partnerid`, etc.) with optional source/medium mapping
* Configurable cookie strategy:

  * Single cookie (JSON format)
  * Separate cookies per parameter, with optional custom prefix
* Smart **case-insensitive** matching of UTM parameters
* Case-sensitive capture for all click identifiers
* Values stored in:

  * Cookie(s) (with refreshable expiration)
  * LocalStorage (`gtm_dd_traffic_data`)
* Supports referral domain exclusions
* Smart UTM and click identifier prioritization
* Fully customizable domain/path settings
* DataLayer push with default or custom event name
* Logic supports last-non-direct click model with intelligent override behavior
* Automatically clears out-of-date UTM data while preserving updated fields
* 🆕 **First-Click Attribution Support**  
  Enables persistent storage of a visitor’s **initial traffic source and medium** alongside their **click identifiers**.  
  This enhancement allows you to distinguish between a user’s **first touchpoint** and **last non-direct click** across sessions for improved attribution accuracy.
* 🆕 **Normalized Click Identifier Handling**  
  All **native and custom click identifiers** are now **normalized and stored as strings**, as an update to ensure **uniformity and consistency** when reading identifiers from **cookies**, **LocalStorage**, or the **dataLayer**.


---


## 📥 How to Import to GTM

1. Download the [template JSON file](#).
2. In your web GTM container UI, go to the **Templates** section under the **Admin** panel.
3. Click **New Tag Template**, then click the **3-dot menu** → **Import**.
4. Upload the downloaded JSON file.
5. Review and approve required permissions.
6. Save the template.

---

## ⚙️ Configuration Guide

### 🔹 Cookie Storage Type

* **Cookie Storage Type** (dropdown): Choose between:

  * `Use Separate Cookie Name`: Store each data point in a separate cookie
  * `Use Single Cookie Name`: Store all data in one cookie as an object

### 🔹 Cookie Settings

* **Cookie Name**: Used when "Single Cookie" is selected
* **Cookie Duration** & **Lifespan**: Define how long cookies persist (e.g., `30 days`)
* **Use Custom Prefix**: If using separate cookies, this sets a prefix (e.g., `utm_`) for each cookie name. If you don't use a custom cookie name prefix, it defaults to using the `gtm_dd_sep_` prefix.

### 🔹 Traffic Data Options

Enable checkboxes for any traffic parameters you'd like to capture:

* **Traffic Source** (`utm_source`)
* **Traffic Medium** (`utm_medium`)
* **Campaign** (`utm_campaign`)
* **Campaign ID** (`utm_id`)
* **Term**, **Content** – Additional UTM fields

### **🤖** AI Overview Traffic Sources

The Attribution Insight Tag now supports classifying **AI Overview (AIO) traffic sources**, such as **Featured Snippets**, **People Also Ask**, and **Google's AI Overviews**.  
These traffic sources often rely on special **Text Fragments** in the URL (after `#:~:text=`), which highlight the portion of the page the user was sent to.

Unlike the standard GTM `getUrl` custom template core API (`document.location.href`), which only returns the visible browser URL due to privacy reasons, the AIO detection uses the **Performance API** to capture the full navigation entry, ensuring text fragments are retained.

---

### ⚙️ How to Configure AIO Tracking

1. In the **AIT tag template**, if you have **Traffic Source** enabled, you'll have to enable the **AIO Traffic Source Detection** option.  
2. In the field **`URL Text Fragment (after #:~:text=)`**, provide a GTM **Custom JavaScript Variable** that extracts the text fragment from the navigation entry.  

Use this exact code in your **Custom JavaScript Variable**:

```javascript
function() {
  var aioURLCheckVar = performance.getEntries()[0].name;
  return aioURLCheckVar;
}

```

#### Custom UTM Parameter Mapping

The Attribution Insight Tag now supports **custom UTM parameter mapping**, enabling teams with non-standard URL parameter naming conventions to normalize data into standard UTM fields. For instance, if your URLs use `src=my_source` instead of `utm_source=my_source`, you can configure the tag to interpret `src` as `utm_source`. This ensures consistent attribution and analytics reporting without requiring changes to your URL structures.

When you check the **"Recognise Custom UTM Parameters"** option. You can define your mappings using the **Custom UTM Mappings** table, where:

- The **URL Parameter Key** column contains your custom parameter (e.g., `mtm_source`, `mtm_campaign`, `pk_medium`, `mtm_id`, `<your own query>`)
- The **Mapped UTM Field** column should be one of the standard fields (`utm_source`, `utm_medium`, `utm_campaign`, `utm_id`, `utm_source_platform`, etc.)

This mapping is **case-sensitive** and only overrides the specific UTM fields it maps to in the absence of the UTM parameter, which means you can use your standard UTM naming syntax and your own custom syntax. to—unmapped parameters remain unaffected.

### 🔹 Ad Click Identifiers

Enable checkboxes to capture:

* `gclid`, `fbclid`, `ttclid`, `li_fat_id`, `msclkid`, `twclid`, `obclid`, etc.
* Also supports **custom click identifiers** via a table configuration
* Case-sensitive matching for all click identifiers

### 🔹 Behavior Options

* **Use Last-Non Direct**: Enables last-non-direct model for source and medium

  * Automatically overridden by new UTMs, click identifiers, or valid referrers
* **Refresh Lifespan on Each Execution**: Resets cookie lifespan each time tag is fired

### 🔹 Local Storage and Data Layer

* **Store in LocalStorage**: Also persist values in LocalStorage under key `gtm_dd_traffic_data`
* **Disable Data Layer Push**: Prevents automatic `dataLayer` push
* **Use Custom Event Name**: Overrides the default `gtm_dd_marketing_traffic_data` event name

### 🔹 Referral Exclusion

* **Enable Referral Exclusion**: Prevents selected domains from being recognized as referral source
* **Exclusion Domains**: Add comma-separated domains (e.g., `stripe.com, paypal.com`)

### 🔹 Domain & Path

* Set custom domain or path if needed. Defaults to top-level domain and root path

---

## 🧠 Logic Overview

1. **Case-Insensitive UTM Parsing**:

   * UTM parameters are handled without regard to casing, with a check for any use of custom UTM mapping.

2. **Case-Sensitive Click Identifier Parsing**:

   * Native and custom click identifiers preserve original casing for accuracy

3. **UTM Parameter Evaluation**:

   * If one or more UTM parameters exist, all values are updated accordingly
   * Missing UTM values are set to "none" to prevent fallback to old values
   * Stored values (cookie/localStorage) used only when no UTM fields exist

4. **Click Identifier Evaluation**:

   * When no UTM values are found but a valid click identifier is present, it sets a known traffic source and medium (e.g., `google / cpc` for `gclid`)
   * Always takes priority before fallback

5. **Source & Medium Logic**:

   * Order of evaluation:

     1. UTM Parameters
     2. Click Identifiers (default or custom)
     3. Valid Referrer (not excluded, not same-host)
     4. Cookie/localStorage (only when traffic is direct or referrer is excluded)
     5. Default: `direct / (none)`

6. **Referral Exclusion**:

   * Compares actual referrer host (not top-level domain) against exclusion list

7. **Last-Non Direct Logic**:

   * If traffic is `direct` or from an excluded referrer domain, fallback to stored values
   * Ensures campaign attribution persists during internal navigation

8. **Storage Mechanics**:

   * **Single Cookie**: JSON-encoded object with all enabled values
   * **Separate Cookies**: Each data point stored independently, with optional prefix
   * **LocalStorage**: Uses key `gtm_dd_traffic_data` with structured object

9. **Data Layer Push**:

   * Automatically fires a structured event with `marketing_data`, `click_identifiers`, and `datalayer_source`

---
## ⚙️ First-Click Attribution Configuration  

The **Attribution Insight Tag** now supports a dedicated **First-Click Attribution** feature, providing deeper attribution control and persistence across sessions.  

---

### 🔹 Enabling First-Click Attribution  

Enable **“Enable First-Click Attribution Settings”** to activate the first-click storage logic.  

**Choose your First-Click Attribution Storage Type:**  
- **Use Separate Cookie Name** → Creates independent cookies for each data point  
  *(e.g., `first_click_attr_gtm_dd_sep_source`, `first_click_attr_gtm_dd_sep_medium`).*  
- **Use Single Cookie Name** → Combines all first-click values into a single JSON object stored under the **First-Click Attribution Cookie Name**.  

**Configure:**  
- **First-Click Attribution Cookie Name:** Used when storing data in a single cookie.  
- **Use Custom Prefix:** For separate cookies, define your own prefix *(defaults to `first_click_attr_gtm_dd_sep_`)*.  
- **Cookie Lifespan (Days):** Determines how long the first-click attribution data remains available in days.  

---

### 🔹 Storage and Persistence  

When enabled, the tag stores a snapshot of:  
- **Marketing Data** (*utm_source, utm_medium, utm_campaign, etc.*)  
- **Click Identifiers** (*gclid, fbclid, msclkid, and any custom identifiers*)  

These values are stored as either:  
- Separate cookies *(per data point)*, or  
- A single combined cookie, and  
- Persisted in **LocalStorage** using the key (`gtm_dd_first_click_attr_data`).  

Once created, **first-click cookies remain constant** for their defined lifespan — they won’t be overwritten by subsequent traffic, preserving **original attribution integrity**.  

If **LocalStorage** contains valid first-click data but cookies are missing, the tag automatically restores the cookies.  

---

### 🔹 Updating Missing Click Identifiers  

If first-click attribution data already exists (e.g., from a prior session) but some click identifiers weren’t initially captured, the tag automatically checks for missing identifiers on each page load.  

When it detects new valid identifiers, they’re added to:  
- The existing **first-click cookie(s)**  
- The **LocalStorage** backup (`gtm_dd_first_click_attr_data`)  

This ensures your **first-click record remains complete** without losing prior stored data.  

---

### 🔹Enabgle Data Layer Behavior  

The **Data Layer push** follows a clear logic:  

When **Data Layer Push** is enabled, the tag always pushes an event containing the :  

```js
{
  event: "gtm_dd_marketing_traffic_data",
  marketing_data: {...},
  click_identifiers: {...},
  datalayer_source: "dd gtm custom tag template"
}
```
If the **“Push First-Click Attribution to Data Layer”** option is enabled, the tag appends the **first-click marketing data** object to the Data Layer.  

```js

first_click_attribution: {
  marketing_data: {...},
  data_point_source: "dd gtm custom tag template (first-click)"
}
```

Only **marketing_data** from the first-click record is pushed —  
**click identifiers** remain accessible only via **browser cookies** or **LocalStorage**,  
preventing unnecessary bloating of the Data Layer and avoiding potential confusion in downstream analytics.  


---
## 💼 Use Cases

### 📄 Lead Source Tracking

Capture source and medium into hidden form fields for lead attribution using cookie/localStorage values.

### 🔁 Offline Conversion Tracking

Send `gclid`, `fbclid`, and other identifiers to CRM/backend systems for offline event mapping.

### 📊 Funnel Attribution

Track consistent campaign attribution across the user journey using stored cookie values.

### 🔎 Debugging Attribution Issues

You can also use this **Attribution Insight Tag** GTM Template to send your own attribution data along with your events, which can later be utilized to troubleshoot attribution issues.

---

## 📄 License

Apache License 2.0

---

## 👨‍💻 Developed by

Template built by **Jude Nwachukwu Onyejekwe** for [DumbData](https://dumbdata.co/)

### 👉 Get Support or Raise an Issue

* Contact DumbData: [https://dumbdata.co/contact-us/](https://dumbdata.co/contact-us/)
* Explore more templates: [https://dumbdata.co/gtm-custom-templates/](https://dumbdata.co/gtm-custom-templates/)
* Found a bug or feature request? [Raise an issue here](#)
