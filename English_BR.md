# 📋 Business Requirements Document

## Employee Leave, Absence \& Coverage Management System

> **Business Requirements Document (BRD)**  
> This document defines the system scope, operational requirements, and approved business rules before the design and development phases.

---

## 📌 1. Project Overview

The project aims to develop a desktop application for managing employees, organizing weekly leave schedules, handling unexpected absences, and coordinating employee coverage across different work locations.

The system will help the manager monitor employee distribution across locations and shifts, detect scheduling conflicts before approving changes, and manage employee transfers, exchanges, and coverage assignments while maintaining records of important modifications and administrative decisions.

### 🎯 Main Objectives

- Organize employees' weekly leave schedules.
- Track and manage unexpected employee absences.
- Facilitate employee coverage between different locations.
- Manage employee transfers and exchanges.
- Prevent unauthorized operational conflicts.
- Notify the manager when issues require administrative intervention.
- Maintain records of schedule modifications and important decisions.

---

## ⚙️ 2. System Environment

| Item | Description |
| --- | --- |
| Application Type | Desktop Application |
| Proposed Technology | C# WinForms |
| Database | SQL Server |
| Number of Managers | One |
| Operating Devices | One Computer |
| Calendar System | Gregorian |
| Week Starts | Saturday |
| Week Ends | Friday |

The system is designed to be managed by a single administrator who has full control over all locations and shifts.

---

## 🏢 3. Locations \& Shifts

The system consists of multiple work locations. Each location operates 24 hours a day through three shifts:

- ☀️ Morning Shift
- 🌆 Evening Shift
- 🌙 Night Shift

### Employee Assignment Rules

- Each employee has one primary location.
- Each employee has one primary shift.
- All employees have equal capabilities and can be considered for coverage.
- Leave limits are managed independently for each location and shift.
- Each location has a configurable maximum leave limit for each shift.

---

## 👥 4. Employee Management

The system allows the manager to add employees, update their information, and manage their employment status.

### Basic Rules

- Employee IDs are generated automatically by the database.
- Each new employee must be assigned a primary location, shift, and weekly leave day.
- The system must validate leave limits before saving a new employee.
- When an employee leaves, their status changes to `Inactive` instead of permanently deleting their record.
- Inactive employees cannot be selected for coverage or assigned to future operations.
- Employee information and historical records must be preserved.

---

## 📅 5. Leave Schedule Management

### Scheduling Rules

- Each employee's primary weekly leave day repeats automatically every week.
- The manager can change an employee's leave day permanently or temporarily.
- Individual employee schedules are displayed monthly.
- All employees' schedules are displayed weekly, organized by location and shift.
- The system maintains the current schedule and the upcoming schedule.
- The next month's schedule is generated automatically at the beginning of each month.

### Schedule Updates \& History

- Every approved modification must be reflected in the current schedule immediately.
- The system must preserve a beginning-of-month schedule record.
- All schedule modifications must be recorded.
- Previous changes must remain traceable without losing their history.

---

## 🚨 6. Unexpected Absence Management

The system allows the manager to register unexpected employee absences.

### Absence Registration Rules

- Each absence record applies to one day only.
- Multiple employees can be registered as absent on the same day.
- The system must not allow absence registration for previous dates.
- An absence cannot be registered on the employee's weekly leave day.
- An employee with an approved coverage assignment on the same day cannot be registered as absent.
- The manager can cancel a registered absence.

### Absence Cancellation

When an absence is cancelled:

- Its associated coverage assignment is automatically cancelled, if one exists.
- The employee assigned to provide coverage returns to their original schedule for that day.

---

## 🔄 7. Employee Coverage Management

Coverage means assigning an employee to work in place of an absent employee for one day.

### When Is Coverage Required?

The system calculates the number of unavailable employees using the following rule:

**Unavailable Employees = Employees on Weekly Leave + Absent Employees.**

If an absence does not cause the maximum allowed leave limit to be exceeded, coverage is not required.

If the absence causes the limit to be exceeded, the system starts identifying eligible employees for coverage.

### Coverage Rules

- Coverage cannot be assigned across different shifts.
- The replacement employee must belong to a different location.
- An employee cannot cover another employee from the same location.
- An employee cannot be assigned coverage on their weekly leave day.
- An absent employee cannot be selected for coverage on the same day.
- An employee already assigned to another coverage task on the same day cannot be selected.
- Each employee can cover only one person per day.
- An employee undergoing a transfer is unavailable for coverage during the transfer period.

### Employee Selection Process

- The system displays a list of eligible employees.
- The manager manually selects the replacement employee.
- Once approved, the coverage assignment becomes mandatory.
- The manager can change or cancel the assignment.
- When a coverage assignment is cancelled, the employee returns to their original schedule for that day.

### When No Suitable Employee Is Available

If no suitable replacement is available, the manager may authorize exceeding the maximum leave limit for one day only.

This is an exceptional administrative decision, not an automatic system operation.

The system must also consider the impact of assigning an employee from another location on the leave limit of that employee's original location and shift.

---

## 🚚 8. Employee Transfer Management

A transfer means moving an employee from one location to another without requiring another employee to move in the opposite direction.

### Transfer Types

- Permanent Transfer
- Temporary Transfer

### Temporary Transfer Rules

A temporary transfer must have a defined expiration date.

When the expiration date arrives:

1. The system notifies the manager.
2. The system does not automatically return the employee to their original location.
3. The manager decides how to proceed:
   - Confirm the employee's return.
   - Extend the transfer period.
   - Make the transfer permanent.

### Validation Before Return or Extension

The system must check the impact of the decision on leave limits and coverage availability across all affected locations and shifts.

If a conflict is detected, the system notifies the manager and leaves the administrative decision to them, such as changing another employee's leave day.

---

## 🔁 9. Employee Exchange Management

An employee exchange means exchanging both locations and shifts between two or more employees.

Unlike a transfer, an exchange involves a mutual movement between the participating employees.

### Example

Before the exchange:

- Ahmed: Location A — Morning Shift.
- Mohammed: Location B — Evening Shift.

After the exchange:

- Ahmed: Location B — Evening Shift.
- Mohammed: Location A — Morning Shift.

### Exchange Rules

- An exchange can involve two or more employees.
- It can be permanent or temporary.
- A temporary exchange must have a defined expiration date.
- All participating employees, locations, and shifts must be validated before approval.
- The entire operation must be atomic: either all changes succeed or none of them are applied.

### Temporary Exchange Expiration

When the expiration date arrives:

- The system notifies the manager.
- Employees are not automatically returned to their previous assignments.
- The manager decides whether to restore the original assignments, extend the exchange, or make it permanent.
- The impact of the decision on leave limits and coverage must be validated before approval.

---

## 🛡️ 10. Conflict Detection \& Validation Rules

One of the system's main responsibilities is detecting scheduling conflicts before approving operations.

### Operations Requiring Validation

- Adding a new employee.
- Changing an employee's leave day.
- Transferring an employee.
- Exchanging employees.
- Assigning employee coverage.
- Ending a temporary transfer or exchange.

### Conflict Handling

- The system must validate all affected locations and shifts.
- A notification must be displayed immediately when a conflict is detected.
- Normal operations that result in unauthorized conflicts must not be saved.
- For exceptional cases requiring administrative intervention, the system displays a warning and allows the manager to make the appropriate decision.
- The system must not make administrative decisions automatically on behalf of the manager.

---

## 📝 11. Modification \& Decision History

The system must maintain records of important changes made to schedules and employee assignments.

These records include:

- Changes to weekly leave days.
- Employee transfers.
- Employee exchanges.
- Absence registration and cancellation.
- Coverage assignments, modifications, and cancellations.
- Administrative decisions related to scheduling conflicts.

The purpose is to ensure traceability and preserve the history of important operational changes.

---

## 🧩 12. Project Scope

The current project scope includes the following functionalities:

- Employee Management.
- Location \& Shift Management.
- Weekly Leave Management.
- Current and Upcoming Schedule Management.
- Unexpected Absence Management.
- Employee Coverage Management.
- Employee Transfer Management.
- Employee Exchange Management.
- Conflict Detection \& Notifications.
- Modification \& Administrative Decision History.

---

## 📌 13. Core System Principle

> The system is designed to assist the manager in daily operations and identify potential problems before they occur. It is not a replacement for administrative decision-making.

The system validates operations, detects conflicts, and prevents unauthorized changes. However, the manager retains full authority over exceptional situations that require administrative intervention.

---

**Document Type:** Business Requirements Document (BRD)  
**Project Status:** Requirements Definition  
**Version:** 1.0
