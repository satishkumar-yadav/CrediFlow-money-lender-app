# CrediFlow - Money Lender Management System

## Problem Statement

Traditional money lending operations often rely on manual record-keeping, face challenges in maintaining customer data securely, tracking loan disbursements and repayments, and managing interest calculations. These issues lead to inefficiencies, risks of human error, lack of transparency, and difficulties in customer management. Businesses require a seamless, secure, and scalable digital solution to overcome these hurdles, streamline lending processes, and ensure transparent communication with customers.

## About This Project

The **CrediFlow** is a full-stack web application built with modern technologies including Next.js, MongoDB, and Redux Toolkit. It offers a comprehensive platform where lenders can manage customers and loans efficiently, automate interest calculations, track payments, and facilitate secure OTP-based login for both admins and customers. The system includes an intuitive admin dashboard for customer management and financial operations and a dedicated customer portal for personal loan details and payment history.

### How it Solves the Problem

- **Centralized Management:** The system centralizes customer profiles, loan details, and payment records in a secure database.
- **Real-time Tracking:** Payments and outstanding amounts with interest are updated dynamically with clear audit trails.
- **Secure Access:** OTP-based login enhances security for customers and admins.
- **Automated Interest Handling:** Interest structures can be modified and applied automatically to loans.
- **Soft and Hard Delete Mechanism:** Manages customer lifecycle carefully, preventing accidental data loss.
- **Responsive Design:** Fully desktop and mobile responsive for ease of access from multiple devices.

### Differentiation from Existing Solutions

- **Comprehensive Financial Controls:** Unlike many isolated loan management apps, this includes payment modes, receipt uploads, and interest configuration.
- **Full Stack Next.js Solution:** Combines frontend and backend into a seamless developer experience with API routes.
- **Built-in OTP Authentication:** Directly integratedOTP login workflows are uncommon in standard money lending apps.
- **Soft Delete with Auto Hard Delete:** Provides safer data management and GDPR-compliant deletion processes.
- **Open Source Modular Architecture:** Designed for easy customizations and integrations tailored to lender-specific needs.

---

## Project Workflow and Working

### About

**CrediFlow** - Fullstack Next.js + MongoDB platform with Admin & Customer portals, secure OTP login, payments, interest structure, mobile/desktop responsive UI.

### Data Flow Diagram (DFD)

```
[Customer] --Login/Payment Info--> [Frontend UI in Next.js] --> [API Routes] --> [MongoDB Database]
^ |
|---------------------------------------------------------------------|


+---------------+ +------------------+ +----------------+
| Customer | <---> | Frontend UI | <---> | Backend |
| (Portal Login,| | (Next.js React) | | (API Routes) |
| Payments & | | | | |
| Profile View) | | | | |
+---------------+ +------------------+ +----------------+
| |
| |
v v
+-------------+ +---------------+
| OTP Service | | MongoDB Atlas |
| (Twilio/ | | (Customer, |
| Email API) | | Admin, Payment|
+-------------+ | Collections) |
+---------------+

+---------------+ +---------------+ +-------------+
| Admin |<---->| Admin Frontend|<--->| Admin APIs |
| (Dashboard, | | (Next.js) | | (Customer, |
| Customers, | +---------------+ | Payments, |
| Payments) | | Interest) |
+---------------+ +-------------+
```

- Customers and admins interact with a React-based frontend.
- Actions trigger API calls for authentication, customer CRUD, payments, and interest updates.
- MongoDB stores all persistent data including profiles, loans, payments, and audit logs.
- Background processes handle soft-delete expiry and hard-deletion.

### Workflow / Working Diagram

```
[Customer]
|
v
[OTP Login Page] <--> [Send OTP Service]
|
v
[Dashboard] <---------------------------------------
| |
v |
[View Payments] | (Makes API calls)
| |
v |
[Make Payment] ---> [Upload Receipt (Image Upload API)]
| |
v v
[Payment Recorded] <------------------------ [Backend APIs]
|
v
[MongoDB Database]

[Admin]
|
v
[Admin Dashboard]
| \
 | ------> [Add/Edit Customers]
| \
 | ------> [Record Payments]
| \
 | ------> [Set Interest Rates]
| \
 | ------> [View Soft Deleted Customers]
| |
| v
| [Soft Delete Expiry & Hard Delete Job]
|
v
[Modify Financial Data] (All changes update MongoDB)
```

### Design Flow Chart

```
+------------------+ +-------------------+ +-----------------+
| Client Interface | <--> | API Routes / Logic | <--> | MongoDB Database |
+------------------+ +-------------------+ +-----------------+
| |
| +-- OTP Service (Twilio/Nodemailer)
| +-- Image Upload Handler
| +-- Authentication / Authorization
```

### Low-Fidelity Wireframe Diagram

1. Admin Dashboard Wireframe

```
+-------------------------------------------------------+
| Navbar (Logo, User Menu) |
+-------------------------------------------------------+
| Sidebar (Menu: Dashboard, Add Customer, Interest, ...)|
+-------------------------------------------------------+
| Main Content: |
| [Customer List] |
| +-------+ +-------+ +-------+ +-------+ +----------+ |
| | Name | | Phone | | Amount| |Interest| | Actions | |
| +-------+ +-------+ +-------+ +-------+ +----------+ |
| | ... | | ... | | ... | | ... | | Edit/Delete| |
+-------------------------------------------------------+
```

2. Customer Dashboard Wireframe

```
+--------------------------------+
| Navbar (Logo, Logout) |
+--------------------------------+
| Personal Info |
| +----------------------------+ |
| | Name: ... | |
| | Phone: ... | |
| | Address: ... | |
| +----------------------------+ |
| Financial Details |
| +----------------------------+ |
| | Amount Taken: ... | |
| | Interest: ... % | |
| | Amount Remaining: ... | |
| +----------------------------+ |
+--------------------------------+
| Buttons: |
| [View Payment History] [Make Online Payment] |
+--------------------------------+
```

3. OTP Login Page Wireframe

```
+------------------------+
| Login |
+------------------------+
| [Enter Phone or Email] |
| [Send OTP Button] |
| [Enter OTP] |
| [Verify OTP Button] |
+------------------------+
| Error / Info Messages |
+------------------------+
```

---

## Project Structure

### Part 1: Project Setup & Structure

Tech Stack

- Frontend: Next.js (with Redux Toolkit, hooks, axios)
- Backend: Next.js API routes or Node.js (Express.js)
- Database: MongoDB (Mongoose ORM)

Folder Structure

- /components # Reusable React components
- /pages # Next.js pages (frontend routes)
- /store # Redux Toolkit slices, store config
- /api # Next.js API (if using API routes)
- /models # Mongoose schemas/models
- /services # API & business logic
- /public # Static assets (profile pics, receipts)
- /utils # Helper functions (OTP, validation etc.)

---

### Part 2: Database Design

Customer Model

- Unique ID (auto-generated)
- Name, father's name, phone
- Aadhar, address
- Profile pic (URL)
- Credentials: username/phone, password (hashed), OTP fields
- Financials: amount, interest, tenure
- Payment history: [
  { date, amount, mode, receiptURL }
  ]
- Status: active/soft-deleted/hard-deleted

Admin Model

- Username, password (hashed), role

Payment Model

- Customer reference, amount, date, payment mode, receipt, interest updated, remaining amount

---

### Part 3: Backend Implementation

API Endpoints

Auth:

- POST /api/auth/admin-login: OTP based
- POST /api/auth/customer-login

Customer CRUD (Admin only):

- POST /api/customers: Add new customer (auto default credential)
- GET /api/customers: List all customers, search by ID/phone
- PUT /api/customers/:id: Update customer
- DELETE /api/customers/:id/soft: Soft delete (flag in db)
- DELETE /api/customers/:id/hard: Remove from db (after 2 months)

Payments:

- GET /api/customers/:id/payments: List payments
- POST /api/customers/:id/payments: Add payment (amount, mode, receipt, interest/remaining update)
- PUT /api/customers/:id/financials: Update interest/tenure info

OTP Handling:

- Generate OTP, send via SMS/email (integration with 3rd party)

---

### Part 4: Frontend Implementation

Authentication & Security

- OTP-based login for both admin and customer
- Session tokens/cookies for security (JWT or NextAuth)
- First login: force password change for customers with auto credentials

Admin Dashboard

- Responsive UI (desktop/mobile)
- List/add/edit/search customers
- View customer info, payment history, financial details
- Payment receiving page; search customer by ID/phone, accept updates/payment, upload receipt/screenshot
- Manage interest structure/financial info per customer or globally
- Delete actions: show "soft deleted" customers with countdown till hard delete

Customer Dashboard

- OTP-secured login
- View/edit profile (change password after first login)
- See personal details (all fields)
- Financials: amount, interest, payment schedule/tenure
- Payment history: view all payments, see receipts/screenshots
- Online payment: fill form, set mode to 'online', attach payment screenshot, record date/time

---

### Part 5: Soft Delete & Hard Delete Logic

- Soft Delete: Set status: 'soft-deleted' and record delete date, hide from normal list.
- Automatic Hard Delete: Use cron job/serverless schedule to check for 2 months expired, then delete from DB permanently.

---

### Part 6: Responsive Design & UI Frameworks

- Use Tailwind CSS for easy responsive styling.
- Desktop/mobile layouts tested with Next.js routing.
- Reusable forms, modals for receipts/payment screens.

---

### Part 7: Deployment

- Environment: Deploy on Vercel (Next.js app + API on serverless functions) or server (Express.js backend).
- Use MongoDB Atlas for cloud database.
- Use proper .env files for DB/password configurations.
- SSL/HTTPS for secure login and data transmission.

---

### Next Steps & Coding Sequence

- Project initialization (Next.js, Tailwind, Redux Toolkit, Mongoose models)
- Set up MongoDB and admin/customer models
- Add authentication (OTP logic, login, session)
- Admin dashboard: customer CRUD, payment page, soft/hard delete
- Customer dashboard: payment history, pay online, profile features
- Payment receipt image upload (Next.js file API)
- Interest structure logic
- Testing and deployment scripts
- Responsive layout for mobile/desktop

---

### Project Directory Structure

```
money-lender-app/
├── components/
│ ├── Admin/
│ ├── Customer/
│ ├── Auth/
│ ├── Shared/
│ ├── ui/
├── pages/
│ ├── admin/
│ ├── customer/
│ ├── auth/
│ ├── api/
├── store/
├── models/
├── services/
├── public/
│ ├── profile_pics/
│ ├── receipts/
├── utils/
├── middleware.ts
├── tailwind.config.js
├── next.config.js
├── package.json
├── .env.local
├── README.md
```

#### Detailed Directory Structure

```
money-lender-app/
├── components/
│ ├── Admin/
│ │ ├── CustomerList.tsx
│ │ ├── AddCustomerForm.tsx
│ │ ├── EditCustomerForm.tsx
│ │ ├── PaymentForm.tsx
│ │ ├── InterestSettings.tsx
│ ├── Customer/
│ │ ├── Dashboard.tsx
│ │ ├── PaymentHistory.tsx
│ │ ├── OnlinePaymentForm.tsx
│ ├── Auth/
│ │ ├── OTPLogin.tsx
│ │ ├── SetPassword.tsx
│ ├── Shared/
│ │ ├── Navbar.tsx
│ │ ├── Sidebar.tsx
│ │ ├── ProfilePicUploader.tsx
│ │ ├── ReceiptUploader.tsx
│ └── ui/
│ ├── Input.tsx
│ ├── Button.tsx
├── pages/
│ ├── admin/
│ │ ├── index.tsx
│ │ ├── add.tsx
│ │ ├── edit/[id].tsx
│ │ ├── payments/[id].tsx
│ │ ├── interest.tsx
│ │ ├── deleted.tsx
│ ├── customer/
│ │ ├── index.tsx
│ │ ├── pay.tsx
│ │ ├── payments.tsx
│ │ ├── profile.tsx
│ ├── auth/
│ │ ├── login.tsx
│ │ ├── otp.tsx
│ │ ├── reset-password.tsx
│ │ ├── set-password.tsx
│ ├── api/
│ │ ├── admin/
│ │ │ ├── login.ts
│ │ │ ├── customers.ts
│ │ │ ├── payments.ts
│ │ │ ├── interest.ts
│ │ │ ├── deleted.ts
│ │ ├── customer/
│ │ │ ├── login.ts
│ │ │ ├── pay.ts
│ │ │ ├── payments.ts
│ │ │ ├── profile.ts
│ │ ├── auth/
│ │ │ ├── otp.ts
│ │ │ ├── set-password.ts
│ │ │ ├── reset-password.ts
├── store/
│ └── index.ts
│ └── slices/
│ ├── adminSlice.ts
│ ├── customerSlice.ts
│ ├── paymentSlice.ts
├── models/
│ ├── Customer.ts
│ ├── Admin.ts
│ ├── Payment.ts
├── services/
│ ├── mongo.ts
│ ├── otp.ts
│ ├── imageUpload.ts
│ └── payment.ts
├── public/
│ ├── profile_pics/
│ ├── receipts/
├── utils/
│ ├── validateAadhar.ts
│ └── validatePhone.ts
├── middleware.ts
├── tailwind.config.js
├── next.config.js
├── package.json
├── .env.local
├── README.md
```

---

## Installation and Setup

### Prerequisites

- Node.js (v16+ recommended)
- MongoDB Atlas or local MongoDB instance
- npm or pnpm package manager

### Steps to Install and Run

1. **Project Initialization Commands**

```
pnpm create next-app@latest money-lender-app --typescript
cd money-lender-app
```

2. **Or Clone the repository**

```
git clone https://github.com/satishkumar-yadav/money-lender-app.git
cd money-lender-app
```

3. **Install dependencies**

#### Runtime Dependencies

bash

```
npm install axios
npm install mongoose(MongoDB ORM)
npm install react-redux (React bindings for Redux)
npm install @reduxjs/toolkit (Redux state management toolkit)

npm install bcryptjs (Password hashing)
npm install jsonwebtoken (JWT handling)
npm install multer next-auth zod (File/image upload handling, auth)

npm install twilio (Optional, for SMS OTP sending)
npm install nodemailer (Optional, for sending emails)
```

#### Development Dependencies (TypeScript typings) / Types (dev)

```
npm install --save-dev @types/mongoose @types/react @types/node @types/bcryptjs @types/react-redux  @types/multer
```

#### Dependencies List with Installation Commands

```
pnpm install axios mongoose react-redux @reduxjs/toolkit bcryptjs jsonwebtoken multer next-auth zod twilio nodemailer
```

4. **Configure environment variables**

Create a `.env.local` file with the following:

```
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret_key
TWILIO_SID=your_twilio_sid (if using)
TWILIO_AUTH_TOKEN=your_twilio_auth_token (if using)
DEFAULT_PASSWORD=changeme123
```

5. **Run the development server**

```
pnpm dev
```

6. **Access the app**

```
Open [http://localhost:3000](http://localhost:3000) in your browser.
```

---

## How to Use

**Admin:**

- Login via OTP or credentials.
- Add and manage customers and loan details.
- Record payments, upload receipts.
- Adjust interest settings.
- Monitor soft deleted customers and perform permanent deletes.

**Customer:**

- Secure OTP login.
- View personal loan and payment history.
- Make online payments and upload payment proof.
- Edit personal profile including photo upload.

---

## Demo Screenshots

![Admin Dashboard](./screenshots/admin-dashboard.png)
_Admin dashboard showing customer list_

![Add Customer Form](./screenshots/add-customer.png)
_Form to add new customer details_

![Payment Form](./screenshots/payment-form.png)
_Admin page to record payment with receipt upload_

![Customer Dashboard](./screenshots/customer-dashboard.png)
_Customer personal loan overview_

![Online Payment](./screenshots/online-payment.png)
_Customer portal online payment submission_

---

## Developer and Contributor Information

- **Lead Developer:**  
  Satish Kumar Yadav  
  Email: satishkumaryadav8730@gmail.com
  GitHub: [github.com/satishkumar-yadav](https://github.com/satishkumar-yadav)

- **Contributors:**

  - Satish Kumar Yadav ([github.com/satishkumar-yadav](https://github.com/satishkumar-yadav))

- **License:** MIT

---

Thank you for using the CrediFlow!  
Feel free to contribute or provide feedback via GitHub issues and pull requests.

---
