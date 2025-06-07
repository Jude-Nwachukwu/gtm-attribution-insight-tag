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
* Configurable cookie strategy: single cookie or separate cookies with optional prefix
* Smart case-insensitive matching of query parameters
* Stores values in:

  * A single JSON cookie
  * Separate cookies for each data point
  * localStorage
* Pushes data to `dataLayer` using a default or custom event name
* Avoids overwriting cookies with undefined or empty values
* Referral exclusion support
* Respects last non-direct click model when enabled
* Fully customizable cookie domain/path and lifespan

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

* **Cookie Name**: (used when single cookie is selected)
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

* `gclid`, `fbclid`, `ttclid`, `li_fat_id`, `msclkid`, etc. — case-insensitive matching

### 🔹 Behavior Options

* **Use Last-Non Direct**: Enables last-non-direct model for source and medium
* **Refresh Lifespan on Each Execution**: Resets cookie lifespan each time tag is fired

### 🔹 Local Storage and Data Layer

* **Store in LocalStorage**: Also persist values in localStorage under key `gtm_dd_traffic_data`
* **Disable Data Layer Push**: Prevents automatic `dataLayer` push
* **Use Custom Event Name**: Overrides the default `gtm_dd_marketing_traffic_data` event name

### 🔹 Referral Exclusion

* **Enable Referral Exclusion**: Filter internal domains from being set as source
* **Exclusion Domains**: Add comma-separated domains (e.g., `stripe.com, paypal.com`)

### 🔹 Domain & Path

* Set custom domain or path if needed. Defaults to top-level domain and root path

---

## 🧠 Logic Overview

1. **Case-Insensitive Query Parsing**: Extracts UTM parameters and click identifiers from the URL regardless of case
2. **UTM Parameter Evaluation**:

   * Uses query parameter if present
   * Otherwise falls back to referrer or predefined click identifiers
3. **Click Identifier Evaluation**:

   * Stored if present in URL
   * Not overwritten if not found on subsequent pages
4. **Source & Medium Logic**:

   * `utm_source` and `utm_medium` are prioritized
   * If absent, source is inferred from known identifiers (e.g., `gclid` = `google`)
   * Medium inferred as `cpc` for known sources
   * Source and medium are always lowercased
5. **Referral Exclusion**:

   * If enabled, domains in the exclusion list are not considered as referrers
6. **Last-Non Direct Fallback**:

   * Source/medium fallback to cookie/localStorage only if traffic appears direct
   * Applies also when UTM or click identifiers are missing
7. **Storage Mechanics**:

   * Cookies: either a single JSON object or individual cookies per field
   * LocalStorage: saved as `gtm_dd_traffic_data` object
8. **Data Layer Push**:

   * Sent as structured object with `marketing_data`, `click_identifiers`, and metadata

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
