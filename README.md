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


## Phase I – Business Process Modeling

**Business process:** Automated near-Earth asteroid monitoring and risk alert generation.

The nightly monitoring cycle retrieves and validates NASA/JPL data, registers or updates asteroid records, evaluates monitoring eligibility, and creates or updates alerts. The active watchlist is maintained as conditions change. Confirmed departures are archived before the corresponding watchlist entries are removed; monitoring summaries and audit records support review and oversight.

**Modeling tool:** Draw.io (diagrams.net) — UML activity diagram with swimlanes.

**Actors:** Data Services, Monitor Engine, Risk Operators, and Administration.

### Phase I deliverables

- [Published activity diagram](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=nightWatch_business_process.drawio&dark=0#R%3Cmxfile%3E%3Cdiagram%20name%3D%22Page-1%22%20id%3D%22UyRVvJqNdsNS2IZtdefz%22%3E7V1Zd6M4Fv41Pif1kBwk9kfHS3fPVHWnK11npudlDgHZVgdDBnCW%2FvUjAcIIhE0MMo7j6qpOLBaD7vbdRVcjdbJ%2B%2FSlynlbfQg%2F5I6h4ryN1OoLQtKFFftCRt2xEg5aZjSwj7GVjynbgHv%2BNskHARjfYQ3E%2Blg0lYegn%2BIkfdMMgQG7CjTlRFL7wpy1C3%2BMGnpwlqg3cu45fH%2F0X9pJVNqqqirI98DPCy1X%2B1eSQnR15cNzHZRRugvwLfXrStedEj1cjqM7TPyM4Ib8DSP%2F7kl22dtiX5N8QrxwvfCkNqbOROonCMMl%2BW79OkE9nnZ%2FPecPR4oUiFCRtLvgO%2FvaS8V%2F%2Fff3zl7v%2F%2FB6Zdw%2B%2F314DpX6b%2FM5x8sYmL37Ba98JyKfblxVO0P2T49JDL4RZyNgqWfvkEyC%2F%2Bs4D8m%2BLGZuEfhiRQ0GYXpyOIS8%2FN%2F90hyK8RgmK8uE4caIkZyCdDiyw77MbkVlG6Z%2F0xCh8RKUjqqHaKrn7bfYCz46%2FyV9gBA0%2FobcKyUuWX83434bO%2Fy332zL%2FWb5kkb3x9rw%2FnFW4dvZeV%2F8qeuA6Tt9vTE4A%2BtOr8AGmTuLQG6DoGbtEdPL7EgJnt%2Ba%2F7iGqjjSc2PQ%2BPzmRsw4Jk5ce5F0TFT85QfMLv%2BSyRV85CKO143e4WYvZQ6%2FZCaXZyO4pmo1s7NfxPbli%2Fo%2B7r3umsOFezWfzwylvoihBryV5y2X2JxQSWYjeyCmrkjYCFtCyC1%2B2ygsauQrO73MNlVxL50r62tDyq5xcey6L228VBfkl1xXv0Ruwrjd2KpiSNuG1gBO5ubATYon1C52%2BubPGPn2vn5H%2FjBLsOvmB%2FGIyP5wialQBD4cLeAt2hA3seBeFz8QApqQgaovMLbGlmYCP9NvvI326n4tqT%2B7h5y7S3VLi3EzB0teLlg9XlpYaO2X784vwja%2BYBN6FMU5wGGSnp3OV%2FfaEkPflYJniXv4AkdKVmkABXbsxdU6mdOMGqJxQAVu50WVJlfrhrLHrIn2xaLTG0xbWuB%2FJ0xok71sY4IQ8E1RQsMTkxXdzlkCmejVsHc1v27f%2BNX0owt1O4q4oq24SMqWZGB5o27oKnNCGmbbNyZuhHM2CGdmNkVdzHPaZtFxK43AT5VRsOFGryS79tvv8Yxglq3AZBo4%2F246WpJKaw%2B05X8PwKZfJv1CSvOUySQnLC72HFs7GT76KZT%2FzkzLNcOd4Hg6W6VdBRSsIS0R%2BiXbOgC6QaRH9I%2BQTpnvmJ7h%2FUmrvpVyJIL5PXFBUV2yEXS1wK1JsxRHhJLxbLti0lJEd5IRCtSxmct6YWZI1lZZ8qdDPVSrM05IK%2FXCpaILoqlIXlOntbDyfiQSFsKk624oQi7uAG1jB7mr%2BuSxkAFjQ5GnY6AeIYNCwqAMT0IEdPwW7SkCFnTx0zVVfZ9iEchtU3DfXb0YnPSFcg4e3KuNZBm6Zu9Y7NzI2l6lYzDNVLCyCeCqKxTyOYpnpM3MmtMCWacMxGEixPIU4SNI51W%2FJX0KbCf0H8p9K6stP6JEbqO84uOuY2XwQNF9GP2Xf2XRw1zFTfBDsuivYcVew667sDRpnrengrmNm80Gw4zJF1ww7nfAbyzRVDRbn5Ofpw3uxEZFcjJ5Ta4JeiNhunjwnQV4WUEJ8ROnq%2B5c%2B7QjL%2FZQBqskjVGioFYRK3kVamASq0m1JkbQ5O1sCBYw8nC1h83wBqQMoFfK9mGqRQnP0qDP0us5QeOwJbR57QtOUxWTyQz1MrD6OvqgJwnyumcB%2BhybRBlUc8gMVZ0BTw5rPLZYO60Cq%2FNI7Cn1L3qNtV6y%2BZln8TTIuyq%2BrkLx4kA5c0DoNKTAfRJM%2BbOK6rchLOoSRP%2FLSesUuwGPq%2FwPTBGaDBcgBY2oIRuq8Twug1S0AUA3OAqgqqPCPatiy9EWHGPF7gMZ8NoMzKGKe6QQaxBJ9EKBxIKM1lWP44ZLxWZ6FUlAU0bScoHzFWdNHzv5Pv%2FURP6XKyg0j6uVcTXp1bzRYZ1Rd4xjVUCDPpqa8HLDaARGT1054JhHxAs%2BZeTK4zMb5ECHWMiAfXfIcNB98SyeWsJg%2Fzg%2BssecJjF6Fd%2BdzlaDsQ3hwEq6xSw7cO0FMfny774kZg7BP%2FmEEK%2FEPy2KyIgKlquVUWczzbhB04syjKgSW6kMwT5PT9LajFE8K9xSFqEXprazkn2pKx9TqIJiaPHz09m96%2FY3OPv6Z3y79MH3lPr3ln7pg8dZ1MrpmTVO42dLvUgf1uxgBL4j7MPXhrpD7SCd5MSrXAuJ99b0da%2F06hZ6XmIaYabBZdlGerdaxmFYUPzCFCPUbWPU7ZcWONPmxZvVcy4RUa1BNdSSP7xJaFiiNLD014rJTzIGrenxXP6T7c2YFkBuVcipbmvrQ5asP60zVhzYs0LGOoz5OtMqhB%2FVRy1xz5U7Rxt96UrvCP8kqQvEq9D3qxvWc2haUSO1LU5lQVuRHk%2B9%2BaR8SakhJXGj29g%2B%2FbMpW1RvDVhX2r2IbJKc0tAts6UXvIHqPDINwegf5eIkfsI8TQUFmXfu4vhPHeJEyHY4fZSsfyAMVaFUzbHR2JakfW7r60QX196etfjpnybVBfSBdftWtZgxB0yEiejVe0OB8bljteUFXhuQFRqhL5G63S2s0uLRrRJ4uNSGOTxT94PG6pud0ya3SBbfxroz%2B4CE7m8Vnemd0U77PrZ2rz51lJgdTURefW37IrqlFActMLNLF0h4iREg2EcXPbkSeNcJOHS9fJZHzF3IJuH7LFvU%2F47gA1%2FRzgtfoS%2F9%2Bu6C4aD90BsUp%2FeMs%2BSpHH6QifQic1RZNDRoe1OUHa4ah%2BDEoZwxKuS5LDj4RDlb32IlwMTgEbnrEHKAr6BXH%2B4B6DxCYFa2WIXAliqwbBm%2BLlGpYsT%2F%2Bhofz96UG7DRrwHTA14DZLFjXO%2FcYUL5dGyQJsUUyHI7ZwprhkMzAMcJjJRwm88nMFJpIY6xq44%2FrdOV2MUJ5yUM5PFTylfotW2eFeGWjA%2FiGgFbF5gBNk1a2bsiPNOvDRprBRW8IyXFJVGbvtC15apD%2FH18OE3RBgFXhF1LV0CWQ1jZN7xCiu6DL00SXRmV9ClAsWQtUdPvM2OeyuglA%2B1jcw7757NLZckoU0t6tY9o8f8v26dgc03nPQYtXOYOMlI63hCKssqO%2FtiLiyi2TreNl5TCgsuA8I3CtOmvvjWwb8jeSXObFJuyiCM9GEapGLeOkWNLiNPIzTsbnrewhzDxWx%2B3VnzFoLorFnyXygvnhclH1fuT6fF50cuvdMBkA3FhE3tnfikOmWuoNLRhjf49qa4wOtuaS8aJsWNRCVJ16NwwWOFqjvrun6ILuKZWUkqpVereqUBbuZoBeprEZOClwcuUN5rAm5bIKYbhKrAglDqYtctL%2FpVs4%2BKVE9s7lCuQrcLCprXo4whpLTa8UfZqg2E1lawZlxSWNDtHoiz91kv4UZPas4B5Vl5e%2Fkl%2FPZdQ576Nh6MIv6ilsxKRWdtjIMAHPSoBpsPfGjTSjEu0EZoUlJYN58whY7Fyr282u3NVNxRypuv18Cy06YCoyZSv8XOlIsSKoKi1dl12rYdbhEusvwFK4WtXYSYNKZocC2JOESpcULtRrASeJWMmUX%2BtzBlipiDf2hJWY2EpPsZm1YpIDsZJNeHBHTBSY2nAxUVN%2BT3i2R8bH4eG2nDhoaIpN66WT8wAwKkLrMEVRiyhc7w5OXU37xVCCMrh9GEqTtqWEeYSU26eJiAsrWJQtoNuvkAYthLVyw%2ByGQYDcxHlg5GqLsxkzVSidziVPnXcAZkSkOX8USh9%2B1zIKS3CwQhFO%2FkgR%2FbSkg%2BCk0GH5r%2BlzwQkF%2F78RbZSt7s02lw4jD0UZ6bihTCmyoQpDwMkudskvKvikb94aa%2BpUFG%2BFY2Wb8i8pWBr3bKmwhAyXX3Kt3ABgV%2BJQrcEZOyVcLGIkBRFZg6i0QfEO8h9SWM6kiA7tVbElOJ81h3I5RN9lrdHumoXWW9C%2Bmy8tjdVwNnIieWXnrXRCrk8avQig3FSMsZ4vEWnyFmpXQMjaiDRdIth2rnoN%2BSV79IYvpX1JSp3gKvKZ3q7i%2Fch2TLpkXk%2BgWKP19pi7N7J8%2FzaO%2B%2FbabLhu7w6WDRtY3kDTVmhgj5xi6Da0YD%2FbRvZcu7IOo3LQU9BxkyhUwm1R6KJ4Z8egAzA742UuTcxrHxVWLKIOZcU9LSjfwA2SOTnhKhbrPUCpfy%2FtknEZLlSwRAGKBA0448167YiSLvVqFmfjYfqg2e5RfSdl6l0bikW1DF%2BYerWi2zBk7b5iddi7h9weP8XVLEtq3sl%2Fwk5XxREBlxwwm0yGuY5MvKa3lOpkmvoO2NlpMu1LjuvsclzXfL2rrcgCCqCNJwxaQgIwSBbghLtdvMtH7b%2Btv6AWHtQodDH%2B5ZB%2Ftv9PyYuQXjAhqi%2BtbB5a7NGYq4OifX7%2FLNMmVdhWHXyeDdBq4qAQEwbekfBWh%2B1F2sZbbE31QcoZSlTXdJs3BArcQ%2Fr00x2KMJlOiluOZR6G3TTK3kHkLe3iF7z2nRTeiVRviQTC9EeBDXkzk38qz3nWidaJmKXQQd3%2BLBaua9sigVMN1Va9FragDgLrZuBQ88Gui5%2BcoBXebCo%2F%2F%2F7L%2FT%2FJ8d%2FuZt%2FHf%2Fz2%2Fb5kb7J789%2FH%2BZW7GgB2DMQd%2BsYvubmj7xyE0TrNPkicvmz3CsUhCuSNa4oomrzm4YYWip0tPnFG68FCaFQaWjF%2Fkdl8Q1qfG%2Fvg%2BiC7rVGwhzYKpxYstPVBVX%2FbVbJ2N3%2FhfJuPM3%2FhGaOXUdEHS6nF9aoKGgfPKE7wMgsW9twqXFR7zZpcsrohWA1MySu%2BtvXjcNn5LqzM6%2FndxyB88VP1vLvlWo%2BsZAhYqdLnt5rNUqRFOLUe%2FZNhVvyccJDKHtT3ZFsOfigvZLGwrPP3QsbeGgeYvKRDt9gZwgfp%2BEJKzfvQFEX4qjHxExAt8l2GRFEGTuCiPe87tH8AAOTXGMh0ENp2MmTCfIGLu%2BGiHy7jbNOYtYODrANCVYxEO8jux5cRikM%2FrVrHcbxB2T6yhOj9ogO9DjQhs%2F7FXrI8OjB0WTFroLRpldgWHgxT1nIMM28PaeaB0iramOej35tw9tGCXtaYbi5pCNUSaAgFTIpxRmnQp8b4lcpOuoDk5iCBY3sg7NgE6hoYfNL4Wl7JAVBapRabSzU005hqTYTuUU8f4vnUp1rlVds1VKzqTEvLz7PM%2F9ByA%2FUh5ObHt69kYOwSBZWuxFCm2FkSnEjN2gumqy6U%2B9w1iA9sOV6HWma1Z7RaoTZbniqB2oKQyRDUBm21pNIntcfbdgXfSujnAKrCevjC1CpUrcTXTZau7p%2BqWiuqNmtLBmDrVRcSoO0hDosg7sh7K9cGA2msmbEmTYZEm%2F8KIgor54n%2B6oWbBx%2FNPtXcG9KsldYqmnOewd1DiFdXU1qVeICv%2FbGktVkGoi1pBcQ7gYU3Pc1%2BHelpenX2edtvsxJHCbMvCLJ8lmq944iOrUoLQYg22vwsqdMjEc%2BQFprQW4UmLkarA%2FEsWf2ZgGgL0G7E%2BzjNUI5DPKBIS%2BwCtjVIq9BtyVXarkt%2F8EP3cVRdlV4lkL4DlgjnrGNjqRpwAIpWuUnbzlLX9f10q7eSvEy7KD8fhErKdD7X1FwAjkEoQ%2B%2BNUNVbSSdUm51SZBEq145HolLVDepApeqtpFOplZN8hCAfEJgp6UG%2Be1pdcZDtYj2yuVAH3xaLUJdnFEu1mpmto%2B1q5S6fKxlLMXk6j1C5y9sqSKOsfrRAiGjnxs9D2RmtwTmWeOrSxNNo5ZKfKxGnyMVxWrV1JHGUGBkzWoGYc6XkhO21nq47VUbENbWUES3aVByPSKoSpCVFnpM4EonNJ7xtVZXWcbmoWP%2Bc5P6OHErUdGUAR%2ByIzC5GaWlXZnDzWrLjEl43JBK%2BVVHJuRL%2BB9tTPa3k50i%2FDj28oORDrzhOsorA45LdAhLJfiIVLkMZah9lZJ%2FWJT5vQCyZ1rVwn7x2%2BsUaks9J7LkfvhwNWwNFlVZjIdqC7POQ8c%2BsrtoNAw9nSyOUJNqgwwrQDtHHQGGBcwmk%2FdTRqV%2FDKmUXjh8flbTm%2B7uukY9RSJcabOOU5P1W30KPdtWe%2FR8%3D%3C%2Fdiagram%3E%3C%2Fmxfile%3E)
- [Business process diagram (PDF)](docs/business_process/business_process_diagram.pdf)
- [Business process explanation (PDF)](docs/business_process/business_process_explanation.pdf)
- [Editable Draw.io source](docs/business_process/business_process_diagram.drawio)
