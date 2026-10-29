
# Crownfield Eye Hospital database — starter files

Module: CPU4:103, Introduction to Database (GBS/BSU)
Technology: MySQL Server **8.0.16 or newer**; run commands through **MySQL Workbench**.

This package is a starter database for a **fictional teaching scenario**. The first five appointments reproduce the supplied Appendix 1 records (with labels and clearly indicated assumptions); all additional appointments are fabricated. It is **not a live clinical system** and the prescription fixtures are not medical advice.

## Contents
- `01_schema.sql`: tables, PKs, FKs, unique fields, checks and indexes.
- `02_sample_data.sql`: insert statements (including source rows and synthetic examples).
- `03_explore_queries.sql`: beginner queries for practice.
- `patients_15.csv`: quick view of the patient records.

## First run in MySQL Workbench
1. Make sure MySQL Server is running, then open **MySQL Workbench** and connect to your local MySQL instance.
2. Choose **File → Open SQL Script** and open `01_schema.sql`.
3. Click the **lightning bolt** to run the whole script. The script creates the `crownfield_eye_hospital` database.
4. Open `02_sample_data.sql` and click the lightning bolt again to insert the data.
5. Open `03_explore_queries.sql`. Select a single SELECT statement and click the lightning bolt to run it.
6. In the `SCHEMAS` panel, right-click and **Refresh All** to show the new database/tables.

**Warning:** the schema script drops and rebuilds its eleven named tables each time you run it. If you add work you want to keep, do not rerun `01_schema.sql` without backing it up first. Running the data script twice without rebuilding will produce duplicate-key errors.

## Verification queries
```sql
USE crownfield_eye_hospital;
SELECT COUNT(*) FROM patient;      -- expect 15
SELECT COUNT(*) FROM appointment;  -- expect 15
SELECT data_origin, COUNT(*) FROM appointment GROUP BY data_origin;
-- expect Appendix1 = 5, Synthetic = 10
```

## Eleven connected tables
Specialty -> Clinician -> Appointment <- Patient
Clinic -> Appointment
Appointment -> AppointmentDiagnosis <- Diagnosis
Appointment -> Prescription -> PrescriptionItem <- Medicine
Appointment -> FollowUp (optional maximum one)

## How the source data was handled
- Appendix 1 has **5 encounters for 4 distinct patients**, including OP001 (Amina Yusuf) twice.
- The source 'Latanoprost | Timolol' became two prescription items in RX001.
- Source entry 'None' for Sofia Bennett did not create any prescription row.
- The source gave no doses, routes, frequencies or durations, so these original prescription item fields are `NULL`.
- Original appointment statuses were absent from the source; past dates were *assumed* Completed and the future appointment Scheduled.
- Source follow-up intervals were converted into calculated due dates; all notes retained.
- The extra example names, clinics, conditions and prescriptions are **synthetic classroom fixtures**, not medical instructions.

## Counts (examples)
Patient 15, Appointment 15, Prescription 14, PrescriptionItem 15, FollowUp 15,
Clinician 13, Clinic 13, Specialty 13, Medicine 14, Diagnosis 14.
Each main table includes at least 10 synthetic additions beyond the original source records.

## Next assignment work you must independently verify and explain
This is not the whole assignment: you still need to create/explain the Crow's Foot ER diagram,
normalisation steps to 3NF, your own Part C SQL tasks (including procedure and trigger),
access roles/permissions, a meaningful protection method, transaction testing and real
MySQL screenshots showing successful results. Keep an individual GitHub repository with
ordered scripts and a README. Check the exact rubric before submission.

GitHub contribution attribution: identify which code changes and tests you personally executed.

