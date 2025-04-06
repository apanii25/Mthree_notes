# 🚀 React Frontend for Legal Information & Complaint Portal

## 📘 Project Overview
A modern, responsive web application built in **React** to serve three distinct user roles:
- 👮‍♂️ **Police**: Register complaints, view assigned cases
- 🧑‍💼 **Lawyers**: Access IPC sections and legal info
- 👨‍👩‍⚖️ **Civilians**: File and track complaints, access legal knowledge

This frontend interfaces with a Flask backend (not covered here), and focuses on delivering role-specific functionality with clean routing and componentized UI.

---

## 🛠️ Tech Stack
- React (Functional Components & Hooks)
- React Router DOM
- CSS Modules
- LocalStorage for temporary auth simulation

---

## 🗂️ Folder Structure
```bash
src/
├── App.jsx
├── App.css
├── main.jsx
├── config.js
├── components/
│   ├── Navbar.jsx
│   ├── Hero.jsx
│   ├── Features.jsx
│   ├── Footer.jsx
│   ├── RegisterComplaint.jsx
│   ├── LawPage.jsx
│   ├── Learn_IPC.jsx
│   ├── About.jsx
│   ├── ComplaintHistory.jsx
│   ├── HelpSupportWidget.jsx
│   ├── CivilianDashboard.jsx
│   ├── PoliceDashboard.jsx
│   ├── LawyerDashboard.jsx
│   ├── UserAuth/
│   │   ├── LoginForm.jsx
│   │   ├── SignupForm.jsx
│   │   └── ProtectedRoute.jsx
│   └── ...
├── assets/  # Images
└── styles/  # CSS files
```

---

## 🧭 Routing Overview (from `App.jsx`)
React Router handles navigation between public and protected routes based on user type.

```jsx
<Route path="/dashboard/civilian" element={<ProtectedRoute element={<CivilianDashboard />} userType="Civilian" />} />
<Route path="/dashboard/police" element={<ProtectedRoute element={<PoliceDashboard />} userType="Police" />} />
<Route path="/dashboard/lawyer" element={<ProtectedRoute element={<LawyerDashboard />} userType="Lawyer" />} />
```

Role-based protection is handled using `localStorage` keys like `isAuthenticated` and `userType`.

---

## 🔐 Protected Route Logic
```jsx
const ProtectedRoute = ({ element, userType }) => {
  const isAuthenticated = localStorage.getItem('isAuthenticated') === 'true';
  const currentUserType = localStorage.getItem('userType');
  return isAuthenticated && (!userType || currentUserType === userType) ? (
    element
  ) : (
    <Navigate to="/login/civilian" replace />
  );
};
```

---

## 🧩 Component Snapshot: Police Dashboard
```jsx
const PoliceDashboard = () => {
  return (
    <div>
      <Hero />
      <Features />
      <Footer />
    </div>
  );
};
```
Displays police-specific widgets and information blocks.

---

## 🖼️ UI Previews
> Replace these with actual screenshots in your local project’s `./screenshots/` folder.

```md
### Civilian Dashboard
![Civilian Dashboard](./screenshots/civilian_dashboard.png)

### Police Complaint Page
![Police Complaint](./screenshots/police_complaint.png)
```

---

## 🚀 Running the App
```bash
npm install
npm run dev
```
Runs the app locally at `http://localhost:5173` or similar, depending on Vite or Create React App config.

---

## 🔮 Future Enhancements
- Integrate context API or Redux for global auth state
- Replace `localStorage` auth with secure token-based handling
- Add loading states and error boundaries
- Role-specific dashboards with analytics and charts

---

## 🙌 Acknowledgements
Thanks to all contributors, open-source libraries, and the Indian Penal Code documentation that powers this interface.

