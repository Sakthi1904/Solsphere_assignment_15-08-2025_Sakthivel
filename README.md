# 🛡️ **System Health Dashboard(Solsphere Assignment: Cross-Platform System Utility + Admin Dashboard)**

A cross-platform utility for monitoring and managing system health across macOS, Windows, and Linux.

- 🐍 **Client Agent** (Python): Collects machine health data.
- 🐣 **Backend API** (Flask + SQLite): Stores, processes, and serves machine data.
- ⚛️ **Frontend Dashboard** (React + Vite): Displays machine health in an interactive UI with filters, search, and CSV export.

## 📸 **Screenshot**

![Admin Dashboard UI](frontend/public/dashboard.png)

---

## 🚀 **Key Features**

- 🔒 **Disk Encryption Status**  
  Ensures that data is protected by disk encryption.

- 🛠️ **OS Update Status**  
  Displays if the system's OS is up-to-date, with platform-specific checks.

- 🛡️ **Antivirus Presence & Status**  
  Checks whether antivirus software is installed and actively protecting the system.

- 🌙 **Sleep Timeout Settings**  
  Verifies that system sleep settings are configured for productivity, ensuring no longer than 10 minutes of inactivity.

- ⏰ **Last Check-in Timestamp**  
  Timestamps when the last data check was sent.

- 🔍 **Filters & Sorting**  
  Filter machines based on OS, issues, or search by machine ID.

- 🕵️‍♂️ **Search by Machine ID**

- 🔄 **Manual Refresh & CSV Export**  
  Pull the latest data with a manual refresh and export it in CSV format.

- 📊 **System Health Counts (Optional)**  
  Optional display of system health metrics (e.g., number of machines with issues).

---

## ✨ **System Overview**

- **Real-time Data Flow:**  
  - The client agent sends periodic updates to the server at a set interval (15-60 minutes) when data changes.  
  - The backend stores the system data in an SQLite database and serves it to the frontend UI.
  - **API Schema:**  
    The backend accepts a standardized JSON payload with the following fields:  
    `machine_id`, `os`, `disk_encryption`, `os_update`, `antivirus`, `sleep_settings`, `timestamp`.

- **Security-First Design:**  
  - API access requires an API key for authentication (currently using a basic `X-API-Key` header).  
  - **Next steps for production**: Implement HTTPS, secret rotation, JWT/OAuth, and Role-Based Access Control (RBAC).

- **Cross-Platform Client (Python):**  
  - The Python agent works on Windows, macOS, and Linux.  
  - Each platform has platform-specific checks for updates, disk encryption, antivirus, and sleep settings.

- **Daemon Implementation:**  
  The client agent runs as a **background daemon**, periodically checking the system state at a configurable interval (15–60 minutes). It sends updates to the server only when a change in the system state is detected, ensuring minimal resource consumption.
  
---

## 📁 **Repository Layout**

- `backend/` Flask API server.
- `client/` Python agent.
- `frontend/` React (Vite) dashboard UI.

## 🛠️ Prerequisites

- Python 3.9+
- Node.js 18+
- Windows/macOS/Linux as target clients

## ⚙️ Setup

### 1) Backend (Flask)

From `backend/`:

```bash
pip install -r requirements.txt  
python app.py
```

- Runs on http://localhost:5000
- Endpoints:
  - `POST /report` (requires header `X-API-Key: StrongInterviewKey`)
  - `GET /machines`

### 2) Client (Python agent)

From project root or `client/`:

```bash
pip install -r requirements.txt  
python client/main.py
```

- Sends data to the backend every 15 minutes if changes are detected.
- Fields sent: `machine_id, os, disk_encryption, os_update, antivirus, sleep_settings, timestamp`.

#### Platform-Specific Notes:

🪟 Windows:
- Uses PowerShell commands for antivirus and sleep settings.


🍎 macOS:
- Utilizes softwareupdate and pmset for system checks.

🐧 Linux:
- Uses apt and gsettings to fetch update and sleep settings.

### 3) Frontend (React/Vite)

From `frontend/`:

```bash
npm install
npm run dev
```

- The dashboard UI points to http://localhost:5000/machines to fetch and display data.
- Key UI actions:
  - Filter by OS and issue.
  - Search by machine ID.
  - Refresh for the latest status.
  - Export filtered data to CSV.

## 🗂️ API Response Data Model

Here’s the structure of a typical machine response:

```json
{
  "machine_id": "MACHINE-123",
  "os": "Windows",
  "disk_encryption": true,
  "os_update": { "current": "10.0.19045", "latest": "Up to date" },
  "antivirus": { "installed": true, "active": true },
  "sleep_settings": { "timeout_minutes": 10 },
  "timestamp": "2025-08-14T12:34:56.789Z"
}
```

## 🔍 Frontend Filtering Logic

- You can filter and display machines based on these issues:
  - Disk encryption off.
  - OS update `latest` contains "update available" (case-insensitive).
  - Antivirus not installed or not active.
  - Sleep timeout > 10 minutes.
`.

## 🔐 Security

- For demo purposes, an API key (X-API-Key) is required for communication.


## 👨‍💻 Author

**Sakthivel C**  
[GitHub Profile](https://github.com/Sakthi1904)


