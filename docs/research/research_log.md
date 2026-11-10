# Research Log: Project Night Watch

Student: Rukundo Kenny (ID 20252SEN454)
Course: INSY 8311, Database Development with PL/SQL

Source 1: Chesley et al. (2002)
Type: Research paper
Title: Quantifying the risk posed by potential Earth impacts
Organization: NASA Jet Propulsion Laboratory, University of Pisa, IASF-CNR (published in Icarus, 159(2), 423-432)
Link: https://cneos.jpl.nasa.gov/doc/chesley_impact_paper.html

Key Findings: The paper introduces the Palermo Technical Impact Hazard Scale, which compares the probability and energy of a potential impact with the background risk, so that specialists can rank and prioritise potential impacts.

Relevance to Night Watch: The scale provides a scientific basis for the risk levels used in the database. It supports the risk classification function and the thresholds stored in the RISK_LEVELS table.

Research Gap: The paper defines a risk metric but not a stored, queryable system. Night Watch will apply a simplified risk classification inside an Oracle database and generate alerts from it.

Source 2: Roa, Farnocchia and Chesley (2021)
Type: Research paper
Title: A novel approach to asteroid impact monitoring
Organization: NASA Jet Propulsion Laboratory (published in The Astronomical Journal)
Link: https://arxiv.org/abs/2108.03201

Key Findings: The paper describes the method behind JPL's Sentry-II impact monitoring system, which finds possible impact trajectories by treating the impact condition as an observation in orbit determination.

Relevance to Night Watch: It explains how the impact probability and risk data that Night Watch stores are produced, and why monitoring asteroid risk is an ongoing, automated task.

Research Gap: The paper targets orbit-determination specialists. Night Watch will not recompute impact probabilities. It will store Sentry's published results and build alerts, reports and an audit trail on top of them.

Source 3: National Science and Technology Council (2023)
Type: Report
Title: National Preparedness Strategy and Action Plan for Near-Earth Object Hazards and Planetary Defense
Organization: Office of Science and Technology Policy, United States
Link: https://bidenwhitehouse.archives.gov/wp-content/uploads/2023/04/2023-NSTC-National-Preparedness-Strategy-and-Action-Plan-for-Near-Earth-Object-Hazards-and-Planetary-Defense.pdf

Key Findings: The report updates the 2018 US strategy and sets out how detection, research, preparedness and coordination for near-Earth objects should be improved.

Relevance to Night Watch: It shows that tracking near-Earth objects and assessing their risk is a recognised national priority, which supports the need for monitoring tools.

Research Gap: The report is a policy document. It does not provide a simple data tool for tracking close approaches and risks, which is the niche Night Watch fills at a small scale.

Source 4: ESA NEO Coordination Centre (2025)
Type: Statistic
Title: Newsletter December 2025
Organization: European Space Agency
Link: https://neo.ssa.esa.int/documents/20126/419169/Newsletter+December+2025.pdf

Key Findings: The newsletter reports more than 40,000 known near-Earth asteroids (40,157 asteroids and 124 comets) and 1,867 objects on the risk list.

Relevance to Night Watch: The growing number of known objects shows that the data volume is large and increasing, which justifies a structured database.

Research Gap: The newsletter gives totals, not records that a user can query. Counts also differ between NASA and ESA and by date, so Night Watch will state its data source and extraction date.

Source 5: Center for Near Earth Object Studies (n.d.)
Type: Existing system
Title: Sentry: Earth Impact Monitoring (Impact Risk Data)
Organization: NASA Jet Propulsion Laboratory
Link: https://cneos.jpl.nasa.gov/risk/

Key Findings: Sentry continually scans the current asteroid catalogue for possible Earth impacts over the next 100 years. It publishes each object's impact probability and its Palermo and Torino scale ratings.

Relevance to Night Watch: Sentry is the closest existing system and a data source for the IMPACT_RISKS table. Night Watch's risk levels follow the same scales.

Research Gap: Sentry is a read-only public table and API. It has no user-defined alert rules, scheduled monitoring or audit trail. Night Watch adds these with PL/SQL triggers, procedures and a scheduled job.

Source 6: Sakhawat18 (n.d.)
Type: Dataset
Title: Asteroid Dataset
Organization: Kaggle (data originally from NASA JPL Small-Body Database)
Link: https://www.kaggle.com/datasets/sakhawat18/asteroid-dataset

Key Findings: The dataset is a JPL Small-Body Database export with about 958,000 rows and 45 columns, including NEO and PHA flags and orbital and physical fields.

Relevance to Night Watch: It is the main data source for the ASTEROIDS and ORBITAL_ELEMENTS tables, after filtering to near-Earth objects.

Research Gap: The data is one flat file that is not normalised. Phase II must split it into 3NF tables and link it to close-approach and risk data.

Source 7: Jet Propulsion Laboratory (n.d.)
Type: Technical source
Title: SBDB Query API
Organization: NASA/JPL Solar System Dynamics
Link: https://ssd-api.jpl.nasa.gov/doc/sbdb_query.html

Key Findings: The documentation explains how to request asteroid and comet data that match chosen constraints, with selectable fields, in JSON format.

Relevance to Night Watch: It shows how the official source data is structured and how it can be refreshed, which guides the data loading design.

Research Gap: The output is flat JSON. Night Watch needs a loading step that validates it and inserts it into related Oracle tables.

Source 8: Center for Near Earth Object Studies (n.d.)
Type: Technical source
Title: Palermo Technical Impact Hazard Scale
Organization: NASA Jet Propulsion Laboratory
Link: https://cneos.jpl.nasa.gov/sentry/palermo_scale.html

Key Findings: The scale is the base-10 logarithm of relative risk (PS = log10 R, where R = PI / (fB x DT)). Values below -2 have no likely consequences, values from -2 to 0 merit careful monitoring, and positive values merit concern.

Relevance to Night Watch: These ranges are a ready model for the Low, Watch and High levels in the RISK_LEVELS table and for the classification function.

Research Gap: The page explains the scale but gives no implementation. Night Watch will implement a simplified classification as PL/SQL functions and triggers.
