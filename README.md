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

   * UTM parameters are handled without regard to casing

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
