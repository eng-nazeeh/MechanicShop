# Mechanic Shop Management System

> A clean, scalable, and maintainable workshop operations platform built with **Clean Architecture** to manage customers, vehicles, repair tasks, work orders, scheduling, technician assignments, and operational monitoring.

---

## Overview

The **Mechanic Shop Management System** is designed to digitize and streamline the daily operations of automotive repair shops through a centralized and structured platform.  
It provides end-to-end support for managing customer records, vehicle information, reusable repair task templates, work orders, technician scheduling, service bay allocation, and operational dashboards.

This project was implemented using **Clean Architecture** to ensure clear separation of concerns, business rule integrity, maintainability, and long-term extensibility.

---

## Problem Statement

Many mechanic workshops still rely on fragmented processes or manual tracking methods that make it difficult to:

- Manage customer and vehicle records efficiently
- Track the full lifecycle of repair jobs
- Prevent scheduling conflicts and technician double-booking
- Enforce workshop business rules consistently
- Monitor daily operational performance
- Maintain a single source of truth across the shop

This system addresses these challenges by providing a centralized digital solution for workshop management. :contentReference[oaicite:1]{index=1}

---

## Goals

This project aims to:

- Digitize workshop operations and reduce paper-based workflows
- Prevent scheduling conflicts for technicians and service bays
- Improve work order visibility for managers and technicians
- Standardize repair procedures using reusable repair task templates
- Enable better operational decisions through dashboard metrics
- Increase workshop efficiency and customer satisfaction :contentReference[oaicite:2]{index=2}

---

## Architecture

This project is implemented using **Clean Architecture**, which helps organize the solution around business rules rather than infrastructure details.

### Why Clean Architecture?

Clean Architecture is used in this project to achieve:

- Clear separation between business logic and technical concerns
- Better maintainability as the system grows
- Easier testing of core workflows
- Stronger enforcement of business rules
- Flexibility to change UI, database, or external technologies with minimal impact on the domain layer

### Architectural Benefits in This Project

Because this system includes scheduling rules, work order lifecycle rules, permissions, validation, and operational constraints, Clean Architecture provides the right structure to keep these responsibilities well organized and easy to evolve.

---

## Core Features

## 1. Customer & Vehicle Management

The system allows managers to manage customer accounts and their vehicles through structured CRUD operations.

### Main Capabilities
- Register new customers
- Update customer information
- Delete customers only when they have no associated work orders
- Add vehicles to customer accounts
- Update vehicle details
- Remove vehicles only when they have no active work orders
- Enforce VIN uniqueness across the system

### Why This Matters
This feature centralizes customer and vehicle information, ensuring that every work order is associated with valid and traceable records.

### Problem It Solves
It eliminates scattered customer records, invalid vehicle references, and inconsistent service history management. :contentReference[oaicite:3]{index=3}

---

## 2. Repair Task Catalog

The system includes a reusable repair task catalog that allows managers to define standard service procedures.

### Main Capabilities
- Create repair task templates
- Define task name, description, estimated cost, and estimated duration
- Attach required parts to each repair task
- Update or remove parts from templates
- Reuse the same task template across multiple work orders

### Why This Matters
Standardized repair tasks improve consistency, reduce duplication, and make cost and time estimation more predictable.

### Problem It Solves
It solves the issue of manually redefining the same repair operations repeatedly and helps enforce standardized service procedures across the workshop. :contentReference[oaicite:4]{index=4}

---

## 3. Work Order Management

Work orders represent the operational core of the system. They connect vehicles, repair tasks, technicians, scheduling, and status tracking.

### Main Capabilities
- Create work orders for specific vehicles
- Add one or more repair tasks from the task catalog
- Assign technicians
- Assign service bays
- View summary and detailed work order information
- Update work order status through its lifecycle
- Prevent invalid deletion or invalid state transitions

### Work Order Lifecycle
- `Scheduled`
- `InProgress`
- `Completed`
- `Cancelled`

### Why This Matters
Work order management provides full visibility into repair activities and helps both management and technicians track current and upcoming jobs.

### Problem It Solves
It removes ambiguity in job tracking, prevents inconsistent status changes, and ensures that repair work is managed through a controlled lifecycle. :contentReference[oaicite:5]{index=5}

---

## 4. Scheduling Management

Scheduling is one of the most important components of the system and acts as the main control point for work order timing and service bay allocation.

### Main Capabilities
- Schedule work orders by date and time
- Assign service bays
- View daily schedules
- View schedules by technician
- Reschedule existing work orders
- Cancel scheduled work orders
- Prevent overlaps for both technicians and service bays

### Important Rule
All scheduling operations must be performed **only from the Schedule Page**, ensuring a single point of control and reducing conflict risk.

### Why This Matters
Scheduling directly impacts workshop efficiency, technician utilization, and customer flow.

### Problem It Solves
It prevents double-booking, timing conflicts, and uncontrolled modifications from different parts of the system. :contentReference[oaicite:6]{index=6}

---

## 5. Labor Management

The system supports technician assignment and workload visibility.

### Main Capabilities
- View all technicians
- Check technician availability
- Assign technicians to work orders
- Reassign technicians when needed
- Restrict technicians to viewing only their own assigned work orders

### Why This Matters
Technician availability and controlled assignment are critical for operational continuity and fair workload distribution.

### Problem It Solves
It eliminates unclear labor allocation and prevents technicians from being assigned to overlapping work periods. :contentReference[oaicite:7]{index=7}

---

## 6. Dashboard & Reporting

The platform includes a dashboard for operational monitoring and reporting.

### Main Capabilities
- View total work orders
- Track completed work orders
- Track in-progress work orders
- Track cancelled work orders
- Filter statistics by date range
- Support near-real-time operational visibility

### Why This Matters
Managers need immediate operational insights to make informed decisions.

### Problem It Solves
It replaces manual counting and disconnected reporting with a centralized and data-driven operational overview. :contentReference[oaicite:8]{index=8}

---

## 7. Authentication & Authorization

The system provides secure login and role-based access control.

### Supported Roles
- **Manager**
- **Labor (Technician)**

### Access Model
- Managers have full access to the system
- Technicians have limited access to their assigned work orders and status updates only

### Security Requirements
- Authentication required for all protected pages
- Passwords must never be stored in plain text
- Sessions expire after inactivity
- Failed login attempts should be rate-limited

### Why This Matters
A workshop system contains operational and customer data that must be protected and correctly scoped by role.

### Problem It Solves
It prevents unauthorized access, protects sensitive records, and enforces responsibility boundaries across users. :contentReference[oaicite:9]{index=9}

---

## Business Rules

The system enforces several important business rules, including:

- A customer cannot be deleted if they have existing work orders
- A vehicle cannot be removed if it has active work orders
- VIN must be unique
- Repair task names must be unique
- Estimated duration and estimated cost must be positive
- A work order cannot be created for a non-existent vehicle
- A technician cannot be assigned to overlapping work orders
- A service bay cannot be double-booked
- A work order cannot be deleted once it is in progress
- Technicians can only update their own assigned work orders
- Work orders are automatically cancelled if the customer does not arrive within 15 minutes of the scheduled time :contentReference[oaicite:10]{index=10}

---

## Non-Functional Requirements

This project is also driven by non-functional requirements that shape its technical quality.

### Performance
- Page load time under 2 seconds under normal conditions
- API response time under 500ms for standard queries
- Support for at least 10 concurrent users

### Security
- HTTPS for data in transit
- Secure password storage
- Session timeout after 30 minutes of inactivity
- Audit logging for data modifications

### Usability
- Intuitive user experience
- Clear error messages
- Responsive design for desktop and tablet

### Maintainability
- Consistent code style
- Documented APIs
- Separation of business logic from data access
- Automated tests for critical workflows :contentReference[oaicite:11]{index=11}

---

## User Roles

## Manager
The Manager has full access to the platform and can:

- Manage customers and vehicles
- Manage repair task templates
- Create, reschedule, cancel, and delete work orders
- Assign technicians
- Manage scheduling
- View dashboard metrics and reports

## Labor (Technician)
The Technician has limited access and can:

- View assigned work orders
- Update work order status to `InProgress`
- Update work order status to `Completed` :contentReference[oaicite:12]{index=12}

---

## Out of Scope

The following features are intentionally excluded from the current version:

- Parts inventory tracking
- Payment processing
- Customer communication via email or SMS
- Multi-location support
- Mobile application
- Customer self-service portal
- Third-party integrations
- Advanced reporting and analytics in the initial release :contentReference[oaicite:13]{index=13}

---

## Success Metrics

The PRD defines clear success metrics for the system after launch, including:

- 90% reduction in scheduling conflicts
- 100% work order tracking accuracy
- User adoption rate above 80% within the first month
- Average work order creation time below 2 minutes
- Reduced no-show impact through auto-cancellation
- Better visibility into technician utilization :contentReference[oaicite:14]{index=14}

---

## Why This Project Is Valuable

This project is more than a CRUD application.  
It reflects a real business workflow with scheduling logic, lifecycle transitions, conflict prevention, permission boundaries, auto-cancellation rules, and dashboard monitoring.

Because of that, it is a strong practical example of how **Clean Architecture** can be applied to a business-oriented system with real operational complexity.

---

## Project Highlights

- Real-world workshop domain
- Clean Architecture implementation
- Strong business rule enforcement
- Role-based access control
- Scheduling conflict prevention
- Work order lifecycle tracking
- Reusable repair task templates
- Dashboard-driven operations monitoring

---

## Conclusion

The **Mechanic Shop Management System** is a structured and scalable solution for automotive workshop operations.  
Built with **Clean Architecture**, it provides a maintainable foundation for managing customers, vehicles, repair tasks, work orders, scheduling, technicians, and operational reporting in a professional and extensible way.

It is designed to serve as both a functional business solution and a strong technical example of building a rule-driven system with clean separation of concerns.