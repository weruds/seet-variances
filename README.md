# ODC SEET Variance Dashboard (myMVP)

**Developer:** Wilson Serquina
**Build:** v1.5.1
**Project:** `seet-variances` (Firebase)
**Hosted URL:** https://seet-variances.web.app

---

## 1. What This Dashboard Does

The ODC SEET Variance Dashboard (myMVP) is a browser-based tool for monitoring, validating, and pinpointing attendance variances within the ODC SEET team. It compares two attendance data sources — **Monday.com** records and **ILC (IBM Labour Centre)** clockings — against employee WFH schedules, surfaces discrepancies, and allows employees to submit explanations for review.

### Core capabilities

| Area | What it does |
|---|---|
| **My Variances** | Each signed-in employee sees their own private variance report |
| **Variance Review** | Auditors review and resolve submitted variance explanations |
| **Monthly Attendance (ILC)** | Upload and browse ILC timesheet exports by month |
| **Monthly Attendance (Monday.com)** | Upload and browse Monday.com attendance exports by month |
| **Monday vs WFH Schedule** | Detect WFH compliance variances against employee schedules |
| **Monday vs ILC** | Detect discrepancies between Monday.com and ILC clockings |
| **Discrepancy List** | Consolidated view of all detected discrepancies |
| **Masterlist** | Employee registry, auto-populated from ILC uploads |
| **WFH Schedule** | Manage WFH days per employee |
| **Onboard New Employee** | Add new team members to the masterlist |
| **Service Lines** | Manage service line codes and descriptions |
| **Role Maintenance** | Assign Admin or Auditor roles to employees |

### User roles

| Role | Access |
|---|---|
| **Employee** | View own variances only (My Variances page) |
| **Auditor** | View masterlist, WFH schedule, attendance records, detect variances, notify employees |
| **Admin** | Full access — all of the above plus onboarding, service lines, role management, import tools |

---

## 2. Technology Stack

| Layer | Technology |
|---|---|
| UI | HTML5, CSS3, Vanilla JavaScript (no build step) |
| Authentication | Firebase Auth (email + password) |
| Database | Cloud Firestore |
| Hosting | Firebase Hosting |
| File parsing | SheetJS (CSV / XLSX) |
| Fonts | Google Fonts — Space Grotesk, Inter |

The dashboard is a **single `index.html` file**. There is no Node.js server, no frontend build tool, and no bundler required to run it.

---

## 3. Architecture

```
Browser (index.html)
  │
  ├── Firebase Auth          — email/password sign-in, role detection
  ├── Cloud Firestore        — attendance data, variances, masterlist, WFH schedule
  └── Firebase Hosting       — serves index.html and 404.html
```

### Firestore collections used

| Collection | Purpose |
|---|---|
| `users/{uid}` | User profile and role (admin / auditor / employee) |
| `masterlist/{serialNo}` | Employee records |
| `servicelines/{id}` | Service line codes and descriptions |
| `wfhSchedule/{serialNo}` | WFH day assignments per employee |
| `ilcAttendance/{docId}` | Uploaded ILC attendance records |
| `mondayAttendance/{docId}` | Uploaded Monday.com attendance records |
| `wfhVariances/{docId}` | Detected Monday vs WFH variances |
| `ilcVariances/{docId}` | Detected Monday vs ILC variances |
| `variances/{docId}` | Employee variance submissions and review status |

---

## 4. Accessing the Dashboard

### Option A — Use the hosted site (recommended)

Open your browser and go to:

```
https://seet-variances.web.app
```

No installation needed. Sign in with the IBM email and temporary password provided by your administrator.

### Option B — Open the file locally

Download or clone the project, then open `index.html` directly in your browser:

```powershell
Start-Process ".\index.html"
```

> **Note:** Local file access works for viewing but may behave differently for Firebase Auth and Firestore due to browser security restrictions. Use the hosted URL whenever possible.

---

## 5. First Time Sign-In (All Users)

### Step 1 — Get your credentials

Ask your administrator (Wilson Serquina) for:
- Your **IBM email address** (e.g. `firstname.lastname@ibm.com`)
- Your **temporary password**

### Step 2 — Open the dashboard

Go to **https://seet-variances.web.app** in Chrome, Edge, or Firefox.

### Step 3 — Sign in

1. Click **Sign in** at the bottom of the left sidebar.
2. Enter your IBM email and temporary password.
3. Click **Sign In**.

### Step 4 — Change your password (first login only)

On your very first login the dashboard will prompt you to set a new password.

1. Enter a new password (minimum 8 characters).
2. Confirm it.
3. Click **Update Password**.

You are now signed in and will see the content available for your role.

---

## 6. Navigation Guide (Step by Step)

### What you see when you land on the page

When you first open the dashboard you land on the **My Variances** page. The left sidebar shows only the pages available for your role.

- **Employees** see only "My Variances".
- **Auditors** see My Variances, Discrepancy List, Detect Variances, Records (Masterlist, WFH Schedule), and Attendance.
- **Admins** see everything.

---

### For Employees — View your variances

1. Navigate to **My Variances** (the default landing page).
2. If you see "Sign in to view your private variance report", click **Sign in** in the sidebar and complete Step 5 above.
3. Once signed in, your variance report loads automatically.
4. Click **Refresh** to reload the latest data.

---

### For Admins — Full workflow

#### A. Set up the employee masterlist

1. Go to **Admin → Monthly Attendance (ILC)** and upload an ILC export file (CSV or XLSX). The masterlist is auto-populated from ILC uploads.
2. Alternatively, go to **Admin → Onboard New Employee** and click **＋ Onboard Employee** to add employees manually.

#### B. Configure WFH schedules

1. Go to **Admin → WFH Schedule**.
2. Click **📤 Upload WFH Schedule** and select a CSV or XLSX file.
3. Use the search box to find a specific employee.
4. Edit individual WFH days using the row controls.

#### C. Upload attendance data

1. Go to **Attendance → Monthly Attendance (ILC)**.
2. Click **📤 Upload Monthly Attendance (ILC)** and select the ILC export file.
3. Review the preview, confirm column mappings, and click **Upload**.
4. Repeat for **Attendance → Monthly Attendance (Monday.com)** using the Monday.com export.

#### D. Detect variances

**Monday vs WFH Schedule:**
1. Go to **Discrepancy List → Detect Variances → Monday vs WFH Schedule**.
2. Select the **Year** and **Month** from the toolbar filters.
3. Click **🔍 Detect Variances**.
4. Review the results. Each row shows the employee, WFH days, the date, Monday status, and the variance type.
5. Tag individual rows (or use **TAG ALL**) to mark records for notification.
6. Click **📧 Notify Employees** to send notifications.

**Monday vs ILC:**
1. Go to **Discrepancy List → Detect Variances → Monday vs ILC**.
2. Select the **Year** and **Month**.
3. Click **🔍 Detect Variances**.
4. Review ILC vs Monday discrepancies. Tag and notify as needed.

#### E. Review the discrepancy list

1. Go to **Discrepancy List** (if visible in sidebar, it appears after variances have been detected).
2. Filter by Year, Month, Name, or IBM Serial No.
3. Use **Search** to find a specific employee.

#### F. Review variance submissions (Auditor / Admin)

1. Go to **Variance Review** (visible after sign-in if you are an auditor or admin).
2. Open each pending submission to read the employee's explanation.
3. Approve or reject the submission.

#### G. Manage roles

1. Go to **Admin → Role Maintenance**.
2. Click **＋ Add User** to assign a role to an employee.
3. Select the employee from the masterlist, assign **Admin** or **Auditor**, then click **Save**.
4. Role changes take effect on the employee's next sign-in.

---

## 7. Sidebar Quick Reference

| Section | Item | Who sees it |
|---|---|---|
| Variances | View my Variances | Everyone |
| Variances | Variance Review | Admin, Auditor |
| Admin | Onboard New Employee | Admin |
| Admin | Service Lines | Admin |
| Admin | Masterlist | Admin |
| Admin | WFH Schedule | Admin |
| Admin | Role Maintenance | Admin |
| Discrepancy List | Detect Variances → Monday vs WFH Schedule | Admin, Auditor |
| Discrepancy List | Detect Variances → Monday vs ILC | Admin, Auditor |
| Attendance | Monthly Attendance (ILC) | Admin, Auditor |
| Attendance | Monthly Attendance (Monday.com) | Admin, Auditor |
| Records | Masterlist | Auditor |
| Records | WFH Schedule | Auditor |

---

## 8. File Upload Format Requirements

### ILC Attendance (CSV / XLSX)

Expected columns (case-insensitive):

| Column | Description |
|---|---|
| IBM Serial No. | Employee IBM serial number |
| Name | Employee full name |
| Email | IBM email |
| Weekending | Week-ending date (Saturday) |
| Activity | Activity code (GB, VL, SL, HOL, OHOL, CDO, PRNT, PTNW, UTNW, EMPL) |
| Mon / Tue / Wed / Thu / Fri | Hours worked each day |
| Sat / Sun | Weekend hours |
| Total Hrs | Weekly total hours |
| OT Hrs | Overtime hours |

### Monday.com Attendance (CSV / XLSX)

Expected columns:

| Column | Description |
|---|---|
| IBM Serial No. | Employee IBM serial number |
| Name | Employee full name |
| Email | IBM email |
| Date | Attendance date |
| Group | Monday.com group/service line |
| Status | Attendance status (e.g. WFH, Onsite, Leave) |
| Duration | Hours worked |

### WFH Schedule (CSV / XLSX)

Expected columns:

| Column | Description |
|---|---|
| IBM Serial No. | Employee IBM serial number |
| Name | Employee full name |
| Email | IBM email |
| Days Selection | WFH days (e.g. Monday, Wednesday) |
| WFH Status | Active / Inactive |

---

## 9. Activity Code Legend

| Code | Meaning |
|---|---|
| GB | General Billable |
| VL | Vacation Leave |
| SL | Sick Leave |
| HOL | Designated Holiday |
| OHOL | Optional Holiday |
| CDO | Compensatory Day Off |
| PRNT | Parenting Leave |
| PTNW | Paid Time Not Worked |
| UTNW | Unpaid Time Not Worked |
| EMPL | Pre/Post IBM Employment |

---

## 10. Deploying Updates (Admins / Developers)

### Requirements

- Node.js 20 or newer (only needed for Firebase CLI)
- Firebase CLI installed and authenticated

### Install Firebase CLI (once)

```powershell
npm install -g firebase-tools
firebase --version
```

### Deploy

From the project root folder:

```powershell
firebase login
firebase use seet-variances
firebase deploy --only hosting,firestore
```

The hosted site at `https://seet-variances.web.app` is updated immediately.

### Automated deployment

Pushing to the `main` branch triggers the GitHub Actions workflow in `.github/workflows/` which deploys automatically, provided the Firebase service-account secret is configured in the repository settings.

---

## 11. Troubleshooting

### Cannot sign in

- Confirm your email and password are exactly as provided.
- Passwords are case-sensitive.
- If you forgot your password, ask the administrator to reset it in Firebase Console.

### "Connecting…" never changes in the sidebar

- Check your internet connection.
- Verify you are loading from `https://seet-variances.web.app` or a local server.
- Open browser DevTools (F12) → Console to see any Firebase errors.

### I can sign in but see no data

- Confirm your role has been assigned by an admin in Role Maintenance.
- Role changes take effect on the next sign-in — sign out and sign back in.

### Upload fails or shows no rows

- Check that the file is CSV or XLSX format.
- Confirm the column headers match the expected names (see Section 8).
- Large files (> 5,000 rows) may take a few seconds to parse.

### "Detect Variances" finds nothing

- Confirm both attendance sources (ILC and Monday.com) have been uploaded for the selected month.
- Check the Year and Month filters are set correctly.

### Firebase deployment fails

```powershell
firebase use
firebase projects:list
firebase deploy --only hosting,firestore --debug
```

Confirm you are logged in as an account with access to `seet-variances`.

---

## 12. Security Notes

- Do not share your dashboard password.
- The Firebase configuration embedded in `index.html` is the public client config — it is safe to expose but Firestore rules must remain restrictive.
- Firestore security rules are deployed via `firestore.rules`. Review them before any broader rollout.
- Never commit `.env` files or service-account JSON files to the repository.

---

## License

Internal ODC SEET project. Not for distribution outside the organization.
