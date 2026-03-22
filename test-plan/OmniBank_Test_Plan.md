OmniBank Digital Banking Platform — Test Plan
Document Version: 1.0  
Author: Ivy Mandolunga Risenga — Senior QA Engineer  
Date: March 2026  
Status: Approved
---
1. Introduction
1.1 Purpose
This Test Plan defines the quality assurance strategy, scope, approach, and schedule for testing the OmniBank Digital Banking Platform. It covers all testing activities required to validate that the platform meets functional, non-functional, and regulatory requirements before release.
1.2 Project Overview
OmniBank is a digital banking platform providing retail and business customers with access to account management, payments, transfers, loan applications, and financial reporting. The platform operates across web, mobile (iOS/Android), and API channels, backed by AWS cloud infrastructure (S3, Athena, Redshift).
1.3 Objectives
Validate that all functional requirements are implemented correctly across web, mobile, and API layers
Ensure data integrity and accuracy across all backend systems and AWS data pipelines
Verify system performance meets defined SLAs under peak load conditions
Confirm security controls protect customer financial data appropriately
Support UAT sign-off by business stakeholders prior to production release
---
2. Scope
2.1 In Scope
The following modules and test types are included in this test plan:
Modules:
User Authentication & Authorisation (Login, MFA, Password Reset)
Account Management (View balances, statements, account details)
Payments & Transfers (Interbank, EFT, international, scheduled)
Transaction History & Search
Loan Application & Management
Notifications & Alerts
Admin & Back-Office (BORT) Systems
AWS Data Pipelines (S3 ingestion, Athena queries, Redshift reconciliation)
API Layer (REST APIs for all modules)
BI & Reporting Dashboards
Test Types:
Functional Testing
Regression Testing
Integration Testing
API Testing
Performance & Load Testing
User Acceptance Testing (UAT)
Data Validation & ETL Testing
BORT Back-Office Testing
Security Testing (basic)
2.2 Out of Scope
Third-party payment gateway internal logic (black-box only)
Mobile OS-specific hardware testing
Penetration testing (handled by dedicated security team)
Infrastructure provisioning and DevOps pipelines
---
3. Test Strategy
3.1 Testing Approach — Shift Left
Testing begins at the API and data layer before UI testing. This approach, implemented successfully at FNB to reduce defect leakage by 40%, ensures defects are caught at their source rather than discovered late in the cycle.
Layer 1 — API First:  
All REST APIs are validated before UI testing begins. Contract testing, endpoint validation, error handling, and data schema verification are completed in Sprint 1 of each feature cycle.
Layer 2 — Data Validation:  
SQL and Python validation scripts run against the database and AWS data pipelines after each deployment. ETL outputs, Redshift schemas, and S3 data integrity are verified automatically.
Layer 3 — UI Automation:  
Selenium/Java/TestNG automation framework handles regression across all UI modules. Manual exploratory testing focuses on edge cases and new features.
Layer 4 — Performance & Load:  
JMeter performance tests run against staging environment before each major release. Targets: <2s response time under 500 concurrent users.
3.2 Test Automation Strategy
Framework: Selenium 4 / Java / TestNG / Maven
Page Object Model: Yes — all pages abstracted into Page classes
Parallel Execution: Yes — TestNG parallel execution configured
CI/CD Integration: Jenkins pipeline triggers regression suite on every merge to main
Reporting: Extent Reports HTML reports generated per run
API Automation: Postman collections + Newman CLI for API regression
Data Validation: Python (Pandas) + SQL scripts for data layer
3.3 Defect Management
Tool: Jira
Severity Levels: Critical / High / Medium / Low
Priority Levels: P1 / P2 / P3 / P4
SLA: Critical defects resolved within 4 hours. High within 24 hours.
Defect Lifecycle: New → Open → In Progress → Fixed → Retest → Closed
---
4. Test Environments
Environment	Purpose	Owner
DEV	Developer unit testing	Development Team
SIT	System Integration Testing	QA Team
UAT	User Acceptance Testing	Business / QA
STAGING	Performance & pre-prod validation	QA + DevOps
PRODUCTION	Live environment — smoke tests only	QA Lead
AWS Test Environment:
S3 buckets: `omnibank-test-raw`, `omnibank-test-processed`
Athena workgroup: `omnibank-qa`
Redshift cluster: `omnibank-qa-cluster`
---
5. Test Types & Coverage
5.1 Functional Testing
Validate all features work as per business requirements.
Module	Priority	Test Cases	Automation
Login & MFA	P1	25	Yes
Account Overview	P1	18	Yes
Payments & Transfers	P1	42	Yes
Transaction History	P2	20	Yes
Loan Application	P2	28	Partial
Notifications	P3	12	No
Admin / BORT	P2	35	Partial
5.2 API Testing
All REST API endpoints validated using Postman and automated with Newman.
Coverage:
`POST /api/v1/auth/login` — valid credentials, invalid credentials, locked account, MFA flow
`GET /api/v1/accounts/{id}` — valid account, unauthorised access, invalid ID
`POST /api/v1/payments/transfer` — valid transfer, insufficient funds, invalid beneficiary
`GET /api/v1/transactions` — pagination, date filters, amount filters
`POST /api/v1/loans/apply` — valid application, validation errors, duplicate application
Validations per endpoint:
Status codes (200, 201, 400, 401, 403, 404, 500)
Response schema validation
Response time < 500ms
Error message accuracy
Header validation (Content-Type, Authorization)
5.3 Data Validation & ETL Testing
Python and SQL scripts validate data accuracy across all pipeline stages.
Checks performed:
Row count reconciliation between source and target
Null/blank field validation on mandatory columns
Duplicate record detection
Date format and range validation
Referential integrity checks (foreign keys)
Aggregation validation (sum, count, average match reports)
S3 file integrity (file size, record count, schema)
Athena partition validation
Redshift schema and constraint checks
5.4 Performance Testing
JMeter load tests against SIT/Staging environment.
Scenario	Users	Duration	Target Response Time
Login spike	1000	5 min	< 2s
Account overview load	500	15 min	< 1.5s
Payment processing	200	30 min	< 3s
Dashboard load	300	10 min	< 2.5s
5.5 BORT Back-Office Testing
Validation of all back-office reconciliation and operations workflows.
End-of-day settlement reconciliation
Transaction reversal workflows
Account freeze and unfreeze operations
Audit log completeness
Report generation accuracy
5.6 Regression Testing
Automated Selenium suite runs on every code merge. Full regression suite runs before every release.
Smoke suite: 45 critical path tests — runs in 12 minutes
Full regression suite: 280 tests — runs in 45 minutes (parallel execution)
Trigger: Jenkins pipeline on every merge to `main` branch
---
6. Entry & Exit Criteria
6.1 Entry Criteria
Test environment is stable and accessible
All builds deployed to SIT environment
Test data prepared and validated
Test cases reviewed and approved
API documentation available and up to date
6.2 Exit Criteria
100% of P1 test cases executed
95% of P2 test cases executed
Zero open Critical (P1) defects
Zero open High (P2) defects older than 48 hours
Performance targets met in staging
Data validation pass rate ≥ 99%
UAT sign-off received from business stakeholders
---
7. Risk Assessment
Risk	Likelihood	Impact	Mitigation
Test environment instability	Medium	High	Dedicated SIT environment, daily health checks
Late API documentation	High	High	Shift-left — involve QA in sprint planning
Data quality issues in AWS pipelines	Medium	High	Automated validation scripts run on every deployment
Third-party payment gateway changes	Low	High	Isolated integration tests with mocked responses
Performance degradation under load	Medium	High	Early performance testing in SIT before staging
Test data availability	Low	Medium	Synthetic test data generated for all scenarios
---
8. Tools & Technologies
Category	Tool	Purpose
Test Management	Jira + Zephyr	Test cases, defect tracking, test cycles
UI Automation	Selenium 4 / Java / TestNG	Automated regression testing
API Testing	Postman + Newman	API validation and automation
Performance	JMeter	Load and stress testing
Data Validation	Python (Pandas) + SQL	ETL and data pipeline testing
CI/CD	Jenkins	Automated test execution on code merge
Reporting	Extent Reports + Power BI	Test execution reports and QA metrics dashboards
Cloud	AWS (S3, Athena, Redshift)	Data pipeline testing environment
Version Control	GitHub	Test code and documentation
---
9. Test Schedule
Phase	Activity	Duration	Owner
Sprint 1	Test planning, environment setup, test case design	Week 1-2	QA Lead
Sprint 2	API testing, data validation scripting	Week 3-4	QA Team
Sprint 3	UI automation development, functional testing	Week 5-7	QA Team
Sprint 4	Integration testing, BORT testing	Week 8-9	QA Team
Sprint 5	Performance testing, full regression	Week 10	QA Lead
Sprint 6	UAT support, defect closure, exit criteria	Week 11-12	QA + Business
---
10. Roles & Responsibilities
Role	Name	Responsibilities
QA Lead	Ivy Risenga	Test strategy, planning, automation framework, reporting
QA Engineer	TBC	Test case execution, API testing, defect logging
Business Analyst	TBC	Requirements clarification, UAT coordination
Dev Lead	TBC	Defect resolution, environment support
Product Owner	TBC	UAT sign-off, priority decisions
---
11. Deliverables
✅ Test Plan (this document)
✅ Test Cases (Functional, API, Data Validation)
✅ Automated Test Suite (Selenium/Java/TestNG)
✅ API Test Collection (Postman)
✅ Performance Test Scripts (JMeter)
✅ Data Validation Scripts (Python/SQL)
✅ Defect Reports (Jira)
✅ Test Execution Reports (Extent Reports)
✅ QA Metrics Dashboard (Power BI)
✅ UAT Sign-Off Report
---
12. Sign-Off
Role	Name	Signature	Date
QA Lead	Ivy Mandolunga Risenga		March 2026
Dev Lead			
Product Owner			
Business Stakeholder			
---
This document is version controlled. All changes must be reviewed and approved by the QA Lead before implementation.
