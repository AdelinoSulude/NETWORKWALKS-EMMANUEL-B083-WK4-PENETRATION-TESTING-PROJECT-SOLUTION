# NETWORKWALKS Mediroza General Hospital-Pentest Lab Solution

**Program:** NETWORKWALKS Cybersecurity Internship  
**Batch:** B083  
**Week:** 4  
**Project:** Mediroza General Hospital Penetration Testing Lab  
**Status:** Educational lab documentation

## Overview

This repository documents my learning and methodology from the Mediroza General Hospital penetration-testing exercise provided as part of the NETWORKWALKS Cybersecurity Internship.

The assessment follows a multi-stage attack chain to demonstrate how weaknesses in web authentication, file encryption practices, metadata management, and web-server configuration can combine to expose sensitive information.

The purpose of this project is to document technical learning, understand security weaknesses, and identify appropriate remediation measures.

> **Scope and safety:** This repository is for authorized educational lab work. Do not test the target or access, publish, or redistribute sensitive data without explicit authorization. Live-target testing details, credentials, recovered personal information, and database contents are intentionally excluded.

## Learning Objectives

- Understand web reconnaissance and the security implications of `robots.txt`.
- Identify username enumeration vulnerabilities.
- Understand the mechanics and impact of SQL injection.
- Learn about password-protected PDF analysis and password recovery in an authorized lab.
- Inspect document metadata for unintended information disclosure.
- Understand the risks of exposed database backups and directory listing.
- Develop practical remediation recommendations.

## Assessment Methodology

### Milestone 1 — Initial Access

**Topics:** Reconnaissance, username enumeration, SQL injection.

Activities:
- Review publicly exposed web-server information.
- Identify authentication-related weaknesses.
- Assess differences in login error messages.
- Understand how unsafe SQL query construction can compromise authentication.
- Document the potential impact and appropriate remediation.

**Key security lessons:**
- Authentication errors should not reveal whether an account exists.
- SQL queries should use parameterized statements.
- Authentication must be enforced independently of user-controlled input.

### Milestone 2 — PDF Encryption Analysis

**Topics:** File hashing, password auditing, PDF decryption.

Activities:
- Extract PDF password-verification hashes in the authorized lab.
- Understand dictionary-based password auditing.
- Compare the limitations of small and larger wordlists.
- Learn how authorized PDF decryption enables subsequent metadata analysis.

**Tools studied:**
- Networkwalks Hash Calculator
- Networkwalks Password Cracker
- John the Ripper wordlists
- qpdf

**Key security lessons:**
- Common passwords are vulnerable to dictionary attacks.
- Password-protected documents require appropriate password strength and secure handling.
- Recovered passwords and decrypted documents must be treated as sensitive.

### Milestone 3 — Deep Reconnaissance

**Topics:** Metadata analysis, directory listing, backup exposure.

Activities:
- Inspect document metadata in an authorized environment.
- Correlate metadata clues with earlier reconnaissance.
- Understand the security implications of exposed directories.
- Examine database-backup structures using sanitized training data.
- Trace how information disclosure can lead to further compromise.

**Tools studied:**
- ExifTool
- wget
- SQL text inspection
- qpdf

**Key security lessons:**
- Document metadata can reveal internal names, roles, and infrastructure details.
- Directory listing can expose files that were never intended for public access.
- Database backups must not be stored in publicly accessible web directories.
- Database exports should be encrypted, access-controlled, and securely retained.

## Findings and Remediation

| Finding | Risk | Recommended remediation |
|---|---|---|
| Username enumeration | Account discovery | Use consistent authentication error messages and appropriate rate limiting. |
| SQL injection | Authentication bypass or database compromise | Use parameterized queries, safe database APIs, and least-privilege database accounts. |
| Weak document passwords | Unauthorized document access | Enforce strong, unique passwords and secure key management. |
| Sensitive metadata | Internal information disclosure | Sanitize document metadata before publication. |
| Directory listing enabled | Unintended file discovery | Disable directory indexing unless explicitly required. |
| Exposed database backup | Potential disclosure of sensitive records | Remove backups from web roots, restrict access, encrypt backups, and review access logs. |

## Lessons Learned

This exercise demonstrates that security weaknesses rarely exist in isolation. Information gathered during reconnaissance can become relevant later when combined with document metadata, weak authentication, and exposed backups.

A structured penetration test should document the evidence, explain the impact, identify the root cause, and provide actionable remediation guidance.

## Milestone 4 — Penetration Testing Report

### 4.1 Findings Summary

The assessment identified weaknesses across authentication, database query handling, document protection, metadata management, and backup-file exposure.

| # | Vulnerability | Location | Risk |
|---|---|---|---|
| 1 | Username enumeration | `patient/login.php` | Medium |
| 2 | SQL injection leading to login bypass | `patient/login.php` | Critical |
| 3 | Patient reports accessible following authentication bypass | `patient/reports/` | High |
| 4 | Weak PDF passwords susceptible to dictionary attacks | `patient_report_*.pdf` | High |
| 5 | Sensitive metadata in patient PDF files | `patient_report_3.pdf` | Medium |
| 6 | Exposed backup directory with directory listing enabled | `old/` | Critical |
| 7 | Confidential staff salary and shareholder data exposed in a database backup | `old/mediroza_db_backup_2019.sql` | To be assessed |

**Severity note:** Finding 7 may warrant a Critical or High rating depending on the sensitivity and volume of the exposed information, the accessibility of the backup, and the applicable impact criteria. Confirm the rating against the lab's reporting rubric.

### 4.2 Recommendations and Remediation

#### Critical findings

**1. SQL injection and authentication bypass**

- Use parameterized queries or prepared statements for all database operations.
- Never construct SQL statements by directly concatenating untrusted user input.
- Use a database account with the minimum privileges required.
- Implement server-side authentication and authorization checks.
- Log suspicious authentication activity and test the login flow after remediation.

**2. Directory listing and database-backup exposure**

- Disable directory listing on all web-server directories unless explicitly required.
- Remove exposed database backups from the public web root.
- Store backups in restricted locations with appropriate access controls.
- Encrypt backups at rest and restrict access to authorized personnel.
- Review access logs and follow the incident-response process if unauthorized access is suspected.

#### High findings

**3. Patient report access control**

- Store sensitive reports outside the public web root where practical.
- Enforce server-side authentication and per-user authorization for every document request.
- Avoid relying on hidden URLs or the login page alone to protect files.
- Apply appropriate encryption and access controls to sensitive reports.

**4. Weak PDF passwords**

- Use strong, unique passwords where password-protected PDF files are necessary.
- Avoid common passwords and predictable patterns.
- Establish secure password distribution and storage procedures.
- Review whether PDF password protection is appropriate for the sensitivity of the information.

#### Medium findings

**5. Username enumeration**

- Return consistent, generic authentication error messages for invalid usernames and incorrect passwords.
- Apply rate limiting and monitoring to repeated authentication attempts.
- Avoid exposing unnecessary account details through error messages or response differences.

**6. Sensitive PDF metadata**

- Inspect document metadata before distributing sensitive files.
- Remove unnecessary author names, comments, internal paths, and other identifying information.
- Verify the cleaned file before distribution.

For authorized document sanitization, ExifTool supports metadata removal:

```bash
exiftool -all= -o sanitized.pdf filename.pdf
```

Run this on a working copy, not the original evidence. Verify the output metadata and confirm that the document remains readable. Metadata removal does not replace access control or secure handling.

### 4.3 Overall Assessment

The findings illustrate how multiple weaknesses can combine into a broader compromise. Authentication weaknesses may expose protected documents, weak document passwords may enable further inspection, and metadata may reveal operational clues leading to improperly exposed backups.

The primary remediation priorities are to eliminate SQL injection, enforce authorization on sensitive documents, remove public access to database backups, and review whether any confidential information was accessed.

### 4.4 Scope and Responsible Disclosure

This report summarizes an educational penetration-testing exercise. Any further testing must remain within the explicitly authorized lab scope. Sensitive patient information, credentials, private staff records, and raw database backups must not be published in this repository.

## 👤 Author

This CyberLab was created and documented by **Adelino Sulude**
for hands-on cybersecurity practice.

**LinkedIn:** [Adelino Sulude](https://www.linkedin.com/in/adelino-sulude/)

## 🙏 Credits

The training and lab concepts were learned from:

- **Waqas Karim** — Cybersecurity Professional, CCIE
  - Instructor of the cybersecurity training used as a learning reference.
  - **LinkedIn:** [Waqas Karim](https://www.linkedin.com/in/waqaskarim/)

## Disclaimer

This repository documents educational cybersecurity learning. All testing must remain within the scope of explicit authorization. Do not publish confidential assessment materials, real credentials, recovered personal information, private database records, or sensitive files.

**Program:** NETWORKWALKS Cybersecurity Internship — Batch B083
