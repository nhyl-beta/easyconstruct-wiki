---
tags: [easyconstruct, roles, role/site]
---
# Site Personnel
> [!summary] 🟢 Simple
> The **workers and supervisors on the construction site**. They clock in with their phone (the system checks they're really at the site and takes a photo), update tasks, upload field photos and report problems.

## Features (scope)
Dashboard · Attendance Recording (geofence + live camera photo) · Task Updating · Field Documentation Upload · Requirements Submission · Issue Reporting · Projects (view)

## 🔵 Technical notes
- Clock-in compares phone GPS to the project's site coordinates (default radius **300 m**) → *Inside* / *Outside*; outside = *Flagged* and PM, HR, Admin are alerted instantly. → [[Attendance and Geofencing]]
- **Live-camera capture** (no gallery upload) for photo evidence.
- **Offline queue:** if there's no signal, the clock-in is stored and synced later (`validatedOffline`).
- Attendance is accepted **only in Construction or Closeout** phases.
- Owns gate **K1** (all tasks completed).

Related: [[Human Resources]] · [[Engineer]]
