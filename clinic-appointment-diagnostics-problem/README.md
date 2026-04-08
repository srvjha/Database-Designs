# Clinic Management System – ER Design (MVP)

## Overview

This project defines the database design for a modern clinic system that manages:

- Patients
- Doctors
- Appointments
- Consultations (visits)
- Diagnostic tests
- Reports
- Payments

The goal of this design is to model real-world clinic workflows in a clean, scalable, and query-friendly way while keeping the system simple enough for an MVP.

---

## Core Concepts

The system is built around a **patient journey**:

Patient → Appointment → Consultation → Tests → Reports → Payment

Key principles:
- Separate **booking (appointment)** from **actual visit (consultation)**
- Keep **diagnostics structured and traceable**
- Allow **flexibility for real-world cases** like no-shows, walk-ins, and delayed reports

---

## Entities

### PATIENT
Stores basic patient information.

**Key Points:**
- A patient can have multiple appointments, consultations, and payments
- Acts as the root entity for most queries

---

### DOCTOR
Represents doctors in the clinic.

**Key Points:**
- Each doctor handles multiple appointments and consultations
- Specialty is stored as a simple attribute (flattened for MVP)

---

### APPOINTMENT
Represents a scheduled interaction between a patient and a doctor.

**Key Points:**
- Captures booking intent
- Tracks status (scheduled, cancelled, completed, no-show)
- Does not guarantee a consultation will happen

---

### CONSULTATION
Represents the actual visit between patient and doctor.

**Key Points:**
- May or may not be linked to an appointment (supports walk-ins)
- Stores diagnosis and prescription details
- Acts as the central entity for medical interactions

---

### TEST
Represents a catalog of diagnostic tests.

**Key Points:**
- Reusable across the system
- Contains metadata like name, description, and price

---

### CONSULTATION_TEST
Represents tests prescribed during a consultation.

**Key Points:**
- Resolves many-to-many between consultation and test
- Tracks lifecycle: prescribed → completed
- Allows storing instructions and scheduling info

---

### REPORT
Represents the result of a diagnostic test.

**Key Points:**
- Linked to a specific test instance (not directly to consultation)
- Supports delayed generation
- Stores result summary and file reference

---

### PAYMENT
Handles financial transactions.

**Key Points:**
- Linked to patient
- Can optionally be linked to:
  - consultation (main billing)
  - appointment (booking fee)
- Supports multiple payments per entity (partial payments, retries)

---

## Relationships and Cardinality

### Patient Relationships
- One patient can have many appointments
- One patient can have many consultations
- One patient can have many payments

---

### Doctor Relationships
- One doctor can handle many appointments
- One doctor can perform many consultations

---

### Appointment to Consultation
- One appointment can result in zero or one consultation
- Models real-world cases like cancellations and no-shows

---

### Consultation to Tests
- One consultation can have many prescribed tests
- Each test instance belongs to one consultation

---

### Test Usage
- One test type can be used in many consultations
- Enables reuse and standardization

---

### Test to Report
- One test instance can have zero or one report
- Supports delayed or pending results

---

### Payments
- One consultation can have multiple payments
- One appointment can have multiple payments (optional use case)

---

## Design Decisions

### 1. Appointment vs Consultation Separation
Appointments represent intent, while consultations represent actual events. This avoids issues with no-shows and supports walk-ins.

---

### 2. Diagnostic Modeling
Tests are modeled using a catalog (TEST) and a join table (CONSULTATION_TEST). This ensures:
- scalability
- reuse
- proper tracking of test lifecycle

---

### 3. Report Linking
Reports are linked to specific test instances instead of consultations to avoid ambiguity when multiple tests are involved.

---

### 4. Payment Flexibility
Payments are loosely coupled to support:
- consultation billing
- appointment booking fees
- partial or multiple transactions

---

### 5. Controlled Denormalization
Consultation stores `patient_id` and `doctor_id` directly to avoid unnecessary joins during frequent queries.

---

## Limitations (MVP Scope)

- Diagnosis is stored as free text (not normalized)
- Doctor specialty is not a separate entity
- No support for multiple doctors per appointment
- No audit/history tracking for updates

---

## Future Improvements

- Introduce structured diagnosis entity
- Normalize specialties (support multiple specialties per doctor)
- Add billing/invoice abstraction
- Introduce role-based access control
- Add audit logs and soft deletes

---

## Summary

This design balances:
- simplicity (for MVP)
- correctness (real-world mapping)
- scalability (future extensibility)

It avoids common pitfalls like:
- merging appointments with consultations
- attaching tests directly to patients
- over-denormalizing early