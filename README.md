# Job Portal & Recruitment Tracking System (MySQL)

## Project Overview

Job Portal & Recruitment Tracking System is an end-to-end MySQL database project that stores and analyzes job seekers, companies, recruiters, job postings, job applications, skills, skill proficiencies, and interview schedules[cite: 1, 2, 3].

The project uses SQL queries, views, stored procedures, and triggers to manage recruitment workflows, summarize application activity, filter candidates and roles, and track application status changes.

**Database:** `job_portal_db`  
**Technology:** MySQL 8.0+  
**Tool:** MySQL Workbench / MySQL CLI  
**Core tables:** 8[cite: 1, 2, 3]

## Project Aim

The main aim of Job Portal & Recruitment Tracking System is to model and manage recruitment operations using MySQL, connecting candidates, recruiters, and companies across job postings, applications, skill matching, and interview rounds[cite: 1, 2, 3]. The system also enforces hiring rules and maintains application status history to support structured recruitment tracking.

## Objectives

- Manage job seeker profiles, experience, education, and skill proficiencies[cite: 1, 2, 3].
- Track companies, recruiters, job postings, compensation ranges, application deadlines, and job statuses[cite: 1, 2, 3].
- Prevent duplicate applications for the same job by the same candidate and block applications for closed jobs or past deadlines.
- Schedule and track interview rounds, modes, interviewers, and results[cite: 1, 2, 3].
- Analyze job demand, candidate counts, salary ranges, and application status distributions using a 24-query SQL bank[cite: 1, 3].
- Demonstrate SQL concepts such as indexes, views, stored procedures, and triggers[cite: 1, 3].

## Database Tables

| Table | Purpose |
|---|---|
| `job_seekers` | Stores candidate name, email, phone, city, experience years, education, and registration timestamp[cite: 1, 2, 3]. |
| `companies` | Stores company name, industry, city, website, and email[cite: 1, 2, 3]. |
| `recruiters` | Stores recruiter name, email, phone, and company reference[cite: 1, 2, 3]. |
| `jobs` | Stores job title, description, location, type, salary range, required experience, posted date, deadline, and status[cite: 1, 2, 3]. |
| `applications` | Connects job seekers and jobs with applied date, application status, and resume file[cite: 1, 2, 3]. |
| `skills` | Stores unique skill names in the skills catalogue[cite: 1, 2, 3]. |
| `seeker_skills` | Maps job seekers to skills along with proficiency level in a many-to-many relationship[cite: 1, 2, 3]. |
| `interviews` | Stores interview date/time, mode, round, interviewer name, and result for applications[cite: 1, 2, 3]. |

## Important Attributes

### 1. `job_seekers`
- `seeker_id` — Primary Key (PK)[cite: 1, 2, 3]
- `full_name`
- `email` — Unique[cite: 1, 2, 3]
- `phone`
- `city`
- `experience_years`
- `education`
- `registered_at`

### 2. `companies`
- `company_id` — PK[cite: 1, 2, 3]
- `company_name`
- `industry`
- `city`
- `website`
- `email` — Unique[cite: 1, 2, 3]

### 3. `recruiters`
- `recruiter_id` — PK[cite: 1, 2, 3]
- `company_id` — Foreign Key (FK)[cite: 1, 2, 3]
- `recruiter_name`
- `email` — Unique[cite: 1, 2, 3]
- `phone`

### 4. `jobs`
- `job_id` — PK[cite: 1, 2, 3]
- `company_id` — FK[cite: 1, 2, 3]
- `recruiter_id` — FK[cite: 1, 2, 3]
- `job_title`
- `job_description`
- `job_location`
- `job_type` (`Full-Time`, `Part-Time`, `Internship`, `Contract`)[cite: 1, 2, 3]
- `salary_min`
- `salary_max` (`CHECK` constraint ensuring `salary_max >= salary_min` when both are non-null)[cite: 1, 3]
- `experience_required`
- `posted_date`
- `application_deadline`
- `job_status` (`Open`, `Closed`, `On Hold`)[cite: 1, 2, 3]

### 5. `applications`
- `application_id` — PK[cite: 1, 2, 3]
- `seeker_id` — FK[cite: 1, 2, 3]
- `job_id` — FK[cite: 1, 2, 3]
- `applied_date`
- `application_status` (`Applied`, `Under Review`, `Shortlisted`, `Interview`, `Selected`, `Rejected`)[cite: 1, 3]
- `resume_file`
- `UNIQUE (seeker_id, job_id)` constraint[cite: 1, 3]

### 6. `skills`
- `skill_id` — PK[cite: 1, 2, 3]
- `skill_name` — Unique[cite: 1, 2, 3]

### 7. `seeker_skills`
- `seeker_id` — Composite PK, FK[cite: 1, 2, 3]
- `skill_id` — Composite PK, FK[cite: 1, 2, 3]
- `proficiency_level` (`Beginner`, `Intermediate`, `Advanced`)[cite: 1, 2, 3]

### 8. `interviews`
- `interview_id` — PK[cite: 1, 2, 3]
- `application_id` — FK[cite: 1, 2, 3]
- `interview_date`
- `interview_mode` (`Online`, `In-Person`, `Phone`)[cite: 1, 2, 3]
- `interview_round`
- `interviewer_name`
- `interview_result` (`Pending`, `Passed`, `Failed`)[cite: 1, 2, 3]

**Note:** `applications` links `job_seekers` and `jobs` with a `UNIQUE (seeker_id, job_id)` constraint so a candidate cannot submit duplicate applications for the same job posting[cite: 1, 3]. Meanwhile, `seeker_skills` acts as a junction table with a composite primary key `(seeker_id, skill_id)` and a `proficiency_level` attribute[cite: 1, 2, 3].

## Entity Relationships & ER Diagram

![Job Portal Recruitment Tracking System ER Diagram](./ER%20Diagram.png)[cite: 2]

- `companies` → `recruiters`: One-to-many (1:N) — a company employs recruiters[cite: 1, 2, 3].
- `companies` → `jobs`: One-to-many (1:N) — a company posts jobs[cite: 1, 2, 3].
- `recruiters` → `jobs`: One-to-many (1:N) — a recruiter manages job postings[cite: 1, 2, 3].
- `job_seekers` → `applications`: One-to-many (1:N) — a job seeker submits applications[cite: 1, 2, 3].
- `jobs` → `applications`: One-to-many (1:N) — a job receives applications[cite: 1, 2, 3].
- `job_seekers` ↔ `skills` (via `seeker_skills`): Many-to-many (M:N) — candidates have skills with specific proficiency levels[cite: 1, 2, 3].
- `applications` → `interviews`: One-to-many (1:N) — interviews are scheduled for applications[cite: 1, 2, 3].

PK means Primary Key, FK means Foreign Key, 1:N means one-to-many, and M:N means many-to-many[cite: 2].

## Main SQL Analyses & Database Objects

- **Query Bank (24 Queries across 4 Levels):**
  - *Level 1 (Basic Select):* List all job seekers or jobs, project candidate names and emails, and retrieve distinct candidate cities[cite: 1, 3].
  - *Level 2 (Filtering & Sorting):* Filter open jobs, jobs in a given city, candidates with zero experience, maximum salary thresholds or ranges, latest posted jobs, and top 5 recent registrations[cite: 1, 3].
  - *Level 3 (Joins & Aggregates):* Join jobs with companies and applications with candidates/jobs, count total candidates and applications per job, compute average minimum salary per company, and find candidates with a specific skill such as `SQL`[cite: 1, 3].
  - *Level 4 (Advanced Reports):* Find candidates with more than one application (`HAVING COUNT > 1`), identify jobs with no applications (`LEFT JOIN ... IS NULL`), find highest-experience candidates via subquery, count applications per company, view interview schedules, inspect application status history, and filter companies with at least two applications[cite: 1, 3].
- **Views (4):** `view_job_details`, `view_application_details`, `view_job_application_count`, and `view_candidate_skills`[cite: 1, 3].
- **Stored Procedures (4):** `sp_get_seeker_applications`, `sp_get_job_applicants`, `sp_application_status_report`, and `sp_update_application_status`[cite: 1, 3].
- **Triggers (3):**
  - `trg_application_created` (`AFTER INSERT` on `applications`) — records new applications into `application_status_history`[cite: 1, 3].
  - `trg_application_status_update` (`AFTER UPDATE` on `applications`) — records status changes into `application_status_history`[cite: 1, 3].
  - `trg_check_job_before_application` (`BEFORE INSERT` on `applications`) — raises SQLSTATE `'45000'` if the job status is not `'Open'` or if `applied_date > application_deadline`[cite: 1, 3].
- **Indexes (9):** `idx_seekers_city`, `idx_jobs_company`, `idx_jobs_status`, `idx_jobs_location`, `idx_jobs_posted_date`, `idx_applications_job`, `idx_applications_seeker`, `idx_applications_status`, and `idx_interviews_date`[cite: 1, 3].

## SQL Concepts Used

- `CREATE DATABASE`, `DROP DATABASE`, and `CREATE TABLE`[cite: 1, 3]
- Primary keys, composite primary keys, foreign keys, `UNIQUE`, and `CHECK` constraints[cite: 1, 2, 3]
- `SELECT`, `DISTINCT`, `WHERE`, `BETWEEN`, `ORDER BY`, and `LIMIT`[cite: 1, 3]
- `JOIN` and `LEFT JOIN`[cite: 1, 3]
- `GROUP BY` and `HAVING`[cite: 1, 3]
- `COUNT()`, `AVG()`, and scalar subqueries (`MAX()`)[cite: 1, 3]
- Indexes[cite: 1, 3]
- Views (`CREATE OR REPLACE VIEW`)[cite: 1, 3]
- Stored procedures (`DELIMITER`, `IN` parameters, `CALL`)[cite: 1, 3]
- Triggers (`BEFORE INSERT`, `AFTER INSERT`, `AFTER UPDATE`, `SIGNAL SQLSTATE`) and audit records[cite: 1, 3]

## How to Run the Project

1. Install MySQL Server 8.0 or later[cite: 1, 3].
2. Place `ER Diagram.png` and your SQL script file in your project directory[cite: 1, 2, 3].
3. Open MySQL Workbench (or MySQL CLI) and connect to your server.
4. Create the `application_status_history` audit table if you plan to run the insert/update audit triggers in Part 6[cite: 1, 3]:

   ```sql
   USE job_portal_db;
   CREATE TABLE IF NOT EXISTS application_status_history (
       history_id INT AUTO_INCREMENT PRIMARY KEY,
       application_id INT NOT NULL,
       old_status VARCHAR(30),
       new_status VARCHAR(30) NOT NULL,
       changed_at DATETIME DEFAULT CURRENT_TIMESTAMP,
       FOREIGN KEY (application_id) REFERENCES applications(application_id)
   );




# LIMITATIONS

* The sample data is illustrative sample data for practice and demonstration, not production recruitment data.
* Query results, counts, and reports depend on the sample records currently present in the database.
* The application_status_history table is referenced by the triggers in Part 6 (trg_application_created and trg_application_status_update) and by query Q23 in Part 10, but its CREATE TABLE statement is not included in Part 1 of the script.
* The trg_check_job_before_application trigger blocks inserts when job_status is not 'Open' (such as 'Closed' or 'On Hold') or when applied_date is after application_deadline.
* The project is a standalone MySQL script and query bank rather than a complete web application backend or frontend interface.


# FUTURE ENHANCEMENTS

* Add the CREATE TABLE definition for application_status_history in Part 1 so the audit triggers and Q23 run out of the box.
* Expand the sample dataset across more cities, companies, recruiters, job roles, skill proficiencies, and interview outcomes.
* Add stored procedures or queries for skill-to-job matching, salary range filtering, and recruiter or interview performance summaries.
* Connect the MySQL database to a backend API or analytics dashboard for interactive recruitment tracking.
  
 # Conclusion:-
    The Job Portal & Recruitment Tracking System demonstrates how MySQL 8.0+ can structure end-to-end hiring data across job seekers, companies, recruiters, job vacancies, applications, skills, skill proficiencies, and interview schedules. By combining an eight-table relational schema with primary and foreign keys, unique constraints, and salary checks, the database maintains data integrity across the entire recruitment workflow.   Beyond foundational data storage, the project showcases practical database engineering through indexes for faster lookups, reusable views for simplified reporting, stored procedures for routine operations and status updates, and triggers that enforce open-job and deadline rules while recording application status history. Paired with a 24-query analytical bank spanning basic retrieval, filtering, joins, aggregations, and advanced reports, it serves as a structured, hands-on MySQL project for recruitment data management and SQL practice.   
