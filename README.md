# ABABIL EXTRACTOR - Ultimate Lead Generation Suite

<div align="center">
<img width="1200" height="475" alt="GHBanner" src="./public/banner.png" />
</div>

## Overview

**ABABIL EXTRACTOR** is a comprehensive, all-in-one lead generation and data extraction suite built with Next.js and Electron. It goes far beyond standard mapping tools, offering a complete arsenal of utilities to discover, extract, and validate business leads. The application is secured by a robust, scalable authentication and activation system utilizing Supabase.

**Key Features:**
-   **GMB Scraper**: Effortlessly extract high-quality leads, emails, and business details directly from Google Maps.
-   **Web Scraper**: Built-in web scraping module for deep data extraction across various websites.
-   **Ultimate Extract**: Advanced capabilities offering unlimited data extraction without arbitrary limits.
-   **X-Ray Search**: Precision boolean search functionality to laser-target specific professionals and companies.
-   **Bulk Email Sender**: Integrated outreach tool to effortlessly run email campaigns directly to your newly extracted leads.
-   **Email Validator**: Built-in email verification to ensure high deliverability and protect your sender reputation.
-   **Secure Authentication**: Streamlined user registration, login, and secure password hashing via Supabase.
-   **Role-Based Access**: Granular control with DEFAULT and PRO roles granting different extraction limits and tool access.
-   **Device Locking**: Strictly binds activation keys to hardware MAC addresses (or browser fingerprints) to prevent unauthorized sharing.
-   **Offline Grace Period**: Ensures uninterrupted workflows by allowing valid users to run the software offline for up to 7 days.
-   **Admin Dashboard**: Dedicated, secure admin panel to easily manage users, monitor usage logs, update global settings, and control roles.

---

## 🚀 Getting Started

### Prerequisites
-   Node.js installed.
-   A Supabase Account (for PostgreSQL database and authentication).

### Installation

1.  **Install dependencies:**
    ```bash
    npm install
    ```

2.  **Environment Setup:**
    Set the `GEMINI_API_KEY` in `.env.local` (if using AI features).

3.  **Run Locally:**
    ```bash
    npm run dev
    ```

---

## 🛠️ Architecture & Tools

-   **Frontend**: Next.js, React, Tailwind CSS.
-   **Backend**: Supabase (PostgreSQL Database, Authentication).
-   **Authentication**: Custom implementation with HMAC-SHA256 for key generation.
-   **Scraping**: Playwright / Puppeteer (server-side).

---

## 🔐 Authentication & Activation System

The system uses Supabase as a robust PostgreSQL database to manage users, licenses, and roles with Row Level Security (RLS).

### Features
-   **User Registration & Login**: Secure password handling.
-   **Activation Keys**: Deterministic keys generated from MAC address/Browser User Agent.
-   **Master Key**: Global bypass key for admins.
-   **7-Day Cache**: Login sessions and activation status persist for 7 days locally.
-   **Brute-force Protection**: Auto-blocks users after 10 failed activation attempts.
-   **Forgot Password**: Users can reset passwords using their Activation Key.

### Security Measures
-   **Role Impersonation Prevention**: Critical endpoints verify `activationKey` against the email to prevent local license tampering.
-   **Secure Key Handling**: Activation keys are never exposed during login; they are only sent via email or verified server-side.
-   **Real-Time Status Check**: Scraping jobs check user status (`ACTIVE`/`BLOCK`) and role (`DEFAULT`/`PRO`) directly from the backend before execution.

---

## ⚙️ Supabase Setup (Backend)

1.  Create a new project on [Supabase](https://supabase.com/).
2.  Set up your database tables (e.g., `users`, `app_settings`, `user_logs`).
3.  Configure Row Level Security (RLS) policies to ensure secure access to user data.
4.  Copy your **Project URL** and **anon key** from the Supabase dashboard (Project Settings > API).
5.  Set these values in your `.env.local` file:
    ```env
    NEXT_PUBLIC_SUPABASE_URL=your-project-url
    NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
    ```

---

## 👮 Admin Workflow

### Generating Keys
-   **API**: Send a POST request with `action: "issueKey"`.
-   **Offline**: Compute `HMAC_SHA256(HMAC_SECRET, macAddress)`.

### Master Key
To enable a master key (bypass for all users), configure it within the `app_settings` table in Supabase or directly via the built-in Admin Dashboard settings tab.

### Managing Users
-   **Block User**: Change the user's status to "BLOCK" via the Admin Dashboard.
-   **Change Role**: Update the user's role to "PRO" or "DEFAULT" via the Admin Dashboard.
-   **Reset Password**: Users can request this via the app, and administrators can oversee account details in the Supabase Studio or the Admin Dashboard.

---

## 📝 Recent Updates (v17.0+)

-   **Supabase Migration**: Complete transition to Supabase for enhanced security, performance, and RLS capabilities.
-   **Admin Dashboard Overhaul**: Fully functional internal admin panel for users, logs, and settings.
-   **Login Session Persistence**: Users stay logged in for 7 days.
-   **Forgot Password**: Integrated flow to reset passwords.
-   **Auto-Block**: Automatically blocks IPs/Users after 10 failed activation attempts.
-   **Deterministic Keys**: Keys are now strictly tied to the device/browser fingerprint.
