# Fraud, Waste & Abuse (FWA) Detection Engine

[![Medium](https://img.shields.io/badge/Medium-Deep_Dive-black?style=flat&logo=medium)](https://medium.com/@alessandro.oof/detecting-medicare-fraud-at-scale-building-an-upcoding-unbundling-detection-engine-on-230-4de555db568d)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Profile-blue?style=flat&logo=linkedin)](https://linkedin.com/in/aosousa)
[![Companion Project](https://img.shields.io/badge/Companion-Aberrant_Billing-green?style=flat)](https://github.com/sousa-a/provider-aberrant-billing-detection)
[![Companion Project](https://img.shields.io/badge/Companion-Phantom_Billing-green?style=flat)](https://github.com/sousa-a/medicare-phantom-billing-engine)

---

**Upcoding & Unbundling Detection on CMS DE-SynPUF Medicare Claims**

---
**Project P1** - Part of a three-project Medicare Fraud, Waste & Abuse (FWA) portfolio.

An automated, data-driven analytical pipeline built to detect the two most prevalent forms of healthcare billing fraud-Upcoding and Unbundling-across 230 million synthetic Medicare claims.

This project demonstrates how to architect a production-grade detection engine using in-process OLAP databases and unsupervised machine learning, operating efficiently on a standard local machine without the need for distributed computing clusters (e.g., Spark).
Tech Stack & Data

    Language: Python 3.11+
    Analytical Engine: DuckDB (In-process, columnar SQL execution)
    Machine Learning: scikit-learn (IsolationForest)
    Data Source: CMS 2008-2010 Data Entrepreneurs' Synthetic Public Use File (DE-SynPUF) - All 20 samples (~35GB raw CSVs).

Detection Modules

The pipeline evaluates providers using a multi-stage rule and anomaly detection system:

    M1 - DRG Upcoding (Facilities): Analyzes inpatient claims by computing a Case Mix Index (CMI) per provider using official CMS FY2010 MS-DRG relative weights. Flags providers deviating significantly from the population mean.

    M2 - E&M Upcoding (Physicians): Analyzes carrier claims to identify abnormal concentration ratios of high-level Evaluation & Management codes (CPT 99214 and 99215) in established patient office visits.

    M3 - NCCI Unbundling: Scans across 177+ million carrier and outpatient line items using the complete 753,962-pair CMS NCCI Procedure-to-Procedure (PTP) edit file. Detects both within-claim and cross-claim (same-day) prohibited code pairs.

    M4 - Composite Risk Scoring: Fuses the outputs of M1-M3 using an IsolationForest algorithm with data-driven contamination thresholds. Generates a final, scaled FWA risk score (0-100) and assigns providers to risk tiers.

    Read the Deep-Dive

For a comprehensive breakdown of the methodology, SQL query optimization, and the transition from heuristic scoring to Isolation Forest anomaly detection, read the full write-up on Medium:
Detecting Medicare Fraud at Scale: Building an Upcoding & Unbundling Detection Engine on 230 Million Claims.

https://medium.com/@alessandro.oof/detecting-medicare-fraud-at-scale-building-an-upcoding-unbundling-detection-engine-on-230-4de555db568d

Important Caveat

The CMS DE-SynPUF is fully synthetic data. Provider numbers, NPI numbers, and diagnosis/procedure codes have been randomized as part of the CMS disclosure treatment. Multivariate relationships between variables are altered. The results outputted by this engine should not be interpreted as evidence of actual real-world fraud. The value of this repository lies in demonstrating the architectural pipeline, SQL logic, and ML methodology that would be applied to real Identifiable Medicare claims.
## Related projects

This is part of a three-project FWA portfolio:

| Project | Focus | GitHub | Medium |
|---|---|---|---|
| P1 | Upcoding & unbundling detection | https://github.com/sousa-a/medicare-upcoding-unbundling-engine | https://medium.com/@alessandro.oof/detecting-medicare-fraud-at-scale-building-an-upcoding-unbundling-detection-engine-on-230-4de555db568d |
| P2 | Phantom billing detection | https://github.com/sousa-a/medicare-phantom-billing-engine | https://medium.com/@alessandro.oof/detecting-medicare-phantom-billing-at-scale-building-a-post-mortem-claims-impossible-service-day-c8d57a6b9c3c |
| P3 | Provider aberrant billing pattern detection | https://github.com/sousa-a/provider-aberrant-billing-detection | https://medium.com/@alessandro.oof/detecting-aberrant-medicare-billing-patterns-a-multi-method-framework-using-synthetic-cms-data-5660d71e2839 |

These projects used data from CMS 2008-2010 Data Entrepreneurs’ Synthetic Public Use File (DE-SynPUF).
https://www.cms.gov/data-research/statistics-trends-and-reports/medicare-claims-synthetic-public-use-files/cms-2008-2010-data-entrepreneurs-synthetic-public-use-file-de-synpuf

---

## Author

**Alessandro Oliveira de Sousa**  
August 2026
