# Tala: Staff, Meeting, and Activity Tracker

An AppSheet app for a campus publication. It keeps the staff, meetings, attendance, and activities in one place, replacing scattered notes and chat messages.

CpE 118 midterm project, 1st Semester SY 2026-2027
Author: Rochelle Ovalo | Section: BSCpE-2A

*Tala* is Filipino for "star".

## Links

- **AppSheet app:** https://www.appsheet.com/Template/AppDef?appName=Tala-122959370&utm_source=share_app_link
- **Google Sheet (view access):** https://docs.google.com/spreadsheets/d/143H2f5rD54zxncRYn-i_W-406FTtMjTL/edit?usp=sharing&ouid=112542240242693169847&rtpof=true&sd=true

## Who uses it

- **Editor in Chief:** maintains all records (add, edit, delete).
- **Staff members:** view all data and add their own attendance and participation records.

Access is limited to the publication's members through the Google (Gmail) account listed in the Staff table.

## Features

- Create, read, update, and delete records on every table
- Dashboard with totals and charts that update when data changes
- Names shown instead of ID numbers (position title, staff name, meeting title, activity name)
- Primary keys auto-incremented and hidden from users
- Data rules: required fields, not-null foreign keys, and a fixed list of attendance statuses

## Database design

The data is stored in a Google Sheet as six normalized tables.

| Table | Purpose |
|---|---|
| Position | Roles in the publication |
| Staff | Members: name, course, year level, section, position, email, phone |
| Meeting | Title, date, times, type, location |
| Publication Activity | Activities the staff work on |
| Attendance | Junction table between Staff and Meeting (status: Present, Late, Absent, Excused) |
| Activity Participation | Junction table between Staff and Publication Activity (time in, time out, hours) |

### Relationships

- One Position has many Staff.
- One Staff member has many Attendance records, and one Meeting has many Attendance records (many-to-many between Staff and Meeting, resolved by Attendance).
- One Staff member has many Activity Participation records, and one Publication Activity has many participation records (many-to-many between Staff and Publication Activity, resolved by Activity Participation).

### ERD

<img src="OVALO_Rochelle_W8_ERD%20(1).png" alt="Tala ERD" width="900">

The diagram source is in [`erd.dbml`](erd.dbml). Paste it into [dbdiagram.io](https://dbdiagram.io) to view or edit it.

## Dashboard

- **Total Staff, Total Meetings, Total Activities:** COUNT of rows in each table
- **Attendance chart:** COUNT of attendance records grouped by status
- **Work Hours chart:** SUM of hours rendered, grouped by staff member
- **Recent Meetings:** latest meetings, newest first

All dashboard views are read-only.

## Denormalized views

The data is stored in separate tables to avoid repetition, and the app joins it back together for reading. For example, the Staff screen shows the position title instead of PositionID, and the Attendance and Participation screens show staff, meeting, and activity names instead of IDs.

## How to use

1. Open the app link and sign in with your Google account (it must be listed in the Staff table).
2. Open the Dashboard for totals and charts. Tap the refresh icon to sync.
3. Use the menu to open Staff, Position, Activity, Attendance, Meeting, or Participation.
4. Tap **+** to add a record where your role allows it.

## Repository contents

| File | Description |
|---|---|
| `README.md` | This file |
| `erd.dbml` | ERD source for dbdiagram.io |
| `OVALO_Rochelle_W8_ERD (1).png` | Final ERD image |

## Known limitations

- Permissions are enforced inside the app. Anyone with edit access to the Google Sheet could still change the data, so the sheet is shared as view-only.
- The automatic ID numbers could collide if two people add a record at the same moment.
- The data in the sheet is sample data for the class project.
