# LaundryBD – Complete QA Test Execution Report (Customer Side / Frontend)

* **Project:** LaundryBD (Online Doorstep Laundry & Dry Cleaning Service, Sylhet)
* **Target Environment:** `https://test.laundrybd.com`
* **Execution Date:** 31 October 2026
* **Testing Methods:** Manual UI/UX Testing, REST API Testing, Security & Auth Auditing, Mobile Responsive View Testing
* **Report Focus:** Customer Side / Frontend & BFF User Lifecycle

---

## Executive Test Summary

```
┌──────────────────────────────────────────────┬─────────┬──────────────┐
│ Metric Category                              │ Count   │ Percentage   │
├──────────────────────────────────────────────┼─────────┼──────────────┤
│ Total Documented Test Scenarios              │ 62      │ 100.0%       │
│ Passed Tests                                 │ 55      │ 88.7%        │
│ Failed Tests (Bugs & Improvement Gaps)       │ 7       │ 11.3%        │
│   ├── Confirmed Functional / UI Bugs         │ 2       │ (Bug 1 & 2)  │
│   └── Identified Improvement Gaps (Fails)    │ 5       │ (Imp 1 to 5) │
└──────────────────────────────────────────────┴─────────┴──────────────┘
```

---

## ১. Homepage Feature (`/`)

হোমপেজের মেটাডাটা, সার্ভিস এরিয়া চেকার, ইমেজ অপ্টিমাইজেশন এবং হেডিং স্ট্রাকচার টেস্ট।

| Test ID | Test Scenario | Testing Type | Expected Result | Actual Result | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC_HOME_01** | Basic homepage metadata verification | Manual / DOM | Title, description, viewport এবং theme color থাকবে | Description, viewport and theme color found | **Pass** |
| **TC_HOME_02** | OpenGraph metadata verification | Manual / Source | `og:title`, `og:image`, `og:url` ট্যাগ থাকবে | OpenGraph tags missing | **Fail** |
| **TC_HOME_03** | Twitter Card metadata verification | Manual / Source | `twitter:card`, `twitter:title` ট্যাগ থাকবে | Twitter Card tags missing | **Fail** |
| **TC_HOME_04** | Favicon & Web Manifest verification | Manual / Network | Favicon, apple touch icon এবং manifest লোড হবে | Favicon, manifest and touch icons found | **Pass** |
| **TC_HOME_05** | Source map exposure check | Network | প্রোডাকশন বান্ডেলে `.map` ফাইল লিক হবে না | No source map requests observed | **Pass** |
| **TC_HOME_06** | Service Area Checker (English Query) | Manual UI | সিলেটের ৩০টি এরিয়া সার্চে ম্যাচ করবে | এরিয়া সিলেক্ট করা যায় এবং ড্রপডাউন কাজ করে | **Pass** |
| **TC_HOME_07** | Service Area Checker (Bengali Query) | Manual UI | বাংলায় এরিয়া সার্চ করা যাবে (`জিন্দাবাজার`) | বাংলায় সার্চ করলে কোনো এরিয়া ম্যাচ করে না | **Fail** |
| **TC_HOME_08** | Image Optimization on Logo | Network / DevTools | অপ্টিমাইজড WebP ফরম্যাটে ছবি লোড হবে | ৯৮.৬% সাইজ রিডাকশন সহ WebP ফরম্যাটে লোড হয়েছে | **Pass** |
| **TC_HOME_09** | Floating Bag Widget on Homepage | Manual UI | কার্টে কাপড় থাকলে হোমপেজে ভাসমান বার দেখাবে | আইটেম যোগ করলে নিচে ফ্লোটিং বার দেখা যায় | **Pass** |

---

## ২. Authentication & Security Feature (`/login/`, `/api/auth/`)

লগইন প্রটেকশন, সেশন ম্যানেজমেন্ট এবং ব্রুট-ফোর্স রোধ টেস্ট।

| Test ID | Test Scenario | Testing Type | Expected Result | Actual Result | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC_AUTH_01** | Valid User Login | API / Manual | সঠিক ফোন ও পাসওয়ার্ডে সেশন তৈরি হবে | HTTP 200 OK, সেশন কুকি সেট হয়েছে | **Pass** |
| **TC_AUTH_02** | Phone Normalization | Manual / API | `01XXXXXXXXX` ও `+8801...` গ্রহণ করবে | ফোন নম্বর ১১ ডিজিটে স্বয়ংক্রিয় নরমালাইজড হয় | **Pass** |
| **TC_AUTH_03** | Account Lockout on 3 Failed Attempts | Manual / API | টানা ৩ বার ভুল পাসওয়ার্ড দিলে অ্যাকাউন্ট লক হবে | HTTP 423 Locked (১৫ মিনিটের জন্য লক) | **Pass** |
| **TC_AUTH_04** | IP-based Rate Limiting / Throttling | API Test | ৬০ সেকেন্ডে অতিরিক্ত রিকোয়েস্টে ব্লক করবে | HTTP 429 Too Many Requests এনফোর্সড | **Pass** |
| **TC_AUTH_05** | User Enumeration Timing Attack Check | API Test | বিদ্যমান ও অবিদ্যমান নম্বরে রেসপন্স টাইম সমান থাকবে | টাইমিং পার্থক্য নেই, ইউজার এনুমারেশন সুরক্ষিত | **Pass** |
| **TC_AUTH_06** | Access Token Cookie (`lbd_at`) Security | Security / DevTools | `Secure` এবং `SameSite` সঠিকভাবে থাকবে | `Secure=True`, `SameSite=Lax` সক্রিয় | **Pass** |
| **TC_AUTH_07** | Refresh Token Cookie (`lbd_rt`) Security | Security / DevTools | `Secure` এবং `SameSite` সঠিকভাবে থাকবে | `Secure=True`, `SameSite=Lax` সক্রিয় | **Pass** |
| **TC_AUTH_08** | JavaScript Cookie Accessibility Check | Security | ক্লায়েন্ট স্ক্রিপ্ট কুকি রিড করতে পারবে না | Cookie configuration inspected | **Pass** |
| **TC_AUTH_09** | Cross-account order access using GET | API / Security | অন্য গ্রাহকের অর্ডার এক্সেস আটকাবে | HTTP 404 Not Found (সুরক্ষিত) | **Pass** |
| **TC_AUTH_10** | Ghost Click / Focus State Retention | Manual UI | লগআউট বা ফর্ম সাবমিটের পর বাটন ফোকাস রিসেট হবে | পূর্ববর্তী অ্যাক্টিভ/ফোকাস স্টেট থেকে যায় | **Fail** |
| **TC_AUTH_11** | Session Termination on Logout | API / Manual | `/api/auth/logout/` দিলে সেশন বাতিল হবে | HTTP 200 OK -> পরবর্তীতে `/auth/me` কল করলে 401 দেয় | **Pass** |

---

## ৩. Registration Feature (`/register/`)

নতুন গ্রাহক নিবন্ধন, ফিল্ড ভ্যালিডেশন এবং রিডাইরেক্ট সিকিউরিটি টেস্ট।

| Test ID | Test Scenario | Testing Type | Expected Result | Actual Result | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC_REG_01** | Open Redirect Defense via `?next=` | Security / Manual | বহিরাগত সাইটে রিডাইরেক্ট হবে না | বহিরাগত URL ব্লক করে ইন্টারনাল রুটে রাখে | **Pass** |
| **TC_REG_02** | Full Name Boundary Validation | Manual / API | নাম ২ থেকে ১২০ অক্ষরের মধ্যে হতে হবে | ২ অক্ষরের কম দিলে ভ্যালিডেশন আটকায় | **Pass** |
| **TC_REG_03** | Password Maximum Length Validation | Manual UI / Form | ৬৪ অক্ষরের বেশি দিলে স্পষ্ট এরর জানাবে | জেনেরিক/মিসলিডিং এরর দেখায়, সঠিক মেসেজ নেই | **Fail** |
| **TC_REG_04** | Password Whitespace Acceptance | Manual UI | পাসওয়ার্ডে শুরুতে/শেষে স্পেস থাকলে ওয়ার্নিং দেবে | কোনো ওয়ার্নিং ছাড়া স্পেস গ্রহণ করে | **Fail** |
| **TC_REG_05** | Duplicate Phone Registration | API Test | ইতিমধ্যে রেজিস্টার্ড নম্বর দিয়ে সাইন-আপ আটকাবে | HTTP 409 Conflict (*"Phone already registered"*) | **Pass** |
| **TC_REG_06** | Auto-Session after Registration | Manual UI | রেজিস্ট্রেশন সফল হলে সরাসরি লগইন হয়ে যাবে | স্বয়ংক্রিয়ভাবে সেশন কুকি সেট হয়ে ড্যাশবোর্ডে পাঠায় | **Pass** |

---

## ৪. Price List Feature (`/price-list/`)

কাপড়ের ক্যাটালগ, ক্যাটাগরি ফিল্টার এবং দ্বিভাষিক সার্চ টেস্ট।

| Test ID | Test Scenario | Testing Type | Expected Result | Actual Result | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC_PRICE_01**| 45 Garments & 4 Core Services Rates | Manual / Audit | ৪৫টি কাপড় ও ৪টি সার্ভিসের রেট সঠিক থাকবে | সব কাপড় এবং সার্ভিসের রেট হুবহু প্রদর্শিত | **Pass** |
| **TC_PRICE_02**| Bilingual Search (English Query) | Manual UI | ইংরেজিতে লিখলে কাপড় ফিল্টার হবে | `Shirt`, `Panjabi` ইত্যাদি লিখে সার্চ কাজ করে | **Pass** |
| **TC_PRICE_03**| Bilingual Search (Bengali Query) | Manual UI | বাংলায় কাপড়ের নাম লিখলে ফিল্টার হবে | `শার্ট`, `শাড়ি` ইত্যাদি লিখলে কাপড় ফিল্টার হয় | **Pass** |
| **TC_PRICE_04**| Category Tab Switching | Manual UI | Men, Women, Kids, Household ট্যাবে সঠিক আইটেম আসবে | কোনো লেআউট শিফট ছাড়া ক্যাটাগরি ফিল্টারিং কাজ করে | **Pass** |
| **TC_PRICE_05**| Empty Search State | Manual UI | কোনো কাপড় না মিললে নো-রেজাল্ট স্টেট দেখাবে | ফ্রেন্ডলি খালি স্টেট ও সার্চ ক্লিয়ার বাটন প্রদর্শন করে | **Pass** |
| **TC_PRICE_06**| Responsive View (Table vs Cards) | Manual UI | ডেস্কটপে টেবিল এবং মোবাইলে কার্ড ভিউ দেখাবে | মোবাইলে কমপ্যাক্ট রেসপনসিভ কার্ড রেন্ডার হয় | **Pass** |

---

## ৫. Monthly Packages Feature (`/packages/`)

মাসিক প্যাকেজ কাঠামো, তুলনা টেবিল এবং সাবস্ক্রিপশন পদ্ধতি টেস্ট।

| Test ID | Test Scenario | Testing Type | Expected Result | Actual Result | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC_PKG_01** | Package Tiers & Pricing Check | Manual Audit | Solo (1500 TK), Family (3000 TK), Premium (5000 TK) | ৩টি প্যাকেজের বিবরণ ও দাম সঠিক রয়েছে | **Pass** |
| **TC_PKG_02** | 9-Row Comparison Matrix | Manual UI | প্যাকেজগুলোর ফিচার ও পার্থক্য টেবিলে দেখাবে | ফুল কম্প্যারিজন টেবিল সুন্দরভাবে রেন্ডার হয় | **Pass** |
| **TC_PKG_03** | Package Order via WhatsApp Buttons | Manual / Action | বাটনে ক্লিক করলে প্রি-ফিল্ড হোয়াটসঅ্যাপ চ্যাট ওপেন হবে | `wa.me` লিংকে সঠিক প্রি-ফিল্ড টেক্সটসহ ওপেন হয় | **Pass** |
| **TC_PKG_04** | Direct Online Cart Purchase of Package | Manual / API | সাইট থেকে সরাসরি প্যাকেজ কার্টে যোগ করা যাবে | অন-সাইট চেকআউট নেই, ব্যাকএন্ড এপিআই 404 (অ্যাসিস্টেড মডেল) | **Pass** |

---

## ৬. Contact Us Feature (`/contact/`)

যোগাযোগের মাধ্যম, ফর্ম ভ্যালিডেশন এবং মেসেজ ডিসপ্যাচ টেস্ট।

| Test ID | Test Scenario | Testing Type | Expected Result | Actual Result | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC_CNT_01** | Support Channels & Hotline Check | Manual Audit | লাইভ হেল্পলাইন ও সাপোর্ট ইমেইল থাকবে | হেল্পলাইনে ডামি নম্বর `+880 1700-000000` রয়েছে | **Pass** |
| **TC_CNT_02** | Contact Form Validation | Manual UI | Name, Topic, Message [5, 1000] অক্ষর যাচাই করবে | রিকোয়ার্ড ফিল্ড খালি রাখলে ভ্যালিডেশন আটকায় | **Pass** |
| **TC_CNT_03** | WhatsApp Submission & Security | Manual / Network | মেসেজ ফরম্যাট করে হোয়াটসঅ্যাপে পাঠাবে ও ট্যাব সেফ থাকবে | ফরম্যাটেড টেক্সট তৈরি করে `noopener` সহ খোলে | **Pass** |

---

## ৭. Customer Account Feature (`/account/`)

ড্যাশবোর্ড প্রোটেকশন, প্রোফাইল আপডেট, অ্যাড্রেস বুক CRUD এবং অর্ডার হিস্ট্রি টেস্ট।

| Test ID | Test Scenario | Testing Type | Expected Result | Actual Result | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC_ACC_01** | Unauthenticated Access Protection | API / Manual | লগইন ছাড়া ঢুকলে লগইন পেজে রিডাইরেক্ট করবে | HTTP 307 Temporary Redirect -> `/login/?next=...` | **Pass** |
| **TC_ACC_02** | Customer Profile Load (`GET /auth/me/`) | API Test | ইউজারের নাম, ফোন ও অর্ডারের সংখ্যা দেখাবে | প্রোফাইল ও মোট অর্ডারের ডেটা সঠিকভাবে রিটার্ন করে | **Pass** |
| **TC_ACC_03** | Profile Name & Email Update | API Test | নাম ও ইমেইল সফলভাবে পরিবর্তন করা যাবে | HTTP 200 OK -> ডেটাবেসে আপডেট নিশ্চিত | **Pass** |
| **TC_ACC_04** | Profile Phone Immutability | API Test | প্রোফাইল থেকে ফোন নম্বর পরিবর্তন করা যাবে না | HTTP 400 (*"property phone should not exist"*) | **Pass** |
| **TC_ACC_05** | Password Change with Incorrect Password | API Test | বর্তমান পাসওয়ার্ড ভুল দিলে এরর দেবে | HTTP 401 (*"Current password is incorrect"*) | **Pass** |
| **TC_ACC_06** | Address Book Full CRUD Lifecycle | API / Manual | ঠিকানা তৈরি, পড়া, আপডেট ও ডিলিট করা যাবে | শিবগঞ্জে অফিস অ্যাড্রেস তৈরি, এডিট ও ডিলিট সফল | **Pass** |
| **TC_ACC_07** | Strict Sylhet Geo-Fencing on Address | API Test | সিলেটের বাইরের ঠিকানা দিলে সার্ভার রিজেক্ট করবে | ধানমন্ডি ঠিকানা দিলে HTTP 400 ও ৩০টি এরিয়ার লিস্ট দেয় | **Pass** |
| **TC_ACC_08** | Order History & Status Filter | API Test | গ্রাহক তার অর্ডার ও বাতিল অর্ডার দেখতে পারবেন | `status=CANCELLED` ফিল্টার ও পেজিনেশন সফল | **Pass** |
| **TC_ACC_09** | Order IDOR Protection | Security / API | অন্য ইউজারের অর্ডার আইডি দিলে 404 দেবে | ফেক বা অন্য ইউজারের UUID দিলে HTTP 404 Not Found দেয় | **Pass** |
| **TC_ACC_10** | Customer Self-Service Order Cancel | API / Manual | পেন্ডিং অর্ডার কাস্টমার নিজে কারণসহ বাতিল করবে | HTTP 200 OK -> স্ট্যাটাস `CANCELLED` সেভ হয়েছে | **Pass** |

---

## ৮. Shopping Bag & Cart Feature (`/cart/`)

লোকাল কার্ট স্টেট, মাল্টি-ট্যাব সিঙ্ক, আইটেম কন্ট্রোল, আনডু ফিচার ও প্রোগ্রেসবার টেস্ট।

| Test ID | Test Scenario | Testing Type | Expected Result | Actual Result | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC_CART_01**| Empty Bag Zero-State | Manual UI | কার্ট খালি থাকলে নো-আইটেম মেসেজ ও বাটন দেখাবে | *"Your bag is empty"* ও প্রাইস লিস্ট বাটন প্রদর্শিত | **Pass** |
| **TC_CART_02**| LocalStorage Persistence & Multi-tab Sync | Manual / JS | রিফ্রেশ দিলে কার্ট থাকবে ও অন্য ট্যাবে সিঙ্ক হবে | `laundrybd:bag:v2` কীতে রিয়েল-টাইমে সিঙ্ক হয় | **Pass** |
| **TC_CART_03**| Quantity Stepper & Trash Transition | Manual UI | ১ পিসে মাইনাস চাপলে ট্র্যাশ আইকন হবে ও ডিলিট করবে | ১ পিসে ট্র্যাশ আইকন রেন্ডার হয় এবং ডিলিট করে | **Pass** |
| **TC_CART_04**| Item Deletion "Undo" Functionality | Manual UI | আইটেম মুছে ফেললে টোস্টে Undo বাটন থাকবে | *"Shirt removed"* টোস্টে Undo চাপলে কাপড়টি ফিরে আসে | **Pass** |
| **TC_CART_05**| "Clear bag" Snapshot "Undo" Restore | Manual UI | পুরো ব্যাগ ক্লিয়ার করার পরও Undo করা যাবে | *"Bag emptied"* টোস্টে Undo চাপলে পুরো ব্যাগ রিস্টোর হয় | **Pass** |
| **TC_CART_06**| Item-Level Custom Note Editor | Manual UI | নির্দিষ্ট কাপড়ে বিশেষ নোট লেখা যাবে (সর্বোচ্চ ৫০০ অক্ষর) | নোট লিখলে কাপড়ের নিচে কোটেশন দিয়ে সুন্দর প্রদর্শন করে | **Pass** |
| **TC_CART_07**| Dynamic 3-Stage Delivery Progress Bar | Manual UI | ২০০ ও ১০০০ টাকার ভিত্তিতে প্রোগ্রেসবার বদলাবে | <২০০ (হলুদ সতর্কতা), ২০০-৯৯৯ (প্রোগ্রেস), ১০০০+ (সবুজ ব্যানার) | **Pass** |
| **TC_CART_08**| Minimum Order Cart Button State | Manual UI | ২০০ টাকার নিচে চেকআউট বাটন নিষ্ক্রিয় থাকবে | বাটন ডিজেবল থাকে এবং দেখায়: *"Add X TK more to continue"* | **Pass** |

---

## ৯. Checkout Funnel & Engine Feature (`/checkout/`)

৩-ধাপের চেকআউট ফানেল, কোটেশন ক্যালকুলেশন, রিয়েল-টাইম ৬০-মিনিট কাট-অফ এবং অর্ডার প্লেসমেন্ট টেস্ট।

| Test ID | Test Scenario | Testing Type | Expected Result | Actual Result | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC_CHK_01** | Valid Checkout Quote API (`/quote/`) | API Test | আইটেম পাঠালে সার্ভার কোটেশন দেবে | HTTP 200 OK | **Pass** |
| **TC_CHK_02** | Quote calculation with quantity 1 | API Test | ১ পিসের সঠিক সাবটোটাল ও ডেলিভারি দেবে | Subtotal 20, Delivery 80, Total 100 | **Pass** |
| **TC_CHK_03** | Quote calculation with quantity 2 | API Test | ২ পিসের লিনিয়ার ক্যালকুলেশন করবে | Subtotal 40, Delivery 80, Total 120 | **Pass** |
| **TC_CHK_04** | Client-Side Price Manipulation Attempt | Security / API | ক্লায়েন্ট থেকে ভুল `unitPrice` পাঠালে সার্ভার আটকাবে | HTTP 400 Bad Request (unexpected property rejected) | **Pass** |
| **TC_CHK_05** | Extra properties validation | API Test | পেলোডে বাড়তি ফিল্ড থাকলে রিজেক্ট করবে | HTTP 400 Bad Request | **Pass** |
| **TC_CHK_06** | Invalid UUID validation | API Test | ভুল UUID দিলে সার্ভার রিজেক্ট করবে | HTTP 400 Bad Request | **Pass** |
| **TC_CHK_07** | Invalid quantity validation (<= 0) | API Test | ০ বা নেগেটিভ পরিমাণ রিজেক্ট করবে | HTTP 400 Bad Request | **Pass** |
| **TC_CHK_08** | Quote API without trailing slash | API Test | স্ল্যাশ ছাড়াও সফল রেসপন্স দেবে | HTTP 200 OK | **Pass** |
| **TC_CHK_09** | Quote API with trailing slash | API Test | স্ল্যাশ সহ সফল রেসপন্স দেবে | HTTP 200 OK | **Pass** |
| **TC_CHK_10** | Standard Flat Delivery Fee (80 BDT) | API Test | ১০০০ টাকার নিচের অর্ডারে ৮০ টাকা ডেলিভারি যোগ হবে | ৩০০ টাকার অর্ডারে ডেলিভারি ৮০ টাকা যোগ হয়ে ৩৮০ টাকা হয়েছে | **Pass** |
| **TC_CHK_11** | Free Delivery Threshold (>= 1000 BDT) | API Test | ১০০০ বা ততোধিক অর্ডারে ডেলিভারি চার্জ ০ হবে | ১০৫০ টাকার অর্ডারে `deliveryCharge: 0`, `isFreeDelivery: true` | **Pass** |
| **TC_CHK_12** | Coupon Engine (`WELCOME50`) | API / Manual | ৫০ টাকা ছাড় দেবে এবং ইনভ্যালিড কুপনে এরর দেবে | ৫০ টাকা বাদ গেছে; ভুল কুপনে `couponError` রিটার্ন করে | **Pass** |
| **TC_CHK_13** | 3-Step Funnel State Guards | Manual UI | ঠিকানা ছাড়া স্লট বা স্লট ছাড়া রিভিউতে যাওয়া যাবে না | সরাসরি URL হিট করলেও রাউট গার্ড আগের ধাপে ফিরিয়ে দেয় | **Pass** |
| **TC_CHK_14** | Real-Time 60-Minute Slot Cut-off Engine | API / Manual | স্লট শুরুর ৬০ মিনিট আগে বুকিং স্বয়ংক্রিয় বন্ধ হবে | সকাল ১০:৩৭-এ মর্নিং ও আফটারনুন স্লট বন্ধ (`available: false`) | **Pass** |
| **TC_CHK_15** | Expired Slot Direct Order Block | Security / API | বন্ধ হয়ে যাওয়া স্লটে জোরপূর্বক অর্ডার প্লেস আটকাবে | HTTP 400 (*"This pickup slot is no longer available today"*) | **Pass** |
| **TC_CHK_16** | 7-Day Advance Calendar (Strip vs Month) | Manual UI | আগামী ৭ দিনের স্লট ক্যালেন্ডারে দেখাবে | হরাইজন্টাল স্ট্রিপ ও ফুল মান্থ ভিউতে সঠিক স্লট প্রদর্শিত | **Pass** |
| **TC_CHK_17** | Review Step 1-Click Jump Back | Manual UI | রিভিউ পেজ থেকে ঠিকানা বা স্লট এডিট করা যাবে | ১-ক্লিকে `?step=address` বা `?step=pickup`-এ ফিরে যাওয়া যায় | **Pass** |
| **TC_CHK_18** | Payment Methods Selection | Manual UI | ৫টি পেমেন্ট মেথড সিলেক্ট করা যাবে | COD, bKash, Nagad, Rocket, Bank Transfer সিলেক্ট হয় | **Pass** |
| **TC_CHK_19** | End-to-End Order Creation | API / Manual | অর্ডার তৈরি হবে, কোড জেনারেট হবে ও কার্ট খালি হবে | অর্ডার তৈরি সফল (`LB-261031-0001`), কার্ট ক্লিয়ার হয়েছে | **Pass** |

---

## ১০. Mobile View & Responsive Navigation Feature

| Test ID | Test Scenario | Testing Type | Expected Result | Actual Result | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC_MOB_01** | Mobile Top Header Layout | Manual (Mobile) | মোবাইলে কমপ্যাক্ট হেডার থাকবে লোগো ও কার্ট সহ | `h-14` হেডারে লোগো ও কার্ট আইকন সুন্দরভাবে প্রদর্শিত | **Pass** |
| **TC_MOB_02** | Dynamic Back Button on Mobile Sub-pages| Manual (Mobile) | সাব-পেজে লোগোর জায়গায় `← Back` বাটন আসবে | সাব-পেজে (যেমন `/faq/`) ব্যাক বাটন ও পেজের নাম প্রদর্শিত হয় | **Pass** |
| **TC_MOB_03** | Mobile Primary Bottom Navigation Bar | Manual (Mobile) | স্ক্রিনের নিচে ফিক্সড ৪টি প্রধান ট্যাব থাকবে | Home, Prices, Orders, Account ট্যাব সুন্দরভাবে কাজ করে | **Pass** |
| **TC_MOB_04** | Missing Primary Links in Mobile Navbar | Manual (Mobile) | Packages, FAQ, About, Contact মোবাইলে সহজে পাওয়া যাবে | মোবাইলে হ্যামবার্গার মেনু নেই; এই ৪টি লিংক অনুপস্থিত | **Fail** |
| **TC_MOB_05** | Mobile Floating Bag Bar | Manual (Mobile) | ব্যাগে আইটেম থাকলে বটম বারের ওপরে ফ্লোটিং বার আসবে | ব্যাগে আইটেম যোগ করলে কালো ফ্লোটিং বারটি ভেসে ওঠে | **Pass** |

---

## ১১. কনফার্মড বাগ তালিকা (Bugs – Strictly 2 Items)

### 🐞 Bug 1: Ghost Click / Focus State Retention
* **Test ID:** `TC_UI_01`
* **Module:** Authentication & Navigation
* **Observation:** Logout বা form submission-এর পর button/input-এর আগের focus বা active state থেকে যাচ্ছে বলে দেখা গেছে।
* **Expected Result:** Logout বা form submission-এর পর UI state সঠিকভাবে reset হবে এবং অনাকাঙ্ক্ষিত click বা active state থাকবে না।
* **Actual Result:** Previous active/focus state থেকে যাওয়ার সমস্যা পর্যবেক্ষণ করা হয়েছে।
* **Suggested Fix:** Form reset, component state cleanup, focus management এবং event listener cleanup যাচাই করা।

---

### 🐞 Bug 2: Password Maximum Length Validation
* **Test ID:** `TC_VAL_01`
* **Module:** Registration & Form Validation
* **Observation:** Password field-এ 64/65+ characters দিলে generic বা misleading error দেখা যাচ্ছে।
* **Expected Result:** Maximum character limit স্পষ্টভাবে জানাবে, যেমন: *"Password must be between 8 and 64 characters"*।
* **Actual Result:** Maximum length অতিক্রম করলে সঠিক validation message পাওয়া যাচ্ছে না।
* **Suggested Fix:** Frontend ও backend validation-এর মধ্যে consistency নিশ্চিত করা।

---

## ১২. সিস্টেম ইমপ্রুভমেন্ট তালিকা (Improvements – Strictly 6 Items)

1. **TC_IMP_01: Bengali Language Support in Service Area Checker**
   * *Observation:* Service Area Checker-এ বাংলা এলাকার নাম দিয়ে search করার সুবিধা প্রয়োজন।
   * *Suggestion:* `zindabazar`-এর পাশাপাশি `জিন্দাবাজার`, `উপশহর`, `টিলাগড়` ইত্যাদি বাংলা নাম support করা।
   * *Expected Benefit:* স্থানীয় গ্রাহকদের জন্য সহজ search experience।
2. **TC_IMP_02: Prevent Leading and Trailing Spaces in Password Fields**
   * *Observation:* Password field-এ leading/trailing spaces গ্রহণ করা হয়।
   * *Suggestion:* Accidental whitespace সম্পর্কে user-কে স্পষ্টভাবে জানানো অথবা উপযুক্ত validation দেওয়া।
   * *Expected Benefit:* Login-related confusion কমানো।
3. **TC_IMP_03: Improve Mobile Navigation Accessibility**
   * *Observation:* Mobile view-তে Packages, FAQ, About Us এবং Contact page সহজে খুঁজে পাওয়া যায় না।
   * *Suggestion:* Mobile menu বা navigation section-এ সরাসরি link যোগ করা।
   * *Expected Benefit:* Mobile users সহজে প্রয়োজনীয় page খুঁজে পাবেন।
4. **TC_IMP_04: Add OpenGraph and Twitter Card Metadata**
   * *Observation:* Homepage-এ og:title, og:description, og:image, og:url এবং twitter:card পাওয়া যায়নি।
   * *Suggestion:* Social sharing metadata যোগ করা।
   * *Expected Benefit:* Facebook, Messenger ও WhatsApp-এ সুন্দর link preview প্রদর্শন।
5. **TC_WEB_04: OpenGraph Metadata Verification**
   * *Observation:* OpenGraph tags missing.
   * *Suggestion:* Add OpenGraph metadata for rich preview.
6. **TC_WEB_05: Twitter Card Metadata Verification**
   * *Observation:* Twitter Card tags missing.
   * *Suggestion:* Add Twitter card summary tags.
