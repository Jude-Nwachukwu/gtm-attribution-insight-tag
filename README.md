# Attribution Insight Tag

A powerful GTM custom **tag template** that captures traffic source data and ad click identifiers, stores them as browser cookies or localStorage values, and pushes them to the dataLayer. Perfect for marketing attribution, offline conversion tracking, and lead source capture.

---

## 🚀 What This Tag Does

The **Attribution Insight Tag** automatically collects UTM parameters and common ad click identifiers from your URL (e.g., `gclid`, `fbclid`, `msclkid`), infers missing data from the referrer or fallback logic, and stores these values:

* As a single or multiple cookies
* In browser localStorage
* In the dataLayer for use in GTM or GA4

---

## 🔧 Features

* Captures key traffic and campaign data: `utm_source`, `utm_medium`, `utm_campaign`, `utm_term`, `utm_content`, and `utm_id`
* Supports major ad click identifiers (e.g., `gclid`, `fbclid`, `ttclid`, etc.)
* Optionally stores data in:

  * A single JSON cookie
  * Separate cookies for each data point
  * localStorage
* Pushes data to `dataLayer` using a default or custom event name
* Built-in fallback logic for traffic source and medium
* Smart referrer handling with optional referral exclusion list
* Configurable cookie lifespan and domain/path
* Avoids overwriting cookies with undefined or empty values
* Respects last non-direct click model when enabled

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

Use the following field settings when configuring the tag:

### 🔹 Cookie Settings

* **Cookie Name**: (e.g., `utm_tracker`) – Sets the name for the cookie when not using separate cookies.
* **Cookie Duration** & **Lifespan**: Define how long cookies persist (e.g., `30 days`).

### 🔹 Traffic Data Options

Enable checkboxes for any traffic parameters you'd like to capture:

* **Traffic Source** (`utm_source`)
* **Traffic Medium** (`utm_medium`)
* **Campaign** (`utm_campaign`)
* **Campaign ID** (`utm_id`)
* **Term**, **Content** – Additional UTM fields

### 🔹 Ad Click Identifiers

Enable checkboxes to capture:

* `gclid`, `fbclid`, `ttclid`, `li_fat_id`, `msclkid`, etc.

### 🔹 Behavior Options

* **Use Last-Non Direct**: Retrieves previous values for source/medium if traffic appears to be direct.
* **Refresh Lifespan on Each Execution**: Resets cookie lifespan each time tag is fired.
* **Skip Setting Cookie on Empty**: Avoid setting cookies when no valid value exists.
* **Use Separate Cookies**: Store each data point in its own cookie.
* **Use Custom Prefix**: Define a prefix for separate cookies (e.g., `lead_`).

### 🔹 Local Storage and Data Layer

* **Store in LocalStorage**: Also persist values in localStorage under key `gtm_dd_traffic_data`.
* **Disable Data Layer Push**: Prevents automatic `dataLayer` push.
* **Use Custom Event Name**: Overrides the default `gtm_dd_marketing_traffic_data` event name.

### 🔹 Referral Exclusion

* **Enable Referral Exclusion**: Enable filtering of internal domains (e.g., `stripe.com, paypal.com`).

### 🔹 Domain & Path

* Set custom domain or path if needed. Defaults to top-level domain and root path.

---

## 🧠 Logic Overview

The tag evaluates data as follows:

1. **UTM Parameters First**: Captures UTM values from the URL (case-insensitive).
2. **Click Identifiers**: Collects known ad click IDs (`gclid`, `fbclid`, etc.) from the URL.
3. **Fallback for Source**:

   * If `utm_source` is missing, checks `document.referrer`.
   * If still missing, infers from known click identifiers (e.g., `gclid` → `google`).
   * If no indicators, returns `direct`.
4. **Fallback for Medium**:

   * If `utm_medium` is missing, returns `referral` or `none`, unless click identifier indicates `cpc`.
5. **Referral Exclusion**: Domains listed in the exclusion list are ignored as referrers.
6. **Last-Non Direct Model**: If direct traffic is detected, and fallback is enabled, retrieves previous values from cookies or localStorage.
7. **Storage**:

   * Writes data to a single JSON cookie or separate cookies.
   * Writes to localStorage if enabled.
8. **dataLayer Push**: Sends all collected data as a structured object to the dataLayer.

---

## 💼 Use Cases

### 📄 Lead Source Tracking

Capture traffic source and medium at form submission time by retrieving values from cookies using hidden form fields.

### 🔁 Offline Conversion Tracking

Send `gclid`, `fbclid`, etc. to your backend system for offline attribution via CRM or server-side processing.

### 📊 Attribution Consistency

Store and reuse consistent traffic attribution across the funnel using cookies and localStorage.

---

## 📄 License

Apache License 2.0

---

## 👨‍💻 Developed by

Template built by **Jude** for [DumbData](https://dumbdata.co/)

### 👉 Get Support or Raise an Issue

* Contact DumbData: [https://dumbdata.co/contact-us/](https://dumbdata.co/contact-us/)
* Explore more templates: [https://dumbdata.co/gtm-custom-templates/](https://dumbdata.co/gtm-custom-templates/)
* Found a bug or feature request? [Raise an issue here](#)
