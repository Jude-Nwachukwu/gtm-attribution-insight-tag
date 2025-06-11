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

* Captures key traffic and campaign data: `utm_source`, `utm_medium`, `utm_campaign`, `utm_term`, `utm_content`, and `utm_id`
* Supports major ad click identifiers (e.g., `gclid`, `fbclid`, `ttclid`, etc.)
* Configurable cookie strategy:

  * Single cookie (JSON format)
  * Separate cookies per parameter, with optional custom prefix
* Smart **case-insensitive** matching of query parameters
* Values stored in:

  * Cookie(s) (with refreshable expiration)
  * LocalStorage (`gtm_dd_traffic_data`)
* Supports referral domain exclusions
* Smart UTM and click identifier prioritization
* Fully customizable domain/path settings
* DataLayer push with default or custom event name
* Logic supports last-non-direct click model with intelligent override behavior

---

## 📥 How to Import to GTM

1. Download the [template JSON file](#).
2. In GTM, go to the **Templates** section under the **Admin** panel.
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
* **Use Custom Prefix**: If using separate cookies, this sets a prefix (e.g., `utm_`) for each cookie name

### 🔹 Traffic Data Options

Enable checkboxes for any traffic parameters you'd like to capture:

* **Traffic Source** (`utm_source`)
* **Traffic Medium** (`utm_medium`)
* **Campaign** (`utm_campaign`)
* **Campaign ID** (`utm_id`)
* **Term**, **Content** – Additional UTM fields

### 🔹 Ad Click Identifiers

Enable checkboxes to capture:

* `gclid`, `fbclid`, `ttclid`, `li_fat_id`, `msclkid`, `twclid`, `obclid`, etc.
* Case-insensitive matching supported for all identifiers

### 🔹 Behavior Options

* **Use Last-Non Direct**: Enables last-non-direct model for source and medium; UTM and click identifiers override if present
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

1. **Case-Insensitive Query Parsing**: Extracts UTM parameters and click identifiers from the URL regardless of case
2. **UTM Parameter Evaluation**:

   * If a UTM value is present, it is stored
   * Missing UTM values default to `"none"`, not stored values
3. **Click Identifier Evaluation**:

   * If present, corresponding source and medium are inferred (e.g., `gclid` = `google / cpc`)
   * Case-insensitive handling of identifiers
   * Does not overwrite stored values unless present
4. **Source & Medium Logic**:

   * Prioritized in order: UTM → Click ID → Referrer → Stored → Direct/None
   * Referrer used if not excluded and not direct
   * Always lowercased
5. **Referral Exclusion**:

   * Excluded domains are ignored in referrer evaluation
6. **Last-Non Direct Logic**:

   * Only used if traffic is direct or excluded
   * Prevents unwanted overwrites from internal navigation
7. **Storage Mechanics**:

   * Single or separate cookie options supported
   * LocalStorage: `gtm_dd_traffic_data`
8. **Data Layer Push**:

   * Fires a structured event with `marketing_data`, `click_identifiers`, and `datalayer_source`

---

## 💼 Use Cases

### 📄 Lead Source Tracking

Capture source and medium into hidden form fields for lead attribution using cookie/localStorage values.

### 🔁 Offline Conversion Tracking

Send `gclid`, `fbclid`, and other identifiers to CRM/backend systems for offline event mapping.

### 📊 Funnel Attribution

Track consistent campaign attribution across the user journey using stored cookie values.

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
