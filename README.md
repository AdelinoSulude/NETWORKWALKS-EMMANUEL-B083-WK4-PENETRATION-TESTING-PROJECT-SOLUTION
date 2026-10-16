# NETWORKWALKS Mediroza General Hospital — Pentest Lab

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

## Recommended Repository Structure

```text
NETWORKWALKS-Mediroza-Pentest-Lab/
├── README.md
├── methodology/
│   ├── milestone-1-recon-and-authentication.md
│   ├── milestone-2-pdf-analysis.md
│   └── milestone-3-information-disclosure.md
├── findings/
│   └── findings-and-remediation.md
├── screenshots/
│   └── README.md
├── notes/
│   └── lessons-learned.md
└── .gitignore
```

## Lessons Learned

This exercise demonstrates that security weaknesses rarely exist in isolation. Information gathered during reconnaissance can become relevant later when combined with document metadata, weak authentication, and exposed backups.

A structured penetration test should document the evidence, explain the impact, identify the root cause, and provide actionable remediation guidance.

## Disclaimer

This repository documents educational cybersecurity learning. All testing must remain within the scope of explicit authorization. Do not publish confidential assessment materials, real credentials, recovered personal information, private database records, or sensitive files.

**Program:** NETWORKWALKS Cybersecurity Internship — Batch B083
