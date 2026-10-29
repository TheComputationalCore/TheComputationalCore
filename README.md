<p align="center">
  <img src="assets/banner.svg" alt="Dinesh Chandra — small-data machine learning with audited evidence" width="100%">
</p>

<h1 align="center">Dinesh Chandra</h1>

<p align="center">
  <strong>Mechanical engineer building audited machine learning and full-stack systems.</strong>
</p>

<p align="center">
  B.Tech Mechanical Engineering, Vardhaman College of Engineering, Hyderabad<br>
  CGPA 9.74/10 · 2nd rank · First Class with Distinction · Oral presentation, ICAMS 2020
</p>

<p align="center">
  <a href="mailto:dineshchandra962@gmail.com"><img src="https://img.shields.io/badge/Email-dineshchandra962%40gmail.com-D14836?style=flat-square&logo=gmail&logoColor=white" alt="Email"></a>
  <a href="https://doi.org/10.1088/1757-899X/998/1/012061"><img src="https://img.shields.io/badge/Paper-10.1088%2F1757--899X%2F998%2F1%2F012061-1f6feb?style=flat-square" alt="Source paper DOI"></a>
  <a href="https://www.youtube.com/@TheComputationalCore"><img src="https://img.shields.io/badge/YouTube-TheComputationalCore-FF0000?style=flat-square&logo=youtube&logoColor=white" alt="YouTube"></a>
  <a href="https://github.com/TheComputationalCore/Material-Hardness-Oxidation-Prediction"><img src="https://img.shields.io/badge/Research-Material%20Prediction-6DB33F?style=flat-square" alt="Material prediction repository"></a>
</p>

I work at the boundary between experimental mechanical engineering and software: small-data machine learning that states its limits, and Spring Boot/React systems with tests, CI, and containers. My first-author TIG-hardfacing study is the origin of that work; the repositories below show the audit, validation, and engineering that followed.

---

## Evidence at a glance

| Research | Source audit | Code | Delivery |
|:--|:--|:--|:--|
| First-author IOP conference paper (2020) | 24 / 24 source values and table labels matched | 46 Python tests · 11 Java test classes | 10 live demos returning HTTP 200 |

## What I work on

| I build | I care about |
|:--|:--|
| Small-data machine learning for materials and mechanical systems | Audited inputs, grouped validation, stated uncertainty |
| Spring Boot and React systems | Tests, CI, containers, explicit deployment status |
| Interfaces for technical work | Showing the measured value and the model's limits, not just a number |

---

## Research flagship: Material Hardness & Oxidation Prediction

[Repository](https://github.com/TheComputationalCore/Material-Hardness-Oxidation-Prediction) · [Live application](https://material-hardness-oxidation-prediction.onrender.com/) · [Source audit](https://github.com/TheComputationalCore/Material-Hardness-Oxidation-Prediction/blob/main/docs/SOURCE_AUDIT.md) · [Model card](https://github.com/TheComputationalCore/Material-Hardness-Oxidation-Prediction/blob/main/docs/MODEL_CARD.md) · [Reproducibility](https://github.com/TheComputationalCore/Material-Hardness-Oxidation-Prediction/blob/main/docs/REPRODUCIBILITY.md) · [API reference](https://github.com/TheComputationalCore/Material-Hardness-Oxidation-Prediction/blob/main/docs/API_REFERENCE.md)

A reproduction and extension of my published TIG-hardfacing experiment: predict Vickers hardness and cumulative areal mass gain for Stellite-6 deposited on EN-8 and mild steel at 120, 140, and 160 A.

> **C. Dinesh Chandra**, B. Rushikesh, Mohammed Numan, B. Venkatesh.
> *Experimental Studies of Stellite-6 Hardfaced Layer on Ferrous Materials by TIG Surfacing Process.*
> IOP Conference Series: Materials Science and Engineering 998 (2020) 012061.
> [DOI](https://doi.org/10.1088/1757-899X/998/1/012061) · CC BY 3.0

> **Scope, stated up front.** Two substrates × three currents give six hardness values. The eighteen oxidation rows reduce to six effective Material × Current conditions, because the 10 h and 15 h values are near-exact multiples of the 5 h value. No replicate specimens or uncertainty estimates are published. This is a research reproduction, not an independently validated engineering predictor.

### What was built

- **Source audit.** Every CSV value and source-table label is compared with the paper. Arithmetic, exposure-time structure, units, sign, exposed area, and observation independence are checked and recorded.
- **Grouped validation.** Leave-one-current-out is the primary protocol and leave-one-condition-out is secondary. Random row splits are never used.
- **Pre-registered model selection.** A challenger must improve primary MAE by at least 20%, win at least two of three folds, and not worsen secondary MAE by more than 10%.
- **Uncertainty and applicability.** Approximate 90% intervals come from signed out-of-fold residuals. Every response reports whether the input is a reported condition, an unmeasured combination within tested levels, or outside the tested domain.
- **Reproducibility gates.** Dataset and artifact hashes, manifests, CI retraining, calibration, benchmark checks, and external-validation checks are part of the build.
- **External measurement path.** New raw weights and measured areas are validated separately and are never merged into training automatically.

### Validation and results

| Target | Validation | MAE | RMSE | R² | Honest reading |
|:--|:--|--:|--:|--:|:--|
| Hardness | Leave-one-current-out | 26.25 HV | 27.51 HV | −0.656 | Worse than predicting the mean; kept as a transparent baseline. |
| Oxidation | Leave-one-current-out | 1.346 × 10⁻⁴ | 1.631 × 10⁻⁴ | 0.9936 | Strong on six effective conditions; not 18 independent observations. |

Oxidation values are shown in the paper's printed unit. The unit itself is unresolved.

### What the audit found

| Audit check | Finding |
|:--|:--|
| Values and table labels | **24 / 24 match** the paper |
| Heat-input arithmetic | `Q = V · I / v` reproduces 0.800, 0.864, and 0.912 kJ/mm within 5.5 × 10⁻⁴ |
| Exposure-time structure | 11 exact and 1 near multiple → **6 effective conditions**, not 18 |
| Printed oxidation unit | **Unresolved**: literal g/mm² implies a 4.6–17.1% specimen mass change |
| Printed sign | **Unresolved**: `w1 − w2` implies mass loss, but every value is positive |
| Exposed area | **Unresolved**: 225 mm² in Table 7 versus 900 mm² in the methods |

### What I do not claim

- That the hardness model beats the mean. Under leave-one-current-out it does not (R² = −0.656).
- That eighteen oxidation rows are eighteen independent kinetic observations. They reduce to six effective conditions.
- That welding current is a causal variable. Current, travel speed, and heat input move together as one process schedule.
- That the printed oxidation unit or sign has been resolved. Both remain open, and no correction is applied.
- That predictions generalize beyond 120–160 A on EN-8 and mild steel. The service rejects unsupported levels rather than extrapolating.

### Verified live prediction

EN-8, 140 A, 10 h, with model versions 0.2.0 / 0.3.0:

| Output | Estimate | 90% interval | Reported value |
|:--|--:|--:|--:|
| Hardness | 355.7 HV | 338.2–392.2 HV | 365.0 HV (Table 6) |
| Oxidation | 0.004708 | 0.004361–0.004969 | 0.004592 (Table 11) |

The API returns `reported_condition` applicability plus warnings for the unresolved unit, sign, independence, and Vickers test load. Verified against the live endpoint on 2026-10-29.

---

## Systems engineering

The same discipline applied to application software: explicit layering, tests, CI, containers, and an honest deployment status.

### [employee-management](https://github.com/TheComputationalCore/employee-management)

Employee lifecycle workflows: recruitment, onboarding, attendance, leave, payroll, performance reviews, and analytics, with admin, HR, and employee roles.

`Spring Boot 4.1.1` · `Thymeleaf` · `Spring Security` · `PostgreSQL` · `Docker`

**Verified:** CI on Java 17 and 21 · CodeQL · Dependabot · 6 test classes. The Render deployment is currently offline; the repository and CI are live.

### [clinic-management-system](https://github.com/TheComputationalCore/clinic-management-system)

Role-based clinic workflow for administrators, doctors, and patients: appointments, prescriptions, and BCrypt authentication.

`Spring Boot 4.1.1` · `Spring Security 6` · `JPA/Hibernate` · `PostgreSQL` · `Thymeleaf` · `Docker Compose`

**Verified:** CI and CodeQL on `main` · Dependabot · 2 test classes, including Mockito service tests for registration validation. No public demo.

### [sarathi](https://github.com/TheComputationalCore/sarathi) · [live demo](https://sarathi-frontend-3zg8.onrender.com/)

A bus-booking demo built around heritage routes and circuits, with real-time seat locking over WebSockets.

`Spring Boot 3.3.10` · `React 18` · `PostgreSQL` · `Redis` · `STOMP/SockJS` · `Leaflet` · `Docker`

**Verified:** CI builds the Maven backend, the React frontend on Node 20, and the Docker image · 3 test classes · live demo returns 200. The test suite is small relative to the feature surface.

### [WindTurbineBladeOptimization](https://github.com/TheComputationalCore/WindTurbineBladeOptimization) · [live demo](https://windturbinebladeoptimization.onrender.com/)

A Random Forest pipeline over a synthetic blade dataset, predicting deformation, stress, strain, factor of safety, fatigue life, and damage index through a Flask interface.

`Python` · `scikit-learn` · `pandas` · `NumPy` · `Flask`

**Verified:** Python CI · 4 tests · live demo returns 200. The dataset is synthetic, so this demonstrates the pipeline rather than validating a physical blade design.

<details>
<summary><strong>Earlier iterations</strong></summary>

- [employee-management-api](https://github.com/TheComputationalCore/employee-management-api) — Spring Boot + MongoDB + Swagger CRUD API; predecessor to the HRMS.
- [patient-system](https://github.com/TheComputationalCore/patient-system) — Spring Boot + Security + Thymeleaf clinic prototype; predecessor to the clinic system.
- [bus-booking-system](https://github.com/TheComputationalCore/bus-booking-system) — React + Spring Boot + PostgreSQL booking app; predecessor to sarathi. Its Render demo is currently offline.

</details>

### Quality gates

| Repository | CI | Tests | Additional gates |
|:--|:--|:--|:--|
| Material | [![CI](https://github.com/TheComputationalCore/Material-Hardness-Oxidation-Prediction/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/TheComputationalCore/Material-Hardness-Oxidation-Prediction/actions/workflows/ci.yml) | 46 pytest tests | Source audit, reproducibility, benchmark |
| employee-management | [![CI](https://github.com/TheComputationalCore/employee-management/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/TheComputationalCore/employee-management/actions/workflows/ci.yml) | 6 test classes | CodeQL, Dependabot |
| clinic-management-system | [![CI](https://github.com/TheComputationalCore/clinic-management-system/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/TheComputationalCore/clinic-management-system/actions/workflows/ci.yml) | 2 test classes | CodeQL, Dependabot |
| sarathi | [![CI](https://github.com/TheComputationalCore/sarathi/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/TheComputationalCore/sarathi/actions/workflows/ci.yml) | 3 test classes | Backend, frontend, and Docker builds |
| WindTurbineBladeOptimization | [![CI](https://github.com/TheComputationalCore/WindTurbineBladeOptimization/actions/workflows/python-app.yml/badge.svg?branch=main)](https://github.com/TheComputationalCore/WindTurbineBladeOptimization/actions/workflows/python-app.yml) | 4 tests | Python CI |

---

## Learning archive

24 public repositories: 1 profile, 2 research/ML projects, 3 flagship Spring services, 3 earlier iterations, and 15 learning repositories.

<details>
<summary><strong>15 learning repositories: Java, Python, and web fundamentals</strong></summary>

### Java and Spring

| Repository | Focus |
|:--|:--|
| [java-basics-control-flow](https://github.com/TheComputationalCore/java-basics-control-flow) | Loops, branching, number processing, pattern printing, and basic I/O |
| [java-oop-fundamentals](https://github.com/TheComputationalCore/java-oop-fundamentals) | Encapsulation, inheritance, and class design |
| [java-oop-interfaces-abstraction](https://github.com/TheComputationalCore/java-oop-interfaces-abstraction) | Interfaces, abstraction, and class relationships |
| [java-advanced-exceptions-collections](https://github.com/TheComputationalCore/java-advanced-exceptions-collections) | Exception handling and the collections framework |
| [java-advanced-streams-lambda-date](https://github.com/TheComputationalCore/java-advanced-streams-lambda-date) | Stream API, lambdas, and date/time handling |
| [SpringIOC](https://github.com/TheComputationalCore/SpringIOC) | Inversion of control, beans, JPA, and a product catalog |
| [sql-jdbc-mongodb-tasks](https://github.com/TheComputationalCore/sql-jdbc-mongodb-tasks) | SQL, JDBC, and MongoDB exercises |

### Python

| Repository | Focus | Live |
|:--|:--|:--|
| [Polygon-Area-Calculator](https://github.com/TheComputationalCore/Polygon-Area-Calculator) | Object-oriented geometry: area, perimeter, diagonals, drawings | [Open](https://thecomputationalcore.github.io/Polygon-Area-Calculator/) |
| [Probability-Calculator](https://github.com/TheComputationalCore/Probability-Calculator) | Monte Carlo probability experiments with a hat of colored balls | [Open](https://thecomputationalcore.github.io/Probability-Calculator/) |
| [Budget-App](https://github.com/TheComputationalCore/Budget-App) | Budget categories, transactions, and percentage charts | [Open](https://thecomputationalcore.github.io/Budget-App/) |
| [Time-Calculator](https://github.com/TheComputationalCore/Time-Calculator) | 12-hour time arithmetic with durations and weekdays | [Open](https://thecomputationalcore.github.io/Time-Calculator/) |
| [Arithmetic-Formatter](https://github.com/TheComputationalCore/Arithmetic-Formatter) | Vertical and side-by-side arithmetic formatting | [Open](https://thecomputationalcore.github.io/Arithmetic-Formatter/) |

### Web fundamentals

| Repository | Focus | Live |
|:--|:--|:--|
| [Iphone-Calculator](https://github.com/TheComputationalCore/Iphone-Calculator) | DOM-based calculator with HTML5, CSS3, and vanilla JavaScript | [Open](https://thecomputationalcore.github.io/Iphone-Calculator/) |
| [html-css-survey-form](https://github.com/TheComputationalCore/html-css-survey-form) | Responsive, accessible survey form with Bootstrap 5 | [Open](https://thecomputationalcore.github.io/html-css-survey-form/) |
| [html-css-basics](https://github.com/TheComputationalCore/html-css-basics) | 11 HTML and CSS tasks covering structure, forms, tables, and semantics | — |

</details>

---

## Stack

**Languages**  
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![HTML/CSS](https://img.shields.io/badge/HTML%2FCSS-E34F26?style=flat-square&logo=html5&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-336791?style=flat-square)

**Machine learning and data**  
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat-square&logo=scipy&logoColor=white)
![joblib](https://img.shields.io/badge/joblib-4B8BBE?style=flat-square)

**Backend**  
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring_Security-6DB33F?style=flat-square&logo=springsecurity&logoColor=white)
![JPA/Hibernate](https://img.shields.io/badge/JPA%2FHibernate-59666C?style=flat-square&logo=hibernate&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=flat-square&logo=flask&logoColor=white)
![REST APIs](https://img.shields.io/badge/REST_APIs-005571?style=flat-square)
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-005F0F?style=flat-square)

**Frontend**  
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![WebSockets](https://img.shields.io/badge/WebSockets-010101?style=flat-square)
![Leaflet](https://img.shields.io/badge/Leaflet-199900?style=flat-square&logo=leaflet&logoColor=white)
![Bootstrap 5](https://img.shields.io/badge/Bootstrap_5-7952B3?style=flat-square&logo=bootstrap&logoColor=white)

**Data and delivery**  
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![CodeQL](https://img.shields.io/badge/CodeQL-1F6FEB?style=flat-square&logo=github&logoColor=white)
![Render](https://img.shields.io/badge/Render-46E3B7?style=flat-square&logo=render&logoColor=white)
![GitHub Pages](https://img.shields.io/badge/GitHub_Pages-222222?style=flat-square&logo=githubpages&logoColor=white)

**Testing and quality**  
![pytest](https://img.shields.io/badge/pytest-0A9EDC?style=flat-square&logo=pytest&logoColor=white)
![JUnit 5](https://img.shields.io/badge/JUnit_5-25A162?style=flat-square&logo=junit5&logoColor=white)
![Mockito](https://img.shields.io/badge/Mockito-78A641?style=flat-square)
![Ruff](https://img.shields.io/badge/Ruff-D7FF64?style=flat-square&logo=ruff&logoColor=black)

**Mechanical engineering foundation**  
TIG surfacing · Vickers hardness · isothermal oxidation · FEA and ANSYS

---

## How I work

1. **Audit first.** Every value, label, unit, and sign is checked before it reaches a model.
2. **Validate by group.** Whole process settings are held out, never random rows.
3. **Pre-register selection.** The adoption rule is fixed before results are inspected.
4. **Make reproducibility executable.** Hashes, manifests, and CI gates, not a promise.
5. **Ship the limits.** Applicability status and warnings travel with every prediction.

This is the same standard I apply to application code: tests, CI, containers, and explicit deployment status.

---

## Currently

- Extending the materials platform toward external measurements and independent validation.
- Deepening backend work in Spring Boot, PostgreSQL, and Redis.
- Open to collaborations in applied machine learning for materials, mechanical engineering, and backend systems.

## Contact

| | |
|:--|:--|
| Email | [dineshchandra962@gmail.com](mailto:dineshchandra962@gmail.com) |
| GitHub | [@TheComputationalCore](https://github.com/TheComputationalCore) |
| YouTube | [@TheComputationalCore](https://www.youtube.com/@TheComputationalCore) |
| Paper | [10.1088/1757-899X/998/1/012061](https://doi.org/10.1088/1757-899X/998/1/012061) |
| Location | Hyderabad, India |

<p align="center"><sub>
Verified 2026-10-29 against the GitHub API, the latest CI runs on <code>main</code>, and the live demo endpoints.
Demos run on free tiers and may cold-start; offline demos are labeled. This profile describes research and portfolio work, not production systems.
</sub></p>
