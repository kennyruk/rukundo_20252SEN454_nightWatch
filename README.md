# Project Night Watch: Near-Earth Asteroid Monitoring and Risk Alert System

**Student:** Rukundo Kenny
**Student ID:** 20252SEN454
**Course:** INSY 8311, Database Development with PL/SQL
**Instructor:** Eric Maniraguha
**University:** Adventist University of Central Africa (AUCA), Faculty of Information Technology
**Academic year:** 2026-2027, Semester I
**Project type:** Individual capstone (Final Examination)
**Domain:** Space science, planetary defense and risk monitoring

---

## 1. Problem Statement

Near-Earth asteroids are tracked by NASA's Jet Propulsion Laboratory (JPL), but the data is spread across separate services: one holds asteroid properties and orbits, another holds predicted close approaches to Earth, and another holds impact-risk assessments. Each service has its own format and update schedule. A student, researcher or analyst who wants to see which asteroids are approaching, how close and how fast they pass, and how risky they are must collect and combine the data by hand.

Project Night Watch consolidates this public data into a structured Oracle database. PL/SQL procedures, functions and triggers classify the risk of each close approach, generate alerts, and keep an audit trail of every change.

## 2. Objectives

**Main objective:** Design and build an Oracle PL/SQL database that consolidates NASA/JPL near-Earth asteroid data and automatically classifies risk and generates alerts for close approaches to Earth.

**Specific objectives:**
1. Collect and clean real near-Earth asteroid, close-approach and impact-risk data from NASA/JPL sources.
2. Normalize the data into third normal form (3NF) tables with clear relationships.
3. Develop PL/SQL procedures, functions, cursors and packages that load data, classify risk and produce monitoring reports.
4. Implement triggers, a restriction rule and an audit log so that alerts are generated automatically and every change to risk data is traceable.
5. Build an interactive layer (a scheduled nightly monitoring job) that runs on the Oracle database and creates alerts without manual action.

## 3. Data Source

All data comes from NASA's Jet Propulsion Laboratory (JPL) Solar System Dynamics group and the Center for Near-Earth Object Studies (CNEOS). It is public scientific data about space objects and contains no personal information.

| Dataset | Content | Source |
|---|---|---|
| **Primary: JPL Small-Body Database (SBDB)** | Asteroid identity, size, absolute magnitude, NEO and PHA flags, orbit class, orbital elements | https://ssd.jpl.nasa.gov/sbdb_query.cgi |
| Kaggle "Asteroid Dataset" (JPL SBDB export) | CSV copy of SBDB data, 958,524 rows and 45 columns; filtered to near-Earth objects for this project | https://www.kaggle.com/datasets/sakhawat18/asteroid-dataset |
| Supporting: CNEOS Close Approach Data | Date, distance and relative velocity of each close approach | https://cneos.jpl.nasa.gov/ca/ |
| Supporting: Sentry impact risk | Impact probability, Palermo and Torino scale ratings | https://cneos.jpl.nasa.gov/risk/ |

- **Records:** [fill in: number of NEO records after filtering] asteroids and [fill in] close approaches
- **Format:** CSV files
- **Extraction date:** [fill in]
- **Licence/usage:** [fill in after checking each dataset page]

## 4. Proposed Solution

The system stores asteroid properties, orbits, close approaches and impact risks in related Oracle tables. When a close approach is loaded, PL/SQL logic compares its distance, velocity and the asteroid's size against defined thresholds, assigns a risk level, and creates an alert when a threshold is crossed. A scheduled job repeats this check every night for upcoming approaches.

**Scope note:** Night Watch is a monitoring and analysis database built on public NASA data. It is not a real planetary-defense warning system, and its risk levels are a simplified model for this project.

## 5. Innovation

**Option:** Smart automation
**Tool:** Oracle `DBMS_SCHEDULER`

A scheduled job runs every night, checks upcoming close approaches against the risk thresholds, creates new alerts, and writes a summary to a report table. The business rules stay in PL/SQL. The job only triggers them on a schedule.

## 6. Planned Tables

| Table | Purpose |
|---|---|
| `ASTEROIDS` | Asteroid identity, designation, diameter, absolute magnitude, NEO/PHA flags, orbit class |
| `ORBITAL_ELEMENTS` | Eccentricity, semi-major axis, inclination and epoch for each asteroid |
| `CLOSE_APPROACHES` | Each approach: asteroid, date, distance (AU and lunar distances), relative velocity |
| `IMPACT_RISKS` | Impact probability and Palermo/Torino ratings for asteroids in the Sentry list |
| `RISK_LEVELS` | Lookup table of thresholds and risk labels (for example Low, Watch, High) |
| `ALERTS` | Generated alerts: linked approach, severity, status, timestamp |
| `AUDIT_LOG` | Record of changes to risk data and alerts: who, what, when |

The final table design will be completed in Phase II after normalization to 3NF.

## 7. Planned PL/SQL Features

| Feature | Where it is used |
|---|---|
| **Procedures** | Load and validate asteroid and close-approach data; generate the weekly monitoring report |
| **Functions** | Convert AU to lunar distances; estimate impact energy from diameter and velocity; return the risk level for an approach |
| **Cursors** | Loop through upcoming close approaches to build the watch list |
| **Packages** | Group the loading, classification and reporting code into one `NIGHT_WATCH_PKG` |
| **Triggers** | Create an alert automatically when an approach is closer than the threshold and the asteroid is above the size threshold; write to `AUDIT_LOG` when risk data changes |
| **Restriction rule** | Reject changes to risk thresholds and acknowledged alerts that do not follow the allowed rules, and log the attempt |
| **Exception handling** | Handle missing, duplicate or invalid data rows during loading without stopping the whole load |
| **Scheduler job** | `DBMS_SCHEDULER` runs the nightly risk check |

## 8. Technologies

- Oracle Database (pluggable database) and PL/SQL
- Oracle SQL Developer
- Oracle `DBMS_SCHEDULER`
- GitHub for version control
- CSV data files from NASA/JPL

## 9. Repository Structure

```
rukundo_20252SEN454_nightWatch/
├── README.md
├── docs/
│   ├── research/research_log.md
│   ├── proposal/
│   └── presentation/
└── data/
```

## 10. Keywords

near-Earth objects, asteroids, planetary defense, close approach, impact risk, NASA JPL, Oracle, PL/SQL, triggers, scheduled jobs, auditing

## 11. References

Jet Propulsion Laboratory. (n.d.). *SBDB query API*. NASA/JPL Solar System Dynamics. https://ssd-api.jpl.nasa.gov/doc/sbdb_query.html

Jet Propulsion Laboratory. (n.d.). *Sentry API*. NASA/JPL Solar System Dynamics. https://ssd-api.jpl.nasa.gov/doc/sentry.html

Center for Near Earth Object Studies. (n.d.). *Sentry: Earth impact monitoring*. NASA/JPL. https://cneos.jpl.nasa.gov/risk/

Sakhawat18. (n.d.). *Asteroid dataset* [Data set]. Kaggle. https://www.kaggle.com/datasets/sakhawat18/asteroid-dataset

*Further references (research papers and reports) are listed in `docs/research/research_log.md`.*
