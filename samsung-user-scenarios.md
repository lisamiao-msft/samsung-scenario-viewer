# Samsung User Retention Scenarios

## Background
Due to Samsung ending their relationship with OneDrive, we have initiated efforts to capture and retain Samsung users before natural churn occurs.

---

## Ecosystem Overview

### 1. Pre-installed OneDrive
- All Samsung device users had OneDrive auto-installed during the contract period
- OneDrive was installed in a protected OS partition and **could not be uninstalled**

### 2. Account Linking & Samsung Gallery Sync
- Users could opt into **Account Linking** to connect their OneDrive account to Samsung Gallery
- Enabled **Samsung Gallery Sync** to sync local photos to OneDrive
- Typical behavior: Samsung Gallery as primary photo management app, with OneDrive providing cross-platform access (web, desktop)

### 3. OneDrive as Primary Photo App
- Users could choose OneDrive as their primary photo management app
- Functionally identical to any other Android user (e.g., Pixel) using OneDrive for photos

### 4. OneDrive as Default PDF Viewer
- Users could set OneDrive as the default PDF viewer on Samsung devices
- **~76M users/month** (Data Science estimate)
- 70% Consumer / 30% Enterprise accounts
- **No sign-in required** to view PDFs

### 5. Default State & Activation Requirements
By default, users are **not signed into OneDrive** despite pre-installation.

To fully use OneDrive as a photo management app, users must:
1. Grant permission to access local photos & files
2. Allow OneDrive to send notifications
3. Sign into their consumer OneDrive account
4. Enable **Camera Roll Backup** for automatic photo sync to cloud

---

## User Segmentation Matrix

### Condition Key
| Abbreviation | Meaning |
|--------------|---------|
| **PDF** | PDF Viewer user |
| **PFA** | Permission: Photos & File Access granted |
| **NP** | Permission: Notifications allowed |
| **SGS** | Samsung Gallery Sync enabled |
| **SI** | Signed In |
| **CRB** | Camera Roll Backup enabled |
| **Paid** | Paid/subscription user |
| **OQ** | Over Quota (storage limit exceeded) |

### Logical Dependencies
- **SGS does NOT require SI** - Account linking happens at the Samsung Gallery level, independent of OneDrive sign-in. Users can have SGS enabled without being signed into OneDrive.
- CRB requires SI + PFA (can't backup without signing in and photo access)
- Paid requires SI (can't have subscription without account)
- OQ requires SI (can't be over quota without an account)
- PDF has no dependencies (works without sign-in)
- PFA and NP are independent of sign-in (can be granted before or after)

### Reachability by Permission
| NP Status | Available Channels |
|-----------|-------------------|
| ✅ NP granted | Push, In-App Toast, In-App Banner, Modal, Email |
| ❌ NP denied | In-App Banner (when app open), Modal (when app open), Email |

### Matrix Legend
- ✅ = Enabled/Active
- ❌ = Not enabled
- ✖️ = Not applicable (user no longer on Samsung device)
- ⚪ = Any value (varies per user)

---

## User Scenarios & Funnel Workflows

| # | PDF | PFA | NP | SGS | SI | CRB | Paid | OQ | Segment Name | Funnel Goal | Notification Sequence | What's Already Been Done (either in beta or prod) | What's Left to Do |
|---|:---:|:---:|:--:|:---:|:--:|:---:|:----:|:--:|--------------|-------------|----------------------|--------------------------|-------------------|
| 0 | ✖️ | ✖️ | ✖️ | ✅ | ✖️ | ✖️ | ✖️ | ✖️ | Former SGS (Switched Device) | Re-engage via email | Email only | | Service mail scheduled for 2/27|
| 1a | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | Dormant Install (No Permissions) | Grant permissions → Sign in | In-app only | SSO experience with other MSA accounts on device; Improved Sign-in page mockups (1a-banner-1-planned_1-1, 1a-banner-1-planned_1-2) | |
| 1b | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | Dormant Install (NP Only) | Sign in | Push, In-app | Push notifications to users on OneDrive value prop. Encourage user to sign in; Improved Sign-in page mockups (1a-banner-1-planned_1-1, 1a-banner-1-planned_1-2) | |
| 1c | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | Dormant Install (PFA Only) | Grant NP → Sign in | In-app only | Turned on local MOJs for OTD and optimized sign-in flow by adding 1 more step to the disable flow; Improved Sign-in page mockups (1a-banner-1-planned_1-1, 1a-banner-1-planned_1-2) | |
| 1d | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | Dormant Install (Both Permissions) | Sign in | Push, In-app | Local MOJ notifications increased in frequency; Push notifications to users on OneDrive value prop and encourage users to sign in; Improved Sign-in page mockups (1a-banner-1-planned_1-1, 1a-banner-1-planned_1-2) | |
| 2a | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | PDF-Only (No Permissions) | Grant permissions → Sign in | In-app only | | "Medium"-like banner + sign-in flow |
| 2b | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | PDF-Only (NP Only) | Sign in | Push, In-app | Same as 1b | Same as 1b |
| 2c | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | PDF-Only (PFA Only) | Grant NP → Sign in | In-app only | Same as 1c | Same as 1c |
| 2d | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | PDF-Only (Both Permissions) | Sign in | Push, In-app | Same as 1d | Same as 1d |
| 3a | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | Signed-In Passive (No Permissions) | Grant PFA → Enable CRB | In-app only | Discounted annual SKU | |
| 3b | ❌ | ❌ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | Signed-In Passive (NP Only) | Grant PFA → Enable CRB | Push, In-app | Discounted annual SKU, Will see People / Florence discovery once PFA granted | Push and in-app notifications to encourage user to give photos access & turn on CRB |
| 3c | ❌ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | Signed-In Passive (PFA Only) | Enable CRB | In-app, Email | OTD MOJs, Will see People / Florence discovery once enabled on thier account, Discounted Annual SKU | In-app banner that CRB is off. Local MOJs |
| 3d | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | Signed-In Passive (Both Permissions) | Enable CRB | Push, In-app, Email | Same as 3c | AI MOJs, Custom AI notifications |
| 4a | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | Signed-In PDF (PFA Only) | Enable CRB | In-app, Email | Same as 3c | Same as 3c |
| 4b | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | Signed-In PDF (Both Permissions) | Enable CRB | Push, In-app, Email | Same as 3d | Same as 3d |
| SGS-0a | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | SGS Not Signed In (No Permissions) | Grant PFA → Sign in → Enable CRB | In-app only | | PFA permission request for SGS users |
| SGS-0b | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | SGS Not Signed In (NP Only) | Grant PFA → Sign in → Enable CRB | Push, In-app | | Push notification about SGS migration; PFA permission request |
| SGS-0c | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | SGS + PDF Not Signed In (No Permissions) | Grant PFA → Sign in → Enable CRB | In-app only | Same as SGS-0a | Same as SGS-0a |
| SGS-0d | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | SGS + PDF Not Signed In (NP Only) | Grant PFA → Sign in → Enable CRB | Push, In-app | Same as SGS-0b | Same as SGS-0b |
| SGS-1a | ❌ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | SGS Not Signed In (PFA Only) | Sign in → Enable CRB | In-app only | | SGS migration banner directing users to sign into OneDrive |
| SGS-1b | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | SGS Not Signed In (Both Permissions) | Sign in → Enable CRB | Push, In-app | | Push notifications about SGS migration; In-app banner to sign in |
| SGS-2a | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | SGS + PDF Not Signed In (PFA Only) | Sign in → Enable CRB | In-app only | Same as SGS-1a | Same as SGS-1a |
| SGS-2b | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | SGS + PDF Not Signed In (Both Permissions) | Sign in → Enable CRB | Push, In-app | Same as SGS-1b | Same as SGS-1b |
| 5c | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | Gallery Sync Signed In (No Permissions) | Grant PFA → Enable CRB | In-app, Email | | PFA permission request for signed-in SGS users |
| 5d | ❌ | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | Gallery Sync Signed In (NP Only) | Grant PFA → Enable CRB | Push, In-app, Email | Same as 5c | Same as 5c |
| 5a | ❌ | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | Gallery Sync Only (PFA Only) | Enable CRB | In-app, Email | Discounted annual SKU | SGS banner to forward users to OneDrive. OneDrive will ask for the requisite permissions and auto enable CRB for these users |
| 5b | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | Gallery Sync Only (Both Permissions) | Enable CRB | Push, In-app, Email | Same as 5a | Same as 5a |
| 6c | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | Gallery Sync + PDF Signed In (No Permissions) | Grant PFA → Enable CRB | In-app, Email | Same as 5c | Same as 5c |
| 6d | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | Gallery Sync + PDF Signed In (NP Only) | Grant PFA → Enable CRB | Push, In-app, Email | Same as 5c | Same as 5c |
| 6a | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | Gallery Sync + PDF (PFA Only) | Enable CRB | In-app, Email | Same as 5a | Same as 5a |
| 6b | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | Gallery Sync + PDF (Both Permissions) | Enable CRB | Push, In-app, Email | Same as 5a | Same as 5a |
| 7a | ❌ | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ | CRB Free (No NP) | Grant NP → Upsell | In-app, Email | Discounted annual SKU, OTD MOJs, Will see People / Florence discovery once enabled on thier account | AI MOJs, Custom AI notifications |
| 7b | ❌ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | CRB Free (Full Permissions) | Upsell to Paid | Push, In-app, Email | Same as 7a | Same as 7a |
| 8a | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ | CRB + PDF Free (No NP) | Grant NP → Upsell | In-app, Email | Same as 7a | Same as 7a |
| 8b | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | CRB + PDF Free (Full Permissions) | Upsell to Paid | Push, In-app, Email | Same as 7a | Same as 7a |
| 9a | ❌ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | Full Sync Free (No NP) | Grant NP → Upsell | In-app, Email | Same as 7a | Same as 7a |
| 9b | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | Full Sync Free (Full Permissions) | Upsell to Paid | Push, In-app, Email | Same as 7a | Same as 7a |
| 10a | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | Power User Free (No NP) | Grant NP → Upsell | In-app, Email | Same as 7a | Same as 7a |
| 10b | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | Power User Free (Full Permissions) | Upsell to Paid | Push, In-app, Email | Same as 7a | Same as 7a |
| 11a | ❌ | ✅ | ❌ | ❌ | ✅ | ✅ | ✅ | ❌ | CRB Paid (No NP) | Grant NP → Retain | In-app, Email | Same as 7a | Same as 7a |
| 11b | ❌ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ | CRB Paid (Full Permissions) | Retain / Cross-sell | Push, In-app, Email |Same as 7a | Same as 7a |
| 12a | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ | ✅ | ❌ | CRB + PDF Paid (No NP) | Grant NP → Retain | In-app, Email |Same as 7a | Same as 7a |
| 12b | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ | CRB + PDF Paid (Full Permissions) | Retain / Cross-sell | Push, In-app, Email |Same as 7a | Same as 7a  |
| 13a | ❌ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | Full Sync Paid (No NP) | Grant NP → Retain | In-app, Email |Same as 7a | Same as 7a |
| 13b | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | Full Sync Paid (Full Permissions) | Retain | Push, In-app, Email |Same as 7a | Same as 7a |
| 14a | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | Power User Paid (No NP) | Grant NP → Retain | In-app, Email | Same as 7a | Same as 7a |
| 14b | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | Power User Paid (Full Permissions) | Retain | Push, In-app, Email |Same as 7a | Same as 7a |
| 15a | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ✅ | ❌ | Paid Inactive (No Permissions) | Grant PFA → CRB | In-app, Email | Same as 3a | Same as 3a |
| 15b | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | Paid Inactive (Full Permissions) | Enable CRB | Push, In-app, Email | Same as 3d | Same as 3d |
| 16a | ❌ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | Paid SGS, No CRB (No NP) | Enable CRB | In-app, Email | Same as 4a | Same as 4a |
| 16b | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | Paid SGS, No CRB (Full Permissions) | Enable CRB | Push, In-app, Email | Same as 3d | Same as 3d |
| OQa | ⚪ | ⚪ | ⚪ | ⚪ | ❌ | ⚪ | ⚪ | ✅ | Over Quota (Not Signed In) | Upsell to Paid | Email only | Overquota emails | |
| OQb | ⚪ | ⚪ | ⚪ | ⚪ | ✅ | ⚪ | ⚪ | ✅ | Over Quota (Signed In) | Upsell to Paid | Push, In-app, Email | Overquota emails | Overquota push notifications (OQb-notification-1-planned_1, OQb-notification-1-planned_2) |

---

## Notification Asset Types

| Type | Channel | Use Case |
|------|---------|----------|
| **Push Notification** | Mobile | Immediate engagement, time-sensitive |
| **In-App Toast** | Mobile | Contextual nudge during app use |
| **In-App Banner** | Mobile/Web | Persistent awareness |
| **Email** | Email | Detailed messaging, re-engagement |
| **Modal** | Mobile/Web | High-priority action required |

---

## Workflow Details by Segment

### Segment 0: Former SGS (Switched Device)
**Current State:** Was a Samsung Gallery Sync user, now on a different device (e.g., iPhone)
**Funnel Goal:** Re-engage — remind them of content in OneDrive
**Reachable via:** Email only (no push/in-app possible)

| Step | Asset Type | Trigger | Message Theme |
|------|------------|---------|---------------|
| 1 | Email | | |
| 2 | Email | | |
| 3 | Email | | |

---

### Segment 1: Dormant Install
**Current State:** OneDrive installed but never opened/signed in
**Funnel Goal:** Sign in

| Step | Asset Type | Trigger | Message Theme |
|------|------------|---------|---------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

---

### Segment 2: PDF-Only Anonymous
**Current State:** Uses OneDrive only for PDF viewing, not signed in
**Funnel Goal:** Sign in

| Step | Asset Type | Trigger | Message Theme |
|------|------------|---------|---------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

---

### Segment 3-6: Signed In, No CRB
**Current State:** Signed in but Camera Roll Backup not enabled
**Funnel Goal:** Enable CRB

| Step | Asset Type | Trigger | Message Theme |
|------|------------|---------|---------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

---

### Segment 7-10: CRB Enabled, Free
**Current State:** Actively backing up photos, on free tier
**Funnel Goal:** Upsell to Paid

| Step | Asset Type | Trigger | Message Theme |
|------|------------|---------|---------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

---

### Segment 11-14: Paid Users
**Current State:** Subscribed users
**Funnel Goal:** Retain, reduce churn risk

| Step | Asset Type | Trigger | Message Theme |
|------|------------|---------|---------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

---

## Notes
- Segments can be further refined based on storage usage, recency of activity, device age, etc.
- Samsung partnership end date will trigger urgency messaging for certain segments
