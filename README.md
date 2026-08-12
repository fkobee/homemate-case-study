# 🏠 HomeMate

**Smart roommate matching for students moving to Montevideo**

HomeMate is a mobile application designed to help students find compatible roommates based not only on budget or location, but also on **habits, lifestyle, interests and personal compatibility**.

The project was developed as the final degree project for the **Bachelor's Degree in Information Systems at Universidad ORT Uruguay**.

> Finding someone to live with should not be a matter of luck.

---

## About the Project

Moving to a new city to study often means living away from home for the first time and, in many cases, sharing accommodation with people you do not know.

Traditional housing platforms mainly focus on properties, price and location. HomeMate approaches the problem from a different perspective:

**Who would I actually be compatible living with?**

The platform was designed around four main pillars:

* **Smart Matching** — compatibility based on structured and semantic signals.
* **Housing Discovery** — connecting people with compatible housing needs.
* **Trust & Safety** — identity verification and transparent profiles.
* **Community** — a space where users can interact, share experiences and connect.

---

## Tech Stack

### Mobile

* React Native
* Expo
* TypeScript

### Backend

* C#
* ASP.NET Core Web API
* Entity Framework Core
* JWT Authentication

### Database

* PostgreSQL
* pgvector

### Matching & AI

* Vertex AI
* Vector embeddings
* Cosine similarity
* Jaccard similarity
* Weighted compatibility scoring

### Infrastructure

* Docker
* DigitalOcean App Platform
* Amazon S3
* GitHub Actions

---

## Architecture

HomeMate follows a client-server architecture where the mobile application communicates with an ASP.NET Core REST API responsible for business logic, authentication, persistence and the matching process.

```text
                    ┌─────────────────────────┐
                    │   React Native + Expo   │
                    │       Mobile App        │
                    └────────────┬────────────┘
                                 │
                              REST API
                                 │
                    ┌────────────▼────────────┐
                    │     ASP.NET Core API    │
                    │                         │
                    │   Business Logic        │
                    │   Authentication        │
                    │   Matching Engine       │
                    │   Community             │
                    │   Chat                  │
                    └───────┬─────────┬───────┘
                            │         │
                 ┌──────────▼───┐ ┌──▼─────────────┐
                 │ PostgreSQL   │ │ Cloud Services │
                 │ + pgvector   │ │                │
                 │              │ │ Vertex AI      │
                 │ Relational   │ │ Amazon S3      │
                 │ + vector data│ │                │
                 └──────────────┘ └────────────────┘
```

PostgreSQL was selected as the main relational database, while **pgvector** allows vector embeddings to coexist with traditional relational information in the same database.

This makes it possible to combine standard filtering with semantic similarity searches without introducing a separate vector database.

---

## Matching Engine

The matching system is one of HomeMate's main technical components.

Rather than calculating compatibility using a single questionnaire or simple predefined rules, the system combines several independent signals to generate a final compatibility score.

### Structural Filtering

The first stage applies objective constraints to determine whether two users are viable candidates.

Filtering candidates before performing more complex calculations reduces unnecessary processing and ensures that incompatible profiles are excluded early.

### Semantic Compatibility

Information such as questionnaire responses and user biographies can contain meaning that is difficult to represent using predefined categories.

HomeMate transforms this information into **vector embeddings** using Vertex AI.

These vectors are stored in PostgreSQL through pgvector and compared to estimate semantic similarity between users.

```text
User Information
      │
      ▼
   Vertex AI
      │
      ▼
Vector Embedding
      │
      ▼
PostgreSQL + pgvector
      │
      ▼
Vector Similarity
```

### Interests

Shared interests are evaluated using **Jaccard similarity**, comparing the intersection of both users' interests with their combined set of interests.

### Habits

Lifestyle and coexistence habits are represented through measurable traits that allow the system to compare characteristics relevant to everyday shared living.

### Final Compatibility Score

The different compatibility channels are combined using configurable weights.

```text
Structural Filtering
        │
        ▼
Candidate Preselection
        │
        ▼
Compatibility Analysis
        │
        ├── Questionnaire Embeddings
        ├── Biography Embeddings
        ├── Interests
        └── Habits
        │
        ▼
Weighted Scoring
        │
        ▼
Compatibility Score
       0–100%
```

The approach was designed to provide:

* **Efficiency** by filtering candidates before ranking.
* **Scalability** by limiting expensive similarity calculations.
* **Flexibility** through configurable weights.
* **Robustness** when some profile information is unavailable.
* **Explainability** through multiple identifiable compatibility dimensions.

---

## Main Features

### User Profiles

Users create profiles containing information relevant to shared living, including personal information, preferences, habits and interests.

This information becomes part of the compatibility analysis performed by the matching engine.

### Discover

Users can explore potential roommates and see a compatibility percentage calculated from the different matching signals.

The objective is not simply to recommend people who are similar, but to identify people whose **living preferences may be compatible**.

### Community

HomeMate includes a community section where users can:

* create publications;
* comment and reply;
* like content;
* bookmark publications;
* exchange recommendations and experiences.

### Private Chat

Users can communicate through private conversations, allowing potential roommates to get to know each other before making decisions about living together.

### Identity Verification

Trust was identified as an important concern during the product research process.

For this reason, HomeMate includes an identity verification flow designed to increase transparency and confidence between users.

---

## Product Discovery

HomeMate was developed using a user-centered approach rather than starting directly from a predefined technical solution.

The discovery process included:

* surveys with **110 participants**;
* **14 interviews**;
* secondary research;
* competitor benchmarking;
* user personas;
* brainstorming;
* SCAMPER;
* effort-impact prioritization.

Several recurring problems emerged during the research process:

* uncertainty associated with moving away from home;
* differences in routines and coexistence habits;
* distrust when considering living with strangers;
* difficulty evaluating potential roommates beforehand;
* the importance of compatibility when choosing who to live with.

These findings directly influenced both the product functionality and the design of the matching system.

---

## Validation

The application was continuously validated throughout the project.

The process included:

* interactive Figma prototypes;
* Think Aloud sessions;
* usability testing;
* validation of functional increments;
* iterative improvements based on user feedback;
* testing on real devices;
* satisfaction and usability surveys.

Instead of validating the product only after development, feedback was incorporated throughout multiple iterations.

This allowed usability issues and incorrect assumptions to be identified earlier in the process.

---

## Quality Engineering

Software quality was considered throughout the complete development lifecycle.

The project used **ISO/IEC 25010** as a reference framework for defining relevant quality attributes.

Development practices included:

* SOLID principles;
* REST conventions;
* Clean Code practices;
* ESLint for the React Native frontend;
* Definition of Done;
* Pull Request reviews;
* automated testing;
* continuous integration.

Different testing strategies were incorporated into the project, including:

* unit testing;
* functional testing;
* integration testing;
* usability testing;
* load testing.

### Automated Testing

Backend unit tests were implemented using **MSTest**.

The project established a target of at least **90% unit test coverage**.

### CI/CD

GitHub Actions was used to automate validation of the application.

```text
Push / Pull Request
        │
        ▼
      Build
        │
        ▼
    Run Tests
        │
        ▼
Generate Coverage
        │
        ▼
Validate Coverage
        │
        ▼
     Deploy
```

The workflow included automated test execution, coverage validation, Pull Request checks and continuous deployment.

---

## Infrastructure

### DigitalOcean

The backend was deployed using **DigitalOcean App Platform**, allowing the team to deploy application changes quickly and maintain an accessible environment for real-user validation.

### Amazon S3

Images and documents are stored using Amazon S3 instead of being persisted directly in the relational database.

Temporary URLs are used to control access to stored resources.

This approach separates binary storage from relational information while keeping the backend responsible for access control.

### Docker

Docker was used to provide consistent development environments and reduce dependency and operating-system compatibility issues between team members.

---

## Development Process

HomeMate followed an **iterative and incremental development process based on Scrum**.

Development was organized around:

* user stories;
* a prioritized backlog;
* two-week sprints;
* Sprint Planning;
* reviews;
* retrospectives;
* continuous requirement refinement.

Requirements were allowed to evolve as new information emerged from user research and validation.

The project started with an estimated scope of **300 Story Points** and finished with approximately **315 Story Points**, representing a controlled increase in scope during development.

---

## Project Outcome

The project resulted in a functional MVP integrating the complete product experience, including user management, compatibility analysis, community functionality, communication between users and supporting infrastructure.

The final product was validated with real users throughout the development process.

The project also provided practical experience integrating different areas of software engineering:

* product discovery;
* requirements engineering;
* mobile development;
* backend development;
* database design;
* recommendation and matching systems;
* cloud infrastructure;
* automated testing;
* CI/CD;
* usability validation;
* agile project management.

---

## Future Work

Potential next steps identified for HomeMate include:

* improving the onboarding experience;
* further refinement of the matching algorithm;
* publication on the App Store and Google Play;
* user acquisition and marketing initiatives;
* strategic partnerships;
* additional work related to personal data regulation and compliance.

---

## Team

HomeMate was developed by:

* **Lucas Medina**
* **Martina Roll**
* **Tiago Abenante**
* **Franco Cáceres**

as the final degree project for the **Bachelor's Degree in Information Systems at Universidad ORT Uruguay**.

---

## Source Code

The application source code is maintained in a **private team repository**.

This public repository serves as a technical case study documenting the product, architecture, engineering decisions, matching strategy, validation process and development practices behind HomeMate.
