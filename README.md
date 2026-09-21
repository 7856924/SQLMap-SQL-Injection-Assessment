# SQL Injection Assessment & Automated Database Enumeration

## 📌 Executive Summary
This repository contains an end-to-end technical walkthrough of a SQL Injection (SQLi) assessment. The evaluation was performed on a legally authorized, deliberately vulnerable testbed (`://vulnweb.com`) using enterprise-grade testing frameworks. This project tracks the entire assessment lifecycle, moving from traffic interception to backend architectural mapping, schema enumeration, and structured data recovery.

## 🛠️ Security Tool Stack
*   **Operating System:** Kali Linux
*   **Interception Proxy:** Burp Suite
*   **Exploitation Engine:** SQLMap Automated Injection Framework

## 🎯 Target Overview
*   **Target Domain:** `://vulnweb.com`
*   **Vulnerable Endpoint:** `/Login.asp`
*   **Vulnerable Parameter:** `tfUName` (HTTP POST Request)

## ⚡ Assessment Methodology
1.  **Traffic Interception:** Analyzing baseline web requests and session parameters.
2.  **Vulnerability Scanning:** Performing automated fuzzing to locate injectable parameters.
3.  **Infrastructure Fingerprinting:** Determining the backend database, OS, and web server runtimes.
4.  **Schema Enumeration:** Discovering available database catalogs, tables, and target columns.
5.  **Data Extraction:** Recovering cleartext database table records using safe hex-encoding options.
6.  **Remediation Design:** Developing secure software solutions to eliminate the input risk.

---

## 🛡️ Core Remediation Strategy
To systematically eliminate SQL injection vulnerabilities, software engineers must follow a defense-in-depth model:

### 1. Primary Control: Parameterized Queries (Prepared Statements)
Developers must use parameterized queries. This separates database instruction code from user-controlled input data. It guarantees that any input is treated strictly as a string literal value, never as executable code.

### 2. Secondary Control: Input Validation
Enforce strict server-side allow-listing. Restrict input strings based on expected character sets, precise lengths, and specific regular expressions before they interact with database drivers.

### 3. Least-Privilege Database Permissions
Configure the web application's database service account with minimal access control rights. The account should only have permissions for specific transactional schemas. It must not have administrative access to system tables or dangerous OS execution commands.

---
*Disclaimer: This project was conducted entirely on an authorized, publicly available security testing application designed solely for educational validation and tool diagnostics.*

