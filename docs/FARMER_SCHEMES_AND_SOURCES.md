# 🏛️ Krishi Mitra — Official Government Schemes & Sources Guide

> **Audit Standard**: Verified against Official Government Portals (Central & State of Uttar Pradesh)  
> **Audited Date**: October 2026  
> **Data Privacy Policy**: Krishi Mitra **never** collects, stores, or transmits Aadhaar numbers, bank account numbers, or land ownership records. All transactional applications redirect to certified government domains (`.gov.in` / `.nic.in`).

---

## 1. Verified Official Government Portals & Repositories

Every link below was checked and verified against official government registries:

| Organization / Authority | Domain / Verified URL | Core Purpose |
| :--- | :--- | :--- |
| **PM-KISAN Central Portal** | [https://pmkisan.gov.in/](https://pmkisan.gov.in/) | Direct benefit income transfer beneficiary registry, e-KYC, and installment status |
| **Pradhan Mantri Fasal Bima Yojana (PMFBY)** | [https://pmfby.gov.in/](https://pmfby.gov.in/) | National crop insurance calculation, premium subsidy, and crop loss claims |
| **Ministry of Agriculture & Farmers Welfare** | [https://agriwelfare.gov.in/](https://agriwelfare.gov.in/) | Central agricultural policy, national mission guidelines, budget allocation |
| **Department of Agriculture, Uttar Pradesh** | [https://agriculture.up.gov.in/](https://agriculture.up.gov.in/) | UP State DBT portal, agricultural subsidies, equipment booking (Krishi Yantra) |
| **Indian Council of Agricultural Research (ICAR)** | [https://icar.org.in/](https://icar.org.in/) | Agronomic research, Krishi Vigyan Kendra (KVK) network, disease management bulletins |
| **Open Government Data Platform India** | [https://data.gov.in/](https://data.gov.in/) | Public AGMARKNET daily mandi price catalog and agricultural statistics |
| **Soil Health Card Portal** | [https://soilhealth.dac.gov.in/](https://soilhealth.dac.gov.in/) | National soil sample database, macro/micronutrient testing recommendations |
| **Agriculture Infrastructure Fund (AIF)** | [https://agriinfra.dac.gov.in/](https://agriinfra.dac.gov.in/) | Medium-long term debt financing facility for post-harvest management infrastructure |

---

## 2. In-Depth Audit of Priority Schemes

### A. Central Government Flagship Schemes

#### 1. PM-KISAN (Pradhan Mantri Kisan Samman Nidhi)
- **Responsible Ministry**: Ministry of Agriculture & Farmers Welfare, Government of India.
- **Official Portal**: [https://pmkisan.gov.in/](https://pmkisan.gov.in/)
- **Core Benefits**: Direct cash benefit of **₹6,000 per year**, paid in three equal installments of ₹2,000 directly into the farmer's bank account via Aadhaar-linked DBT.
- **Eligibility Criteria**: All landholding farmer families who have cultivable landholding in their names. Institutional landholders, serving/retired government employees, and income tax payers are excluded.
- **Required Documents**: Aadhaar Card, Land Records (*Khasra / Khatauni* / RoR), Bank Passbook, Active Mobile Number linked to Aadhaar.
- **Mandatory Compliance**: Mandatory e-KYC (Aadhaar OTP or Biometric at CSC center).
- **Current Status**: **Active** (Nationwide implementation).
- **Krishi Mitra Status**: `[VERIFIED IMPLEMENTED]` (Cataloged in `src/data/schemes.js`).

#### 2. PMFBY (Pradhan Mantri Fasal Bima Yojana)
- **Responsible Ministry**: Ministry of Agriculture & Farmers Welfare, Government of India.
- **Official Portal**: [https://pmfby.gov.in/](https://pmfby.gov.in/)
- **Core Benefits**: Comprehensive risk insurance covering yield losses due to non-preventable natural risks (drought, flood, pests, unseasonal rain, landslides, post-harvest losses up to 14 days).
  - Premium paid by farmer: **2%** for Kharif crops, **1.5%** for Rabi crops, **5%** for commercial/horticultural crops. Balance premium shared 50:50 between Central and State governments.
- **Eligibility**: All farmers (loanee and non-loanee, sharecroppers, tenant farmers) growing notified crops in notified areas.
- **Required Documents**: Land Possession Certificate / Sowing Certificate, Aadhaar Card, Bank Account Details with IFSC, Land Revenue Receipt.
- **Current Status**: **Active** (Enrollment opens seasonally before cut-off dates).
- **Krishi Mitra Status**: `[VERIFIED IMPLEMENTED]` (Cataloged in `src/data/schemes.js`).

#### 3. KCC (Kisan Credit Card Scheme)
- **Responsible Authority**: Reserve Bank of India (RBI) & NABARD.
- **Official Portal**: [https://www.rbi.org.in/](https://www.rbi.org.in/) / Commercial Banks.
- **Core Benefits**: Revolving cash credit up to **₹3,00,000** at a concessional interest rate of **7% per annum**, reduced to **4% per annum** for farmers who ensure prompt repayment (due to 3% prompt repayment incentive). Covers crop cultivation expenses, post-harvest costs, and maintenance of farm assets.
- **Eligibility**: Individual farmers, joint borrowers, tenant farmers, oral lessees, sharecroppers, and Self Help Groups (SHGs).
- **Required Documents**: Application form, Identity & Address Proof (Aadhaar/Voter ID), Land Ownership / Tenancy proof, Passport photograph.
- **Current Status**: **Active** (Continuous enrollment via commercial banks, RRBs, and Cooperative banks).
- **Krishi Mitra Status**: `[VERIFIED IMPLEMENTED]` (Cataloged in `src/data/schemes.js`).

#### 4. Soil Health Card Scheme
- **Responsible Ministry**: Department of Agriculture & Farmers Welfare, Ministry of Agriculture.
- **Official Portal**: [https://soilhealth.dac.gov.in/](https://soilhealth.dac.gov.in/)
- **Core Benefits**: Free periodic testing of farmer soil samples across 12 parameters: Macro-nutrients (N, P, K), Secondary nutrients (S), Micro-nutrients (Zn, Fe, Cu, Mn, Bo), and Physical parameters (pH, EC, Organic Carbon). Issues a printed advisory report recommending customized fertilizer dosage.
- **Eligibility**: All farmers across all states.
- **Required Documents**: Farm plot GPS coordinate identification, sample collection form from local Agriculture Department / KVK.
- **Current Status**: **Active**.
- **Krishi Mitra Status**: `[PLANNED]` (Inputs are aligned with Krishi Mitra Model 2 crop recommender features; portal link recommended for inclusion).

#### 5. PM Krishi Sinchayee Yojana (PMKSY — Per Drop More Crop)
- **Responsible Ministry**: Department of Agriculture & Farmers Welfare.
- **Official Portal**: [https://pmksy.gov.in/](https://pmksy.gov.in/)
- **Core Benefits**: Financial subsidy of **45% to 55%** of benchmark cost for installing Micro-Irrigation systems (Drip and Sprinkler irrigation), significantly conserving water and boosting fertilizer efficiency.
- **Eligibility**: All landholders; priority to small and marginal farmers.
- **Required Documents**: Land Ownership Records, Aadhaar Card, Water Source Certificate, Soil/Water Test Report.
- **Current Status**: **Active**.
- **Krishi Mitra Status**: `[PLANNED]` (Recommended for inclusion in `schemes.js`).

---

### B. Uttar Pradesh State Priority Schemes

The state of Uttar Pradesh operates several high-impact DBT subsidy programs administered through the [UP Agriculture Portal](https://agriculture.up.gov.in/). These represent immediate integration candidates for Krishi Mitra:

#### 1. UP Krishi Yantra Subsidy Yojana (Farm Machinery Subsidy)
- **Responsible Department**: Department of Agriculture, Government of Uttar Pradesh.
- **Official Portal**: [https://agriculture.up.gov.in/](https://agriculture.up.gov.in/) (Sub-Mission on Agricultural Mechanization / SMAM).
- **Core Benefits**: **40% to 50% subsidy** (up to specified caps) on agricultural implements including Tractors, Laser Land Levelers, Rotavators, Happy Seeders, Multi-Crop Threshers, and Power Tillers.
- **Application Process**: Online token generation via `agriculture.up.gov.in`. Selection conducted via transparent lottery/first-come-first-served basis.
- **Eligibility**: Registered farmers on UP Agriculture DBT portal with valid farmer ID.
- **Required Documents**: UP Agriculture Registration ID, Land Khatauni, Aadhaar Card, Bank Passbook, Security Deposit (e-Challan).
- **Current Status**: **Active** (Announced in seasonal notification windows).
- **Krishi Mitra Status**: `[PLANNED FOR UP PILOT]`.

#### 2. UP Khet Talab Yojana (Farm Pond Subsidy)
- **Responsible Department**: Department of Agriculture, Uttar Pradesh.
- **Official Portal**: [https://agriculture.up.gov.in/](https://agriculture.up.gov.in/)
- **Core Benefits**: Up to **50% subsidy (₹52,500 to ₹1,14,000)** for digging small or medium farm ponds (*khet talab*) on private farmland for rainwater harvesting and emergency irrigation.
- **Priority Region**: High priority allocation for the **Bundelkhand region (including Jalaun, Jhansi, Hamirpur, Mahoba, Banda, Chitrakoot, Lalitpur)**.
- **Eligibility**: Farmers possessing at least 0.5 hectare cultivable land in notified water-stressed blocks.
- **Required Documents**: Land Ownership papers, Aadhaar Card, Bank Account, Undertaking for pond maintenance.
- **Current Status**: **Active**.
- **Krishi Mitra Status**: `[PLANNED FOR UP PILOT]`.

#### 3. UP PM-KUSUM Component-B (Solar Pump Yojana)
- **Responsible Department**: Department of Agriculture & UPNEDA, Uttar Pradesh.
- **Official Portal**: [https://agriculture.up.gov.in/](https://agriculture.up.gov.in/) / [https://upneda.org.in/](https://upneda.org.in/)
- **Core Benefits**: Up to **60% financial assistance** (30% Central + 30% State) for installing 2 HP, 3 HP, 5 HP, 7.5 HP, and 10 HP Standalone Surface or Submersible DC/AC Solar Pumps. Farmer contribution is 40%.
- **Eligibility**: Farmers having cultivable land with borewell/open water source without existing electric connection for irrigation.
- **Required Documents**: UP Agriculture registration number, land documents, Aadhaar, proof of borewell.
- **Current Status**: **Active**.
- **Krishi Mitra Status**: `[PLANNED FOR UP PILOT]`.

#### 4. UP Certified Seed Subsidy (Beej Anudan Yojana)
- **Responsible Department**: Uttar Pradesh Agriculture Department.
- **Official Portal**: [https://agriculture.up.gov.in/](https://agriculture.up.gov.in/)
- **Core Benefits**: Up to **50% subsidy** credited directly via DBT to the farmer's bank account upon purchasing certified high-yielding variety (HYV) seeds (Wheat, Paddy, Mustard, Pulses) from Government Seed Stores (*Beej Bhandar*).
- **Eligibility**: All farmers purchasing from authorized government seed centers in UP.
- **Required Documents**: Farmer Registration Slip, Seed Purchase Invoice, Aadhaar-linked Bank Account.
- **Current Status**: **Active**.
- **Krishi Mitra Status**: `[PLANNED FOR UP PILOT]`.

---

## 3. Scheme Status Summary & Current Codebase Audit

| Scheme Name | Jurisdiction | Category | Benefit Summary | Listed in `src/data/schemes.js` | Verification Status | Official Portal Verified |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **PM-KISAN** | Central | Income | ₹6,000/year (3 installments) | **Yes** (`id: "pm-kisan"`) | `[VERIFIED IMPLEMENTED]` | [https://pmkisan.gov.in/](https://pmkisan.gov.in/) |
| **PMFBY** | Central | Insurance | Full crop loss protection | **Yes** (`id: "pmfby"`) | `[VERIFIED IMPLEMENTED]` | [https://pmfby.gov.in/](https://pmfby.gov.in/) |
| **Kisan Credit Card (KCC)** | Central | Credit | Up to ₹3 Lakh loan at 4% | **Yes** (`id: "kcc"`) | `[VERIFIED IMPLEMENTED]` | [https://rbi.org.in/](https://rbi.org.in/) |
| **PKVY (Paramparagat Krishi)** | Central | Organic | ₹50,000/ha over 3 years | **Yes** (`id: "pkvy"`) | `[VERIFIED IMPLEMENTED]` | [https://agricoop.nic.in/](https://agricoop.nic.in/) |
| **MGNREGS (Agriculture)** | Central | Employment | 100 days guaranteed wage labor | **Yes** (`id: "mgnregs-agri"`) | `[VERIFIED IMPLEMENTED]` | [https://nrega.nic.in/](https://nrega.nic.in/) |
| **RKVY (Rashtriya Krishi Vikas)** | Central | Infrastructure | State-specific equipment/storage | **Yes** (`id: "rkvy"`) | `[VERIFIED IMPLEMENTED]` | [https://rkvy.nic.in/](https://rkvy.nic.in/) |
| **Shetkari Sanman Yojana** | Maharashtra | Income | ₹12,000/year additional support | **Yes** (`id: "maha-shetkari"`) | `[VERIFIED IMPLEMENTED]` | [https://krishi.maharashtra.gov.in/](https://krishi.maharashtra.gov.in/) |
| **Raitha Siri** | Karnataka | Income | ₹4,000/year state support | **Yes** (`id: "karnataka-ryothaseva"`) | `[VERIFIED IMPLEMENTED]` | [https://raitamitra.karnataka.gov.in/](https://raitamitra.karnataka.gov.in/) |
| **KALIA Scheme** | Odisha | Income | ₹10,000/year + insurance | **Yes** (`id: "kalia-odisha"`) | `[VERIFIED IMPLEMENTED]` | [https://kalia.odisha.gov.in/](https://kalia.odisha.gov.in/) |
| **Rythu Bandhu** | Telangana | Investment | ₹10,000/acre/year | **Yes** (`id: "rythu-bandhu"`) | `[VERIFIED IMPLEMENTED]` | [https://rythubandhu.telangana.gov.in/](https://rythubandhu.telangana.gov.in/) |
| **UP Krishi Yantra Subsidy** | Uttar Pradesh | Machinery | 40-50% subsidy on equipment | **No** (Integration candidate) | `[PLANNED]` | [https://agriculture.up.gov.in/](https://agriculture.up.gov.in/) |
| **UP Khet Talab (Farm Pond)** | Uttar Pradesh | Irrigation | Up to 50% subsidy (Bundelkhand) | **No** (Integration candidate) | `[PLANNED]` | [https://agriculture.up.gov.in/](https://agriculture.up.gov.in/) |
| **UP Solar Pump (PM-KUSUM)** | Uttar Pradesh | Solar Power | Up to 60% subsidy on solar pump | **No** (Integration candidate) | `[PLANNED]` | [https://agriculture.up.gov.in/](https://agriculture.up.gov.in/) |

---

## 4. Sensitive Data Protection & Regulatory Architecture

In compliance with the **Digital Personal Data Protection Act (DPDP Act, 2023)** and Reserve Bank of India (RBI) cybersecurity guidelines:

1. **Zero Collection of Sensitive Identifiers**: Krishi Mitra explicitly **never** prompts for or stores Aadhaar Numbers, Bank Account Numbers, Bank Passbook scans, or Land Khatauni documents on its servers or databases.
2. **Advisory Discovery Boundary**: Krishi Mitra acts solely as a discovery and eligibility-checking search engine.
3. **Outbound Portal Handoff**: Once a farmer discovers an eligible scheme and reviews documentation requirements, clicking the **"Apply on Official Portal"** button directly opens the certified government domain (e.g. `pmkisan.gov.in` or `agriculture.up.gov.in`).
4. **Third-Party Identity Verification**: Identity authentication, biometric fingerprint matching, and Aadhaar OTP verification are strictly performed inside the government portal environment or through a village Common Service Centre (CSC).
