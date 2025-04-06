# IPC Nexus Frontend Documentation

This markdown file provides a **comprehensive breakdown of the frontend architecture** and component structure for the IPC Nexus web application. This is to be used for internal review and presentation purposes.

---

## 🛠️ Tech Stack

- **React.js** – UI Library
- **React Router DOM** – Routing management
- **Bootstrap 5** – Styling and responsiveness
- **CSS Modules** – Component-level styling (if used)
- **LocalStorage** – Authentication state management

---

## 📁 Project Structure Overview

```
├── App.js
├── components/
│   ├── Navbar.js
│   ├── Hero.js
│   ├── Features.js
│   ├── Footer.js
│   ├── HelpSupportWidget.js
│   ├── RegisterComplaint.js
│   ├── ComplaintHistory.js
│   ├── Learn_IPC.js
│   ├── About.js
│   ├── LawPage.js
│   ├── EvidenceReport.js
│   ├── CaseInfo.js
│   ├── CaseInfoCard.js
│   ├── LawyerInfo.js
│   ├── PoliceInfo.js
│   ├── CivilianDashboard.js
│   ├── PoliceDashboard.js
│   ├── LawyerDashboard.js
│   └── UserAuth/
│       ├── LoginForm.js
│       └── SignupForm.js
├── styles/
│   └── Components.css
```

---

## 🚦 Routing & Authentication

### Main Routing Logic: `App.js`

- **React Router** is used to define routes for public and protected pages.
- **ProtectedRoute** is a custom wrapper that checks if the user is authenticated based on `localStorage` keys (`isAuthenticated`, `userType`).

```jsx
<Route path="/dashboard/civilian" element={
  <ProtectedRoute userType="Civilian" element={<CivilianDashboard />} />
} />
```

### ProtectedRoute Logic

- Authenticates based on localStorage:
  ```js
  const isAuthenticated = localStorage.getItem('isAuthenticated') === 'true';
  const currentUserType = localStorage.getItem('userType');
  ```
- Redirects unauthorized users to their respective login pages:
  ```js
  if (!isAuthenticated || currentUserType !== userType) {
    return <Navigate to={`/login/${userType.toLowerCase()}`} />;
  }
  ```

### LocalStorage Keys Used

| Key             | Description              |
| --------------- | ------------------------ |
| isAuthenticated | Boolean for login status |
| userType        | Role of logged-in user   |
| username        | Name displayed in Navbar |

---

## 👤 Role-based Dashboards

Each role (Civilian, Police, Lawyer) has its own dedicated dashboard and component set:

### CivilianDashboard.js

- Components: `Hero`, `About`, `LawPage`, `HelpSupportWidget`
- Focused on awareness and legal education.

### PoliceDashboard.js

- Components: `Hero`, `Features`, `Footer`, `CaseInfoCard`
- Central location for operational tools.

### LawyerDashboard.js

- Components: `Hero`, `CaseInfoCard`, `CaseInfo`, `EvidenceReport`, `LawyerInfo`
- Supports legal workflows and case analysis.

---

## 🔗 Navbar Component Logic

### Dynamic UI Rendering

- The `Navbar` changes links based on authentication and user role:

```jsx
{userType === 'Police' && isAuthenticated && (
  <Link to="/register-complaint">Register Complaint</Link>
)}
```

### Route Visibility Per Role

| Role     | Navbar Links                                                              |
| -------- | ------------------------------------------------------------------------- |
| Civilian | IPC Sections                                                              |
| Lawyer   | IPC Sections, Case Info, Evidence Report, Lawyer Info                     |
| Police   | Register Complaint, IPC Sections, Evidence Report, Case Info, Police Info |

- Also includes a dropdown-based login/signup system for all user types.
- Greets user with name from `localStorage`.

💡 The Navbar is a central UX element dynamically updated via localStorage-based role detection.

---

## 🧩 Feature Highlights

### 1. **Modular Components**

Each UI element is a reusable component to maintain separation of concerns and scalability.

### 2. **Real-time Navbar Updates**

- Uses `storage` event listener to reactively update UI when localStorage changes (e.g., login/logout).

### 3. **Custom HelpSupportWidget**

- A support/contact/help system visible on nearly all pages.

### 4. **Complaint Management**

- `RegisterComplaint`, `ComplaintHistory` (for civilians), `CaseInfo` and `EvidenceReport` (for police/lawyers) manage workflows.

### 5. **IPC Section & Legal Info**

- `Learn_IPC.js` displays Indian Penal Code sections.
- `LawPage.js` routes dynamically by `lawId` in URL.

### 6. **CaseInfoCard Component**

- A reusable card used to show **individual case summaries** inside dashboards and detailed views.
- Displays basic details like case number, title, date, and a link to full details.
- Can be used in `CaseInfo.js`, `PoliceDashboard.js`, or `LawyerDashboard.js` for showing case lists dynamically.

---

## Let's take live Front-end Tour



- Landing Page
- Dashboards (Civilian, Police, Lawyer)
- IPC Table View
- Register Complaint
- Case Info
- Evidence Report

---

## 🔄 Flow Diagrams

### Routing Logic

```text
User → App.js → Role-based Route → Dashboard Component
```

### Authentication Flow

```text
Login/Signup → Store `isAuthenticated`, `userType`, `username` in localStorage → Redirect to role dashboard
```


---

## 🌳 Component Hierarchy Tree (simplified)

```
App.js
│
├── Navbar
├── Routes
│   ├── CivilianDashboard
│   │   ├── Hero
│   │   ├── About
│   │   ├── LawPage
│   │   └── HelpSupportWidget
│   ├── LawyerDashboard
│   │   ├── Hero
│   │   ├── CaseInfoCard
│   │   ├── CaseInfo
│   │   ├── EvidenceReport
│   │   └── LawyerInfo
│   ├── PoliceDashboard
│   │   ├── Hero
│   │   ├── Features
│   │   ├── Footer
│   │   └── CaseInfoCard
└── Login / Signup Pages
```

---

## 🧠 Key Logic Snippets

### Authentication Storage

```js
localStorage.setItem('isAuthenticated', 'true');
localStorage.setItem('userType', user.userType);
localStorage.setItem('username', user.name);
```

### Logout Handler

```js
localStorage.clear();
window.location.href = '/login/civilian';
```

### Role-based Navbar Rendering

```js
{userType === 'Lawyer' && isAuthenticated && (
  <Link to="/caseInfo">Case Info</Link>
)}
```

---

# 🚀 IPC Nexus - API Integration Documentation

This document provides a comprehensive overview of how API integration is handled in the IPC Nexus web application. It covers:

- API Endpoints
- Request Payloads
- Sample Responses
- Local Storage Usage
- Redirection Flow
- Important Notes

---

## 🌐 API Base URL

```
http://127.0.0.1:5000/api/
```

Use this as your base for all endpoints. In production, replace it with:

```
${config.API_URL}
```

---

## 🔐 Signup API

### 📌 Endpoint

```
POST /api/{userType}/signup
```

**userType** can be:
- `civilian`
- `lawyer`
- `police`

### 📨 Request Payload

#### ✅ Civilian
```json
{
  "username": "john_doe",
  "email": "john@example.com",
  "phoneno": "9876543210",
  "password": "securePassword"
}
```

#### ✅ Lawyer / Police
```json
{
  "id": "LAW123",
  "email": "officer@example.com",
  "phoneno": "9876543210",
  "password": "securePassword"
}
```

### ✅ Success Response
```json
{
  "message": "Signup successful! Please login."
}
```

### ❌ Error Response
```json
{
  "message": "User already exists"
}
```

---

## 🔑 Login API

### 📌 Endpoint

```
POST /api/{userType}/login
```

**userType** can be:
- `civilian`
- `lawyer`
- `police`

### 📨 Request Payload
```json
{
  "idOrUsername": "john_doe",
  "password": "securePassword"
}
```

### ✅ Success Response
```json
{
  "message": "Login successful!",
  "badge_id": "P1234",
  "civilian_id": "C1234",
  "account_id": "L1234"
}
```

### ❌ Error Response
```json
{
  "message": "Invalid credentials"
}
```

---

## 📝 Complaint Registration API (Police Only)

### 📌 Endpoint

```
POST /api/police/complaint
```

### 🧾 Headers
```
Content-Type: application/json
```

### 📨 Request Payload
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "phone": "9876543210",
  "incidentDate": "2025-04-06",
  "location": "Downtown",
  "address": "123 Main Street",
  "description": "Detailed complaint text here...",
  "badge_id": "P1234",
  "timestamp": "2025-04-06T12:00:00Z"
}
```

### ✅ Success Response
```json
{
  "case_id": "CASE20250406-001"
}
```

### ❌ Error Response
```json
{
  "message": "Unauthorized access or validation error"
}
```

---

## 🗃️ LocalStorage Usage

| Key              | Purpose                            |
|------------------|-------------------------------------|
| `userType`       | Stores the role (civilian/lawyer/police) |
| `isAuthenticated`| Boolean to track login state        |
| `username`       | For civilians                       |
| `badge_id`       | For police                          |
| `civilian_id`    | For civilians                       |
| `lawyer_id`      | For lawyers                         |

---

## 🔄 Redirection Flow

- ✅ After **Signup** → `/login/{userType}`
- ✅ After **Login** → `/dashboard/{userType}`
- ❌ Unauthorized complaint access → `/login/police`

---

## 📌 Notes

- 🔒 Always validate inputs before sending API calls.
- 🔁 Use `window.location.reload()` after login to refresh auth state.
- ❗ Store only necessary data in `localStorage` for security.
- ⏱️ Consider adding `axios` timeouts and error handling.
- 🔄 Ensure police badge ID is stored and passed for complaint registration.

---

## 💡 Technologies Used

- **ReactJS** for frontend
- **Flask REST API** for backend
- **Axios** for HTTP requests
- **LocalStorage** for session management

---

## ✉️ Contacts

For backend issues or bug reports, contact the development team at:
```
support@ipcnexus.org
```

---


