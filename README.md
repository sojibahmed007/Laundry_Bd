# 🧺 LaundryBD – QA Audit & Defect Tracking

[![Total Verified](https://img.shields.io/badge/Verified%20Cards-41%20in%20Done-success?style=for-the-badge&logo=trello)](./CARDS_DONE.md)
[![Critical & High](https://img.shields.io/badge/Critical%20%26%20High-21-red?style=for-the-badge)](./CARDS_DONE.md)
[![Medium](https://img.shields.io/badge/Medium-12-yellow?style=for-the-badge)](./CARDS_DONE.md)
[![Low](https://img.shields.io/badge/Low-8-green?style=for-the-badge)](./CARDS_DONE.md)
[![Environment](https://img.shields.io/badge/Scope-Admin%20%26%20Storefront-blue?style=for-the-badge)](https://admin.laundrybd.com)

---

## 📌 Project Overview & Description

This repository documents the comprehensive full-stack **Quality Assurance (QA) & Security Audit** conducted on **LaundryBD** — a premier on-demand laundry and dry-cleaning web platform operating in Sylhet, Bangladesh.

The audit covered both the **Customer-Facing Storefront** (`laundrybd.com`) and the **Administrative Management Portal** (`admin.laundrybd.com`), targeting business logic accuracy, financial calculation integrity, Role-Based Access Control (RBAC), and responsive mobile user experience.

All identified items were systematically reproduced, logged, and verified in the **"Done"** list on the official project management board.

* **Target Platforms:** [admin.laundrybd.com](https://admin.laundrybd.com) & [laundrybd.com](https://laundrybd.com)
* **Project Management Board:** [Kerno Studio – LaundryBD Trello Board](https://trello.com/b/zpnZp2pZ/kernostudio)
* **Lead QA Engineer:** [sojibahmed007](https://github.com/sojibahmed007)
* **Complete Cards List:** See [CARDS_DONE.md](./CARDS_DONE.md)

---

## 📊 Executive KPI Dashboard

```
┌────────────────────────────────────────────────────────┬─────────────┬──────────────┐
│ KPI Metric Category                                    │ Count       │ Ratio / %    │
├────────────────────────────────────────────────────────┼─────────────┼──────────────┤
│ Total Cards Logged & Verified (Done List)              │ 41 Items    │ 100.0%       │
│ ├── Confirmed Functional / UI / Security Bugs [BUG]    │ 34 Items    │ 82.9%        │
│ └── Strategic Feature & Architecture Improvements [IMP]│ 7 Items     │ 17.1%        │
├────────────────────────────────────────────────────────┼─────────────┼──────────────┤
│ Severity Breakdown:                                    │             │              │
│ ├── 🔴 Critical & High (Security / Billing / Integrity)│ 21 Items    │ 51.2%        │
│ ├── 🟡 Medium (Layout / Session / Error State)         │ 12 Items    │ 29.3%        │
│ └── 🟢 Low (Visual Polish / Labels / Copy)             │ 8 Items     │ 19.5%        │
├────────────────────────────────────────────────────────┼─────────────┼──────────────┤
│ Scope Coverage:                                        │             │              │
│ ├── Admin Management Portal & BFF APIs                 │ 23 Items    │ 56.1%        │
│ └── Customer Storefront, Cart & Checkout Funnel        │ 18 Items    │ 43.9%        │
└────────────────────────────────────────────────────────┴─────────────┴──────────────┘
```

---

## 🎯 Key Audit Focus Areas

* **💰 Financial & Billing Accuracy:** Verified order total aggregations, package billing inclusions, coupon validation rules, and Cash on Delivery (COD) cash reconciliations.
* **🛡️ RBAC & Administrative Security:** Audited multi-role authorization (`SUPER_ADMIN`, `ADMIN`, `MANAGER`, `STAFF`, `CUSTOMER`), route guards, and API-level permission matrix enforcement.
* **⚙️ Business Settings & Data Integrity:** Tested validation schemas on business configurations, service area management, and installation-locked system constants.
* **📱 Responsive UX & Conversion Funnel:** Verified mobile viewport layouts, address input containment, chronological pickup slot scheduling, and checkout flow stability.

---

## 📋 Verified Cards Directory

For the complete numbered list of all 41 verified cards with titles and direct Trello links, please see:

👉 **[View All 41 Verified Cards (CARDS_DONE.md)](./CARDS_DONE.md)**
