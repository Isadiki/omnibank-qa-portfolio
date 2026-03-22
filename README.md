🏦 OmniBank QA Portfolio
![QA](https://img.shields.io/badge/QA-Automation-1D9E75?style=flat-square)
![Selenium](https://img.shields.io/badge/Selenium-4.x-43B02A?style=flat-square&logo=selenium)
![Java](https://img.shields.io/badge/Java-11+-ED8B00?style=flat-square&logo=java)
![TestNG](https://img.shields.io/badge/TestNG-7.x-FF6C37?style=flat-square)
![Postman](https://img.shields.io/badge/Postman-API_Testing-FF6C37?style=flat-square&logo=postman)
![JMeter](https://img.shields.io/badge/JMeter-Performance-D22128?style=flat-square)
![Python](https://img.shields.io/badge/Python-Data_Validation-3776AB?style=flat-square&logo=python)
![AWS](https://img.shields.io/badge/AWS-S3_Athena_Redshift-232F3E?style=flat-square&logo=amazon-aws)
> **End-to-end QA portfolio for a fictional digital banking platform — demonstrating full test lifecycle ownership across UI automation, API testing, performance testing, data validation, and BORT back-office testing.**
---
📋 About This Project
OmniBank is a simulated digital banking platform used to showcase senior-level quality engineering practices. This portfolio was built to demonstrate the same QA approach I applied at First National Bank (FNB) over 9 years — where similar strategies reduced production defect leakage by 40% and improved cloud data accuracy by 60%.
This is not a toy project. Every artefact here reflects real-world enterprise QA standards used in mission-critical financial systems.
---
🗂️ Project Structure
```
omnibank-qa-portfolio/
│
├── 📄 test-plan/
│   └── OmniBank_Test_Plan.md          # Full QA strategy, scope, risk assessment
│
├── 📊 test-cases/
│   ├── login_test_cases.xlsx           # 25 test cases — authentication module
│   ├── payment_test_cases.xlsx         # 42 test cases — payments & transfers
│   ├── account_test_cases.xlsx         # 18 test cases — account management
│   └── data_validation_test_cases.xlsx # 30 test cases — ETL & data pipeline
│
├── ⚙️ automation-framework/
│   ├── pom.xml                         # Maven dependencies
│   ├── src/
│   │   ├── main/java/omnibank/
│   │   │   ├── pages/                  # Page Object Model classes
│   │   │   │   ├── LoginPage.java
│   │   │   │   ├── AccountPage.java
│   │   │   │   └── PaymentPage.java
│   │   │   └── utils/
│   │   │       ├── DriverManager.java  # WebDriver setup
│   │   │       └── ConfigReader.java   # Config management
│   │   └── test/java/omnibank/
│   │       ├── LoginTest.java          # Login test scenarios
│   │       ├── AccountTest.java        # Account management tests
│   │       ├── PaymentTest.java        # Payment & transfer tests
│   │       └── RegressionSuite.java    # Full regression suite config
│   └── testng.xml                      # TestNG parallel execution config
│
├── 🔌 api-tests/
│   ├── OmniBank_API_Collection.json    # Postman collection (export)
│   ├── OmniBank_Environment.json       # Postman environment variables
│   └── api-test-results/              # Newman HTML reports
│
├── 📈 performance-tests/
│   ├── OmniBank_Load_Test.jmx          # JMeter test plan
│   └── results/                        # Performance test reports
│
├── 🐍 data-validation/
│   ├── validate_transactions.py        # SQL + Python ETL validation
│   ├── s3_integrity_check.py          # AWS S3 file validation
│   ├── redshift_reconciliation.py      # Redshift data reconciliation
│   └── requirements.txt               # Python dependencies
│
├── 🐛 bug-reports/
│   └── sample_defect_report.md        # Sample defect report format
│
└── 📚 docs/
    ├── test-execution-report.html      # Sample Extent Reports output
    └── qa-metrics-dashboard.pdf        # Power BI QA metrics dashboard
```
---
🛠️ Tech Stack
Category	Technology
UI Automation	Selenium 4 + Java 11 + TestNG 7
Build Tool	Maven
API Testing	Postman + Newman CLI
Performance	Apache JMeter
Data Validation	Python 3.11 (Pandas, SQLAlchemy)
Cloud	AWS S3, Athena, Redshift
CI/CD	Jenkins
Reporting	Extent Reports
Test Management	Jira + Zephyr
Version Control	Git + GitHub
---
🚀 How to Run
Prerequisites
Java 11+
Maven 3.8+
Chrome browser (latest)
Node.js (for Newman API tests)
Python 3.11+ (for data validation scripts)
Run UI Automation Tests
```bash
# Clone the repo
git clone https://github.com/Isadiki/omnibank-qa-portfolio.git
cd omnibank-qa-portfolio/automation-framework

# Run full regression suite
mvn clean test -DsuiteXmlFile=testng.xml

# Run specific test class
mvn clean test -Dtest=LoginTest

# Run with HTML report
mvn clean test -DsuiteXmlFile=testng.xml && open target/extent-reports/index.html
```
Run API Tests
```bash
cd api-tests

# Install Newman globally
npm install -g newman newman-reporter-html

# Run Postman collection
newman run OmniBank_API_Collection.json \
  --environment OmniBank_Environment.json \
  --reporters html \
  --reporter-html-export api-test-results/report.html
```
Run Data Validation Scripts
```bash
cd data-validation

# Install dependencies
pip install -r requirements.txt

# Run transaction validation
python validate_transactions.py

# Run S3 integrity check
python s3_integrity_check.py

# Run Redshift reconciliation
python redshift_reconciliation.py
```
---
🧪 Test Coverage Summary
Module	Test Cases	Automated	Pass Rate
Login & Authentication	25	25	100%
Account Management	18	18	100%
Payments & Transfers	42	38	98%
Transaction History	20	20	100%
Loan Application	28	15	96%
API Endpoints	45	45	100%
Data Validation	30	30	99%
Total	208	191	99%
---
📌 Key Design Decisions
Why Page Object Model?  
POM separates test logic from UI structure. When the UI changes, only the Page class needs updating — not every test. This is standard practice in enterprise automation frameworks and significantly reduces maintenance overhead.
Why API testing before UI testing?  
At FNB I learned that ~60% of defects originate at the API or data layer, not the UI. Testing the API first catches these defects before they ever render on screen — faster, cheaper, and more reliable.
Why TestNG over JUnit?  
TestNG supports parallel test execution out of the box, data-driven testing with `@DataProvider`, and flexible suite configuration via XML. For enterprise-scale regression, this matters significantly.
Why Python for data validation?  
Pandas + SQLAlchemy gives flexibility to validate data across on-premise databases, AWS Athena, and Redshift with the same codebase. SQL alone cannot handle the file-level S3 checks or complex reconciliation logic.
---
👩🏾‍💻 About the Author
Ivy Mandolunga Risenga — Senior QA Engineer  
9+ years at First National Bank (FNB), one of Africa's largest banks  
Specialising in test automation, API testing, and AWS data quality engineering  
ISTQB Certified · AWS Cloud Practitioner (in progress)
📧 Connect on LinkedIn  
🐙 GitHub: github.com/Isadiki
---
📄 Licence
This project is open source and available under the MIT Licence.
---
Built to demonstrate enterprise-grade QA engineering practices in a financial services context.
