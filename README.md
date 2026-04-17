# Forensic Analysis: Dissecting a "Fake AI Computing" Scam Platform

## 📌 Overview
This repository contains a technical breakdown and forensic analysis of the platform `superxcomputer.com`. The website claims to offer high-performance AI computing services but was found to be a sophisticated **Task Scam/Ponzi scheme** targeting specific geographic regions, including Romania.

The analysis was performed in a controlled environment (Debian 12, Kali Linux VM) using professional security tools to document the platform's deceptive practices.

## 🛠 Tools Used
* **Burp Suite Professional**: Intercepting and analyzing API calls/JSON configurations.
* **Browser Developer Tools**: Dissecting the Vite/Vue.js frontend and inspecting `localStorage`.
* **CLI Forensics**: Scanning minified JavaScript files for hidden endpoints.
* **OSINT**: Identifying infrastructure providers (Cloudflare) and origin servers.

## 🔍 Key Findings

### 1. Geographical Targeting (Campaign Lock)
The platform utilizes a "Campaign Lock" mechanism. Through the `/api/v1/site/config` endpoint, the backend forces a default country code:
* **Default Code**: `+40` (Romania)
* **Behavior**: Attempting to register with other country codes (e.g., +64) was blocked at the API level, confirming a localized attack strategy.

### 2. Fake Business Logic
Analysis of the site's configuration JSON revealed hardcoded flags that override the UI's promises:
* **Withdrawal Restrictions**: While the UI displays Bank and Revolut withdrawal options, the backend flags `withdrawMethodBank` and `withdrawMethodRevolut` were explicitly set to `false`.
* **Artificial Delay**: The system includes hardcoded logic to prohibit withdrawals on weekends, a classic tactic to allow scammers to move funds before detection.

### 3. Identity & Origin
* **Template Source**: The minified JavaScript (`index-BDUMmj4H.js`) contains language keys for `zh-CN` as the primary origin, indicating a white-label scam template of Asian origin.
* **I18n Exploitation**: The platform contains translations for over 10 languages (RU, ES, HI, PT, etc.), suggesting a global operation template that can be toggled per campaign.

### 4. User Tracking & Fingerprinting
The platform implements aggressive device fingerprinting via the `_device_fp_v2` object in `localStorage`, collecting:
* Canvas Fingerprinting ID
* WebGL Vendor info (e.g., NVIDIA/GTX info)
* CPU Architecture and Core count
* Screen resolution

### 5. Client-Side Manipulation
By manipulating the `localStorage` key `lang`, the interface was successfully forced into a Russian locale (`ru`), bypassing the intended UI flow and proving the lack of server-side state validation for localized sessions.

## 📁 Evidence
The `/screenshots` directory contains documented proof of:
* API Response interceptions showing blocked withdrawal methods.
* The device fingerprinting data extracted from the console.
* Evidence of "Vibecoding" (Dead buttons and non-functional UI elements that exist only for visual deception).

## ⚖️ Disclaimer
This analysis is for **educational and research purposes only**. It was conducted to demonstrate security audit methodologies and to warn potential victims about fraudulent investment schemes.

---
**Author**: [Your GitHub Handle]  
**Field**: Cybernetics and Economic Informatics | Security Researcher
